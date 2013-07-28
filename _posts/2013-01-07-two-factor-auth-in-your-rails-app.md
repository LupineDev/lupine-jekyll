---
layout: post
title: Two-Factor Authentication in your Rails App with Devise & Yubikey
categories:
  - software development
tags:
  - rails
  - yubikey
---

Most rails applications have some sort of "user" to represent either
customers who are consuming the service or administrators that are
publishing content to the web application.  It's important that these
users are authenticated (ensuring that they are who they say they are).
For user authentication in a Ruby on Rails application, [Devise](https://github.com/plataformatec/devise) is one of
the best solutions out there.  It has a very active community and wide
variety of options and extensions to fit your business model.  Sometimes
customers may ask for or the application requires an additional layer of
authentication beyond the basic username/password combination.

Enter Yubikey.

[Yubikey Hardware](http://www.yubico.com/products/yubikey-hardware/) is a small usb "key" that is set up to generate unique
[one-time passwords](http://en.wikipedia.org/wiki/One-time_password) that are validated against their server.  Fortunately
someone has written an extension to incorporate the yubikey as a second
factor of authentication against devise users called
[yubikey_database_authenticatable](https://github.com/mort666/yubikey_database_authenticatable).

In order to require users to log in with a yubikey there are a few large steps you have to take.

1. Have a rails application
2. Set up regular 'database_authenticatable' user authentication with
devise. And require user authentication for access to all or part of the
web application.
3. Add 'yubikey_database_authenticatable' gem to your project and set up
the user model and table to use yubikey authentication.
4. Generate the devise views and customize the session login page to
include the yubikey one-time password.
5. Add user managment/administration to the application for regulating
and associating users with a yubikey.

I have created a skeleton rails app with these steps and posted it to [Github](https://github.com/LupineDev/rails_with_yubikey)

#### 1. Have a rails application

For this app I am using rails 3.2.12

{% highlight bash %}
$ rails new rails_with_yubikey
$ cd rails_with_yubikey/
{% endhighlight %}
<br />

At Ikayzo we have a fondness for lemurs so this application will maintain a list of lemurs species.

{% highlight bash %}
$ rails g scaffold lemurs species:string description:text
{% endhighlight %}
<br />

Now that we have some actual pages in the applications we can set the
root url of the application to the LemursController

{% highlight bash %}
$ rm public/index.html
{% endhighlight %}
<br />
{% highlight ruby %}
# config/routes.rb
...
root :to => 'lemurs#index'
...
{% endhighlight %}
<br />

#### 2. Set up regular devise authentication

There is already some great documentation for this on the [Devise readme](https://github.com/plataformatec/devise#getting-started)
but here are the steps I took to add it to this rails application.

Add devise to the global part of the Gemfile

{% highlight ruby %}
# Gemfile
...
gem 'devise'
...
{% endhighlight %}
<br />
{% highlight bash %}
$ bundle install
{% endhighlight %}
<br />

Next generate the devise files and user model
{% highlight bash %}
$ rails generate devise:install
$ rails generate devise User
$ rake db:migrate
{% endhighlight %}
<br />

The only thing left is to require users to log in when accessing the
LemursController


{% highlight ruby %}
# app/controllers/lemurs_controller.rb
class LemursController < ApplicationController
  before_filter :authenticate_user!
  ...
end
{% endhighlight %}
<br />

#### 3.  Add yubikey_database_authenticatable extension to devise

First add the yubikey_database_authenticatable gem to the applications Gemfile

{% highlight ruby %}
# Gemfile
...
gem 'devise'
gem 'yubikey_database_authenticatable'
...
{% endhighlight %}
<br />

{% highlight bash %}
$ bundle install
{% endhighlight %}
<br />

There are two new columns that the users table will need to authenticat
witht he yubikey.

{% highlight bash %}
$ rails generate migration add_yubikey_to_users
{% endhighlight %}
<br />

{% highlight ruby %}
# db/migrate/TIMESTAMP_addyubikey_to_users.rb
class AddYubikeyToUsers < ActiveRecord::Migration
  def change
    add_column :users, :useyubikey, :boolean
    add_column :users, :registeredyubikey, :string
  end
end
{% endhighlight %}
<br />

{% highlight bash %}
$ rake db:migrate
{% endhighlight %}
<br />

You will have to replace :database_authenticatable with
:yubikey_database_authenticatable to the devise line of the user
model.

{% highlight bash %}
$ rails generate devise:views
{% endhighlight %}
<br />

{% highlight html %}
# app/views/devise/sessions/new.html.erb

<h2>Sign in</h2>

<%= form_for(resource, :as => resource_name, :url => session_path(resource_name)) do |f| %>

  <div><%= f.label :email %><br />

  <%= f.email_field :email %></div>

  <div><%= f.label :password %><br />

  <%= f.password_field :password %></div>

  <% if devise_mapping.rememberable? -%>

    <div><%= f.check_box :remember_me %> <%= f.label :remember_me %></div>

  <% end -%>

  <% if devise_mapping.yubikey_database_authenticatable? -%>

    <div><%= f.label :yubiotp, "Yubikey One Time Password" %><br />

    <%= f.password_field :yubiotp%></div>

  <% end -%>

  <div><%= f.submit "Sign in" %></div>

<% end %>

<%= render :partial => "devise/shared/links" %>
{% endhighlight %}
<br />

#### 5. Add user managment to the application for regulating and associating yubikeys with users

For this application I just created a user and added a yubikey to them
through the rails console.  To require a user to login with yubikey the
boolean **use_yubikey** needs to be set to **true**. The other user also needs
to have the **registeredyubikey** field set to their yubikeky.  I did this
through the console by copying the text output from the yubikey to a
clipboard and setting the field in the console.  Part of the code that
was added to the user model is designed to peel of the first 11
characters of the yubikey one-time password which is its static and
unique identifier.

In a real world application there will need to be features built in to
managing the yubikey for the user.  If the customers/users will be
providing their own yubikey they will need to update these settings
themselves.  However if the yubikeys are distributed to a small group of
users, managing these attributes should probably be hidden behind some
administration side of the application.

Origninally posted on [TechHui](http://www.techhui.com/profiles/blogs/add-two-factor-authentication-to-your-rails-app-with-devise)
