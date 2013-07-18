---
layout: post
title: Ruby HTTP don't for get use_ssl
tags: ruby
---

In a project this week I had send some http requests from a ruby program
and do something with the response.  One of the requests needed to be
https so I tried something along these lines...

{% highlight ruby %}
require "net/https"
require "uri"

uri = URI.parse("https://secure.com/")
http = Net::HTTP.new(uri.host, uri.port)
response = http.request(request)
{% endhighlight %}

This looks like it should work right? When URI parses the https url it
even knows to use the https port...

{% highlight ruby %}
uri.port
# => 443
{% endhighlight %}

Only the request kept timing out and I didn't know why. After some
googleling and rereading parts of blog posts I had previously overlooked
my salvation came from a very helpful post by [August Lilleaas](http://www.rubyinside.com/nethttp-cheat-sheet-2940.html).  Even
though the parsed URI object recognized the https port the http request
still has to be told to send as https.  You can set this with an option
called "user_ssl"

{% highlight ruby %}
http.use_ssl = true
{% endhighlight %}

No more ambiguous timout :)
