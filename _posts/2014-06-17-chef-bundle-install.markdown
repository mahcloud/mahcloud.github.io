---
layout: post
title: "Chef - Server Provisioning Software"
date: 2014-06-17 12:17:00
categories: RailsDeploy
---

So, I got to the point where I wanted a clean slate so I reformatted and started my mac over with a clean install.
I'm setting up things that I haven't configured in a long time.
It's good for my memory though.


Problem #1
I'm running
{% highlight bash %}
bundle install
{% endhighlight %}

on ruby version 1.9.2 for a chef repository. It seems to have a problem installing the nokogiri gem. [Nokogiri][nokogiri] is a HTML, XML, SAX, and Reader parser.
{% highlight bash %}
bundle install
Fetching gem metadata from https://rubygems.org/.......
Fetching additional metadata from https://rubygems.org/..
Using rake 10.3.1
Using builder 3.2.2
Using gyoku 1.1.1
Using mini_portile 0.5.3
sh: -c: line 0: unexpected EOF while looking for matching `"'
sh: -c: line 1: syntax error: unexpected end of file

Gem::Ext::BuildError: ERROR: Failed to build gem native extension.

    /Users/mahcloud/.rvm/rubies/ruby-1.9.2-p320/bin/ruby extconf.rb "--with-xml2-include=/usr/local/Cellar/libxml2/2.7.8/include/libxml2 --with-xml2-lib=/usr/local/Cellar/libxml2/2.7.8/lib --with-xslt-dir=/usr/local/Cellar

extconf failed, exit code 2

Gem files will remain installed in /Users/mahcloud/.rvm/gems/ruby-1.9.2-p320@1kb-chef/gems/nokogiri-1.6.2 for inspection.
Results logged to /Users/mahcloud/.rvm/gems/ruby-1.9.2-p320@1kb-chef/extensions/x86_64-darwin-13/1.9.1/nokogiri-1.6.2/gem_make.out
An error occurred while installing nokogiri (1.6.2), and Bundler cannot continue.
Make sure that `gem install nokogiri -v '1.6.2'` succeeds before bundling.
{% endhighlight %}
I take the advice and run `gem install nokogiri -v '1.6.2'` which fails because it can't find the libiconv native library.
{% highlight bash %}
gem install nokogiri -v '1.6.2'
Building native extensions.  This could take a while...
Building nokogiri using packaged libraries.
ERROR:  Error installing nokogiri:
	ERROR: Failed to build gem native extension.

    /Users/mahcloud/.rvm/rubies/ruby-1.9.2-p320/bin/ruby extconf.rb
Building nokogiri using packaged libraries.
-----
libiconv is missing.  please visit http://nokogiri.org/tutorials/installing_nokogiri.html for help with installing dependencies.
-----
*** extconf.rb failed ***
Could not create Makefile due to some reason, probably lack of
necessary libraries and/or headers.  Check the mkmf.log file for more
details.  You may need configuration options.

Provided configuration options:
	--with-opt-dir
	--with-opt-include
	--without-opt-include=${opt-dir}/include
	--with-opt-lib
	--without-opt-lib=${opt-dir}/lib
	--with-make-prog
	--without-make-prog
	--srcdir=.
	--curdir
	--ruby=/Users/mahcloud/.rvm/rubies/ruby-1.9.2-p320/bin/ruby
	--help
	--clean
	--use-system-libraries
	--enable-static
	--disable-static
	--with-zlib-dir
	--without-zlib-dir
	--with-zlib-include
	--without-zlib-include=${zlib-dir}/include
	--with-zlib-lib
	--without-zlib-lib=${zlib-dir}/lib
	--enable-cross-build
	--disable-cross-build

extconf failed, exit code 1

Gem files will remain installed in /Users/mahcloud/.rvm/gems/ruby-1.9.2-p320@1kb-chef/gems/nokogiri-1.6.2 for inspection.
Results logged to /Users/mahcloud/.rvm/gems/ruby-1.9.2-p320@1kb-chef/extensions/x86_64-darwin-13/1.9.1/nokogiri-1.6.2/gem_make.out
{% endhighlight %}

[libiconv][libiconv] is a library that converts string encodings from or to unicode. libiconv comes standard on my mac, but for some reason nokogiri thinks it is missing.


So we need install libiconv again and tell nokogiri where libiconv is. Some options for installing libiconv are macports and brew. I installed with brew. And then ran.
{% highlight bash %}
gem install nokogiri -- --with-iconv-dir=/usr/local/Cellar/libiconv/1.14/
{% endhighlight %}
This installed successfully. This fixed the problem in my development environment for my site, but for the chef repository, it still doesn't recognize that nokogiri is installed.


Next, I ran
{% highlight bash %}
xcode-select --install
{% endhighlight %}
This installs the command-line developer tools for mac, which includes libiconv. Install nokogiri version as normal and that took care of the problem.


[nokogiri]: https://github.com/sparklemotion/nokogiri
[libiconv]: https://www.gnu.org/software/libiconv/
