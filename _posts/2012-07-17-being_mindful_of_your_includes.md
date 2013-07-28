---
layout: post
title: Being Mindful Of Your Includes
categories:
  - software development
tags:
  - rails
  - ruby
---

The use of "includes(:association_name)" in rails is a widely accepted
rails best practice to prevent n + 1 queries in your views. If you
aren't sure what either of those mean check out the ruby on rails guide
about [eager loading associations](http://guides.rubyonrails.org/active_record_querying.html#eager-loading-associations).  When you are retrieving a collection
of things, this will automatically retrieve the associated object from
the database and store it into memory.   Without this rails would need to
call an extra query for each object when you call an attribute on the
association.

Now this is great especially for *belongs_to* associations.  Where you
should be careful with (and probably not use) this is eager loading
*has_many* associations.  Sure it will cut down on quite a few
queries when the collections are being retrieved and speed up the
rendering of the view.  The problem is that when the collection and
all of it's has_many associated objects  are retrieved in a single
query they all have to be stored in memory.  As the size of the
initial collection or the associated has_many objects grow it will
not scale very well and can quickly bring your server to it's knees.

In computer science there is often a sliding scale between
optimizing for run-time or memory.  For rails (and pretty much every
web application/api) memory is extremely limited.  Therefore, if
your rails view or controller action has quite a few "collections
of collections" you might consider these memory saving alternatives.

- Restructure the sitemap or user interface so not quite so many things need to be displayed at the same time.  This could be as simple as pagination, or more involved like lazy loading "collections of collections" only when they are needed through an asynchronous request.
- Be sure to use YourModel.find_each when you have to iterate through 
  extremely large collections of objects.  The find_each method retrieves the collections in batches.  There is even an option to specify the batch size to reach the happy medium between memory and runtime.

P.S. If you haven't done so already don't forget to register for [AlohaRubyConf](http://aloharubyconf.com/)!
They have an amazing line up of speakers. It's a great opportunity to learn from leaders of the ruby community and share experiences with real developers in the same field.

Origninally posted on [TechHui](http://www.techhui.com/profiles/blogs/being-mindful-of-your-includes)
