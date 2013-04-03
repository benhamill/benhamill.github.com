---
layout: post
title: A Smarter has_many :through?
tags:
- code
- programming
- rails
- ruby
status: publish
type: post
published: true
meta:
  _edit_last: '2'
---
At work, I've been expending a lot of effort on this complicated search functionality where you can enter a search phrase that will full-text search over one model's fields (we're using <a href="http://tenderlove.git.hub.com/texticle/">texticle</a> [<a href="https://github.com/tenderlove/texticle">github</a>], which is awesome) and limit the results by which other models are involved relationally. Sort of like searching Amazon for "green converse" and choosing the "shoes" category.

The object graph behind this is pretty complicated and it's been a real education in SQL trying to make sure the query that gets generated is both reasonably speedy and right. Several times, I've gotten it "working" only to realize I was joining in some table more than once and so either returning some record twice or excluding it when I shouldn't've or joining all rows against all rows and, thus, making everything pass all constraints. My SQL skill has leveled up several times throughout, though, which has been really awesome. This is mostly because I was hand-writing a lot of the join SQL with table aliases and whatnot.

<h4>Example</h4>
The other day, I realized that Rails 3 (or, anyway, the 3.1 release candidates, which is what this app is using) will let you do something that earlier versions would not: do a <code>has_many :through</code> relation on another <code>has_many :through</code>. Say you've got Departments composed of Employees. Employees work in groups to create Widgets and which, in turn, get Tags. You can do this number:

<pre lang="ruby" line="1">class Widget < AR::Base
  has_many :tags
  has_many :employees

  has_many :departments, :through => :employees
end

class Tag < AR::Base
  belongs_to :widget

  has_many :employees, :through => :widget
  has_many :departments, :through => :employees
end

class Employee < AR::Base
  belongs_to :widget
  belongs_to :department

  has_many :tags, :through => :widget
end

class Department < AR::Base
  has_many :employees

  has_many :widgets, :through => :employees
  has_many :tags, :through => :widgets
end</pre>

Which enables stuff like:

<pre lang="ruby" line="1">Department.joins(:tags).where(:tag => { :id => params[:tag_id] })</pre>

<h4>The SQL</h4>
So, though, since I was hand-writing my <code>JOIN</code> statements before, I'm clearly concerned with what, exactly, it's going to execute against the database. So I pulled out good ol' <code>ActiveRecord::Base#to_sql</code> to see. Here's what I got (edited without all the quoting and with newlines):

<pre lang="sql" line="1">SELECT departments.* FROM departments
INNER JOIN employees ON employees.department_id = departments.id
INNER JOIN widgets ON widgets.id = employees.widget_id
INNER JOIN tags ON tags.widget_id = widgets.id
WHERE tags.id = 3</pre>

Hopefully, that query is pretty straight forward and you can see how ActiveRecord has decided how to make all those joins. However, something struck me: I'm joining through with <code>widgets</code> table, but both <code>employees</code> and <code>tags</code> already have <code>widget_id</code> on them. I'd rather have seen something like:

<pre lang="sql" line="1">SELECT departments.* FROM departments
INNER JOIN employees ON employees.department_id = departments.id
INNER JOIN tags ON tags.widget_id = employees.widget_id
WHERE tags.id = 3</pre>

The result set should be the same and it's slightly faster. In this example, joining through the extra table wouldn't be a big hit, probably, but if we've got more objects all related to Widgets and many are, like Departments, related through some other object, we might be (and in my case often are) joining many more tables, so if we can eliminate middle-man joins, it can have an appreciable effect on the query's speed.

<h4>How We Do It</h4>
So it turns out you <em>can</em> make ActiveRecord generate the above SQL. You don't want <code>has_many :through</code> for the second association. If you do like this:

<pre lang="ruby" line="1">class Department < AR::Base
  has_many :employees

  has_many :widgets, :through => :employees
  # has_many :tags, :through => :widgets
  has_many :tags, :foreign_key => :widget_id, :primary_key => :widget_id
end</pre>

You can use the same ActiveRecord query syntax from above to generate the second SQL example. It's a lot of typing, though, so I wondered: Wouldn't it be awesome if ActiveRecord knew you when you had this matching-middle-man-foreign-key situation in a query and generated the leaner SQL?

I'm not sure if there are pitfalls to this I'm not seeing (especially related to uses outside what I'm doing with it right now), but I've started digging around in the Rails source to see where it's thinking about these kinds of things (led me to <code>lib/active_record/associations/join_dependency/join_association.rb:72</code> so far). I'd love some thoughts and feedback on these ideas or guidance in my code-diving efforts. I expect I may end up in the Arel source at some point... we'll see where it takes me.
