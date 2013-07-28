---
layout: post
title: Write You're Own eBook in Markdown with Ruby and Github Pages
categories:
  - software development
  - writing
tags:
  - ruby
  - markdown
  - ebook
---

![wordsmith](http://9a6b3d25fbd39dfd6920-37d0972e62b1cfc05ea05a81768e1191.r69.cf2.rackcdn.com/cover.jpg)

Many of us have either had the urge to write a book or spew some sort of
long-winded excrement of words.  Maybe it's a story that's been bouncing
around in your head, a piece of history you would like to document, or a
coalescence of documentation/blog posts on a subject you are passionate
about. Regardless of the reason they all require the same thing.  The
ability to convert written text into a document or eBook format.

Since I am a ruby developer I've gotten quite comfortable writing
[markdown](http://daringfireball.net/projects/markdown/) for documentation or general formatted text content. So the
ability to compose the book in markdown that supports [Github style](https://help.github.com/articles/github-flavored-markdown) code
snippets is a big plus for me. After some brief searching I found ruby
gem called [wordsmith](https://github.com/tractical/wordsmith) that fit the bill pretty nicely with decent set of
features.

- Compose content in markdown with syntax highlighting for code snippets.
- Simple [directory structure](https://github.com/tractical/wordsmith#usage) that automatically generates the table of contents.
- Generates eBooks in several popular formats
  
  - HTML with styles that are easy to customize
  - epub
  - mobi (kindle)
  - pdf
 
**Note:** The following example requires basic understanding of git/github, html & css, how to use ruby/rubygems,  as well as the command line.

Before installing maker sure to install the [pandoc](http://johnmacfarlane.net/pandoc/installing.html) dependency listed in
the [README](https://github.com/tractical/wordsmith#installing).  Don't forget to install the LaTeX library listed in the
pandoc install instructions if you would like to generate a pdf version.
Also, [kindlegen](http://www.amazon.com/gp/feature.html?ie=UTF8&docId=1000234621) is required if mobi is one of your desired output
formats.

I've tested the gem using ruby 1.9.3-p392. It installs like any other gem:

{% highlight bash %}
gem install wordsmith --no-rdoc --no-ri
{% endhighlight %}

To create a new ebook project simply follow the [usage](https://github.com/tractical/wordsmith#usage) instructions
outlined in the README.  The required structure of the chapters and
subsections are shockingly straight forward.  Simply write all your
content and generate it to the desired format.  The gem sets up a git
repository for the book automatically so it is a good idea to store the
progress of your work in incremental and contextually related git
commits.

Initially the stylesheet that is included is pretty simple but would be easy to override by someone who possesses a bit of CSS know-how.  A custom header/footer can also be added to the eBook by editing the respectively named html files in the layout folder.

The wordsmith gem also includes a publish command. With this you can
publish the html version of your eBook using [github pages](https://help.github.com/categories/20/articles).

1. Write and [generate](https://github.com/tractical/wordsmith#to-generate-your-book-in-different-formats) your ebook in the html format
2. Commit to the master branch
3. [Create a repository](https://help.github.com/articles/create-a-repo) on github named for your ebook but don't push
anything to it. Also elect not to create a README file.
4. Update the .git/config file for the ebook git repository so the
origin remote is referencing your new repository on github.
5. Run the command:
  {% highlight bash %}
    $ wordsmith publish git@github.com:your_github_username/your_ebook_repository_name.git
  {% endhighlight %}
6. If the publish command fails for some reason push it up manually with the command:
  {% highlight bash %}
    git push origin gh-pages:gh-pages
  {% endhighlight %}

You can see an example I created based off the one wordsmith generates
for every new project at [http://lupinedev.github.com/wordsmith-example/](http://lupinedev.github.com/wordsmith-example/)

This is definitely a programmer's approach to writing an eBook and
probably not for everyone, but I found it to be an entertaining exercise
that I might use in future.  I have not done too much research on the
products/tools that already exist in the eBook-creating world but would
be curious what other options people have found and what they think of
them.

Origninally posted on [TechHui](http://www.techhui.com/profiles/blogs/write-you-re-own-ebook-in-markdown-with-ruby-and-github-pages)
