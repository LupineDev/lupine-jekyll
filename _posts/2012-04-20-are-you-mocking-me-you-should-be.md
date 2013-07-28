---
layout: post
title: Are you mocking me? (You should be)
categories:
  - software development
tags:
  - ruby
  - rspec
  - tdd
---

This post is just a friendly reminder for those that practice TDD/BDD to
make sure they are not putting more ceremony into their tests than is
necessary.  Use mocks and stubs instead!

Mocks are just stand-ins that behave like the thing we want it to
represent.  Stubs will fake a method call and return a canned response.
These save time both in setting up the test and in running it (depending
on if your real objects are persisted in a test database).  When you
test a method you should only test things the method is actually **doing**. 

I'm going to use Ruby on Rails and RSpec as an example.  A classic
situation is that we have an e-commerce application with a Order model
that has many ShopItems.  Now all we need to test that a cart object
will calculate it's total as the sum of the prices of all of its
ShopItems.  We don't care what the names of the items are or if they are
actually stored in the database.

{% highlight ruby %}
describe Order do
  describe "#sub_total" do
    # writing everything to db
    it "should calculate the sum of all of its shop_items" do
      order = Order.create(some_options)
      3.times do
        order.shop_items << ShopItem.create(:price => 2.00)
      end
      order.subtotal.should be_within(0.01).of(6.0)
    end
    #  mocking
    it "should calculate the sum of all of its shop_items" do
      order = Order.create(some_options)
      fake_items = [stub(:price => 2.00), stub(:price => 2.00), stub(:price => 2.00)]
      order.stub!(:shop_items).and_return(fake_items)
      order.subtotal.should be_within(0.01).of(6.0)
    end
  end
end
{% endhighlight %}
<br />

Origninally posted on [TechHui](http://www.techhui.com/profiles/blogs/are-you-mocking-me-you-should-be)
