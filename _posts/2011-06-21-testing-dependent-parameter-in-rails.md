---
layout: post
title: Testing :dependent Parameter In Rails
categories:
  - software development
tags:
  - rails
  - ruby
  - rspec
---

In an effort to post on here more often I'm going to start small.   I
had a brain fart on how to test the has_many :dependent parameter for
Rails associations today.  With my trusty [rspec book](http://pragprog.com/titles/achbd/the-rspec-book) not on-hand, it just took a little googling to find a solution. 

{% highlight ruby %}
describe Reticulum do
  describe "associations" do
    should { have_many(:reticules).dependent(:restrict) }
  end
end
{% endhighlight %}
<br />

Many thanks to [Robert Spiecher](http://rspeicher.tumblr.com/) in his
reply to this [Stackoverflow post](http://stackoverflow.com/questions/2673041/checking-activerecord-associations-in-rspec) that showed me the light :)
