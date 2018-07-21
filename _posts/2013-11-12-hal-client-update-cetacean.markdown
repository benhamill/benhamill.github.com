---
layout: post
title: "HAL Client Update: Cetacean"
date: 2013-11-12 22:35
---

After writing my last post, I used the code presented therein in production a
few more days and realized I wanted just a *smidge* more power. Specifically, I
wanted it to handle HAL's embedded resources. So I added that and then the other
night, I pulled it out of that project, broke it up into files, named it after
sea animals and pushed it to RubyGems.org. Introducing:
[Cetacean](https://rubygems.org/gems/cetacean).

Now, I realize my last post was all, "Do we need HAL clients?" and shit. But
let's be real, even 42 lines is a HAL client. Or... whatever, depending on what
your definition of "client". In fact, the design of Cetacean is basically driven
by thinking about what "client" means and what I felt like I needed a tool for
with respect to HAL documents.

Basically, Cetacean helps you deal with HAL documents, it doesn't really help
you with hypermedia APIs. The general work flow is that you make a Faraday
client (configured however seems best to you) and make a request to some URI.
You feed the response to Cetacean and then operate on it. You might get a URI
out of it (from a link, say), which you can then feed back to your Faraday
client, which response you can feed to Cetacean, again, etc.

Perhaps an example would be helpful:

``` ruby
api = Faraday.new('https://api.example.com/') do |faraday|
  faraday.headers['Accept'] = 'application/hal+json'
end

root = Cetacean.new(api.get)
```

At this juncture, you can ask `root` about some stuff. It's an instance of
`Cetacean::Responce`. It proxies some methods back to the `Faraday::Response`,
and otherwise is mostly sugar for getting things out of the HAL document.

``` ruby
root.success? # => true
root.status # => 200
root.response # => The `Faraday::Response` you passed in.
root.body # => '{"foo":"bar","_links":{"self":{"href":"/"},"users":{"href":"..."}}}'
root.hal? # => true
root.attributes # => { 'foo' => 'bar' }
root['foo'] # => 'bar'
root.links # => { 'self' => { 'href' => '/' } }
root.get_uri(:self) # => A `URITemplate` object for you to manipulate.
```

So if you want to follow the 'self' rel, you use `get_uri` method to get a
template, do whatever you want with it (probably `to_s` and `expand` are the
most common cases). And then... do whatever you want with that. Make another
HTTP request? Get it tattooed on your stomach? Cetacean doesn't really care.

Continuing on, imagine further interactions with an API. Notice that at each
stage, Cetacean deals with the HAL, and then gets out of the way.

``` ruby
users = Cetacean.new(api.get(root.get_uri(:users).to_s))
user = users.embedded(:users).first
```

Assuming that the 'users' rel was some document that embedded an array of
documents under the 'users' rel, `user` is an instance of
`Cetacean::EmbeddedResource`. It behaves mostly like a `Cetacean::Response`
except is doesn't have a request object to proxy or refer to. Ideally, you
shouldn't have to keep track of which came from where once you're dealing with a
document.

``` ruby
important_blog_post = Cetacean.new(api.get(user.get_uri(:post).expand(id: 2)))
interesting_blog_posts = Cetacean.new(api.get(root.get_uri(:search_posts).expand(q: 'interesting')))
```

Just another couple of examples of following different links, both of these
requiring expansion.

This is certainly more wordy than using a more featureful library like
Hyperclient or whatever. I feel that this gives you a lot more control over how
you deal with the documents you get back. If you unexpectedly get back an
invalid body, Cetacean won't barf trying to parse it like JSON, if you get back
`'image/jpeg'` instead of `'application/hal+json'`, Cetacean makes it easy to
deal with that however is most appropriate. If you need to do some fancy,
weird-ass configuration of Faraday that's super specific to your use-case, then
you don't have to worry about what a middle-man library might do to your
connection configuration.

Anyway, I guess I wrote another gem. If you use it, I'd be interested to hear
about it. If you want to help, feel free. It's
[on Github](https://github.com/benhamill/cetacean).
