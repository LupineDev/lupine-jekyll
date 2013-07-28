---
layout: post
title: Only Version Image Files With CarrierWave Gem
categories:
  - software development
tags:
  - rails
  - ruby
  - file uploads
---

For the current project I am working on we are using the
[CarrierWave](https://github.com/jnicklas/carrierwave) gem as an upload
solution for users to upload files. It is a great solution for uploading
files to your rails that allows you to specify "versions" of uploaded
images that are cropped and/or resized to the width/height you need.
This is a great feature for creating thumbnails or other precise image
sizes that was needed for this project. Unfortunately we needed the
users to be able to have this single CarrierWave "uploader" to upload
many differenty file types of files not just images. 

Because the versions are applied to all uploads you don't want or need
multiple versions of the same spreadsheet or audio file. I have forked
CarrierWave on github and tweaked the 0.4-stable branch to detect the
content type and not cache, process or store versions of files that
aren't image files. On this branch **only uploads from the browser are
versioned.**

If you are using a rails 2.3.x app and selective versioning sounds like
it would be useful to you, check out
[https://github.com/LupineDev/carrierwave/tree/0.4-selective_versioning](https://github.com/LupineDev/carrierwave/tree/0.4-selective_versioning)

First uninstall the old version of carrierwave and then wget/download/clone my git repository, build the gem and install it. 


{% highlight bash %}
$ cd path/to/download/carrierwave/
$ gem build carrierwave.gemspec
$ gem install carrierwave-0.4.10
{% endhighlight %}
<br />
<br />

Good luck, hopefully some of you find it useful too.
