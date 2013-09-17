---
layout: post
title: "'ActiveModel::Dirty'"
date: 2013-09-17 18:13
comments: true
categories: ruby, ruby on rails 
---

Today, I was briefly introduced to ActiveModel::Dirty.  The source
documentation
[here](http://api.rubyonrails.org/classes/ActiveModel/Dirty.html) shows a
minimal example:

{% codeblock lang:ruby %}
class Person
  include ActiveModel::Dirty

  define_attribute_methods :name

  def name
    @name
  end

  def name=(val)
    name_will_change! unless val == @name
    @name = val
  end

  def save
    @previously_changed = changes
    @changed_attributes.clear
  end
end
{% endcodeblock %}

In the end, I had to choose a different method because of [this
reason](http://stackoverflow.com/questions/13074582/problems-implementing-activemodel-dirty-rails-3-2-8).
Stashing this here for later because ActiveModel::Dirty methods look cool!

The _was method particularly looks cool.  Great for keeping track of
changes to models.
