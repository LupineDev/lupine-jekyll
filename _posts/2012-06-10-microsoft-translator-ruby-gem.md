---
layout: post
title: Microsoft Translator Ruby Gem
categories:
  - software development
tags:
  - ruby
  - translator
---

Origninally posted on [TechHui](http://www.techhui.com/profiles/blogs/microsoft-translator-ruby-gem)

I know "Microsoft" is not the first word that comes mind when you're writing a ruby application but since Google dropped the free tier for their translation service the Microsoft Translator API is a good alternative for a small/personal project that you don't want to have to bother with the monthly bill. 

Recently I've had to use this API in a project and this weekend I
extracted the functionality out into a simple gem.  I present to you
 (queue applause) [microsoft_translator](https://github.com/ikayzo/microsoft_translator)

Before translating things from your ruby application you first need to
sign up for the Microsoft Translator API in the Windows [Azure
Datamarket](https://datamarket.azure.com/dataset/1899a118-d202-492c-aa16-ba21c3)

Also, you shouldn't stress about what to put for the **Redirect URI**. For
the purposes of this gem you won't be using it so your project's
homepage will work just fine. You'll use the **Client ID** and **Client
secret** to authenticate your requests to the API. Once this is done you'll
install it like you would any other gem...

First create a MicrosoftTranslator::Client with your Client ID & secret.
To translate pass in the foreign text allong with the language codes for
the language you are going from/to and the content type. The content
type is either "text/plain" or "text/html"

{% highlight ruby %}
translator = MicrosoftTranslator::Client.new('your_client_id', 'your_client_secret')
spanish = "hasta luego muchacha"translator.translate(spanish,"es","en","text/html")

# =>  "until then girl"
{% endhighlight %}
<br />

That's about it! This is a list of the supported languages by the
[Microsoft Translate API](http://www.microsofttranslator.com/help/?FORM=R5FD) and here are all the
language codes as a [helpful reference](http://www.loc.gov/standards/iso639-2/php/code_list.php).
