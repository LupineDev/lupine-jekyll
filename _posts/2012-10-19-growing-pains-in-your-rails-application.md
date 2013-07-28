---
layout: post
title: Growing Pains in Your Rails Application
categories:
  - software development
tags:
  - rails
  - ruby
  - oop
---

If you have been part of building an application that has lasted longer
than a month then you've probably had some troubles as the project
grows.   Business logic requirements get more and more complicated.
Features change direction.  You occasionally "hack" something just to
satisfy a release or an urgent bug fix, saying "I can refactor/clean
this up this later..." 

Then one day "later..." is now.  New features are getting harder and
harder to implement.  Everything seems intricately related and you can't
change one part of the app without the effects showing up somewhere else
as bugs three weeks after the change was released.

Usually it starts when longer logic starts showing up in your controller
methods to fit the requirements.  Once you notice, you start moving the
behavior logic into the models to fit the "fat-model skinny-controller"
best practice.  Which is good but it can only take you so far.  Sooner
or later the models get too bloated, and complex.

Is there a way to get back out of this?  Bryan Helmkamp wrote a really
great blog post [7 Patterns to Refactor Fat ActiveRecord Models](http://blog.codeclimate.com/blog/2012/10/17/7-ways-to-decompose-fat-activerecord-models/) This post
was incredibly helpful. I've felt this pain on projects but didn't
really know how I could make it better.  The solutions make it easier to
spot and group pieces that can be extracted out into their own classes.

Another issue is how do you know you're in pain?  Often these problems
arise organically over a long period of time.  By the time you become
aware, it already feels like there is no hope. Static analysis and [code metrics](http://rubyrogues.com/041-rr-code-metrics/)
are a great way to pick up on these issues before they require
weeks to extract and refactor.  Bryan is also founder of [Code Climate](https://codeclimate.com/) a
very high quality code metric PaaS for ruby projects.  Being aware of
and dealing with code smells earlier saves you time and money throughout
the life of the project.

These are some recommended Rails & OOP books that are next up in my queue of tech things to read 

- [Objects on Rails](http://objectsonrails.com/) by Avdi Grimm
- [Practical Object-Oriented Design in Ruby](http://www.poodr.info/) by Sandi Metz
- [Growing Object-Oriented Software Guided by Tests](http://www.growing-object-oriented-software.com/) by Steve Freeman & Nat Pryce

Origninally posted on [TechHui](http://www.techhui.com/profiles/blogs/growing-pains-in-your-rails-applicaiton)
