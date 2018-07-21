---
layout: post
title: "Thinking About HAL Clients"
date: 2013-11-05 22:17
---
More and more, [at work](http://context.io), we're talking about service
oriented architectures. Mostly, these are very small apps that deal with just a
few concerns. Since we foresee having lots and lots of these little
mini-services, we decided we'd need to make some decisions up front about how
they're inter-operate. This is sort of in the spirit of convention over
configuration.

More to avoid having to re-make the decision about what shape responses from
these services should take every time we start a new one than for any other
reason, we decided that all these services should speak
[HAL](http://stateless.co/hal_specification.html). This way, we can get good at
dealing with HAL and once you grok how to talk to one service, that knowledge
will transfer readily to others. We picked HAL over other formats we considered
basically by typing out an example response off the top of our heads and then
seeing which existing format looked the closest. So scientific.

Since that time, I've spent a fair amount of time monkeying with HAL and related
hypermedia issues. There are a lot of angles, here, but I wanted to start off by
writing about the idea of a HAL client.


## Hyperclient

Initially, I started using a Ruby library called
[Hyperclient](https://github.com/codegram/hyperclient). It basically abstracts
parsing and navigating HAL documents and making requests to the links in them
away from the developer using it. At first, I thought this would be great.
However, the more I used it, the more I felt like it was doing more *to* me than
*for* me.

In it's effort to keep the developer from needing to know the HAL spec front to
back, it is pretty complicated and has a lot of corners for edge cases to get
stuck in. And, really, since HAL is so readable, it ends up just mapping hash
access to method calls in a lot of cases.


## DIY

I really hope I don't have a NIH complex, but... I *do* use Arch Linux because
of the level of fiddling it allows with how my laptop behaves... and I tune and
tweak my `.vimrc` and `.bashrc` so my environment is just so. Thus, it shouldn't
really be surprising that when I wanted more control, I decided to build
something from scratch (sort of).

The other day, after encountering some weird interaction between Faraday,
Hyperclient and certain features of WebMock, I decided to see how hard it would
be to rip Hyperclient out and replace it with something dumber and simpler.
After talking it over with the team, we decided the best course would be to
time-box it to a day.

Here's the 42 lines I replaced Hyperclient with:

``` ruby hal_response.rb
require 'forwardable'

class HalResponse
  extend Forwardable

  def_delegators :response, :status, :success?, :body
  def_delegators :hal, :[], :fetch

  attr_reader :response

  def initialize(response)
    @response = response
  end

  def hal?
    response.headers['content-type'] =~ %r{\Aapplication/hal\+json}
  end

  def links
    hal['_links'] || {}
  end

  def get_uri(rel, expansions={})
    URITemplate.new(links[rel.to_s]['href']).expand(expansions)
  end

  def attributes
    hal.except('_links', '_embedded')
  end

  private

  def hal
    @hal ||= parse_hal
  end

  def parse_hal
    self.hal? ? JSON.parse(response.body) : {}
  rescue JSON::JSONError, TypeError
    {}
  end
end
```

The way this works, is you build your own Faraday object, you make the requests
yourself, then you feed the response into a `HalResponse` constructor to monkey
with it. If you want to follow a link, you call `get_uri` and feed that result
to your Faraday object. A short example:

``` ruby example.rb
client = Faraday.new('http://example.com')
root = HalResponse.new(client.get)

posts = HalResponse.new(client.get(root.get_uri(:posts)))
search_results = HalResponse.new(client.get(root.get_uri(:search, q: 'Ruby')))
new_post = HalResponse.new(client.post(root.get_uri(:create_post, title: 'Blah', body: 'whatever')))
```

It's important to note that my 42 lines don't deal at all with embedded
resources and are tailored to handle cases presented by a specific API of a
specific service I'm coding against. But that's actually part of my point: Do we
need HAL clients? Is the document format that hard to grok?

Since HAL (and other hypermedia types) rely on the semantics of HTTP pretty
heavily, it seems much more important to have a robust HTTP client. Dealing with
the specially-formatted response bodies seems like a separate issue to me. I may
expand the above class as I continue to use it in production, but I kind of like
the wordy, do-nothing-for-you kind of way it works.
