---
layout: post
title: Setting up Jekyll and GitHub Pages successfully on Windows
tags: [misc]
---

Right, so, this is a tricky one. I've read around the web that quite a handful of people run into issues when they try to set up Jekyll locally on their Windows machines. As a matter of fact, I read about someone who switched over to Mac in order to get Jekyll running. The problem is that the [Jekyll documentation](https://jekyllrb.com/docs/windows/) isn't a silver bullet, and you often end up tweaking and turning in order to get it fully working.

So here comes a step-by-step guide that I worked out, and which I got working on all of my Windows 10 machines. Enjoy!

1. The end goal here is setting up a blog on the web, so you'll need a site to host your blog. [GitHub Pages](https://pages.github.com/) is a great alternative. Go ahead and go through [the steps](https://pages.github.com/) outlined by GitHub.
2. Install [Chocolatey](https://chocolatey.org/install).
3. Install Ruby by starting cmd and:

	```choco install ruby -version 2.2.4```
	
   Exit cmd.
4. Download [Ruby DevKit](http://rubyinstaller.org/downloads/) for Ruby 2.0 and above. Open the executable and extract the content to:

	C:\tools\DevKit2
	
5. Start cmd from C:\tools\DevKit2 and:
	
	```ruby dk.rb init```
	
6. Edit the config.yml file by adding the following to it:

	```- C:/ruby22```
	
   From cmd, run:
   
   ```ruby dk.rb install```
   
   Exit cmd.
7. Start cmd and:

	```gem install jekyll```

8. Set site generation encoding from cmd:

	```chcp 65001```

9. Install Nokogiri gem for GitHub Pages from cmd:

	```cinst -Source "https://go.microsoft.com/fwlink/?LinkID=230477" libxml2```
	<br/>
	```cinst -Source "https://go.microsoft.com/fwlink/?LinkID=230477" libxslt```
	<br/>
	```cinst -Source "https://go.microsoft.com/fwlink/?LinkID=230477" libiconv```
	<br/>
	```gem install nokogiri```
	
10. Install GitHub Pages from cmd:

	```gem install bundler```
	
11. Create an empty file called Gemfile in the root directory of your blog and add the following to it:

	```source 'http://rubygems.org'```
	<br/>
	```gem 'github-pages'```
	<br/>
	```gem 'wdm', '>= 0.1.0' if RbConfig::CONFIG['target_os'] =~ /mswin/i```
	
12. From cmd, run:
	
	 ```bundle install```
	 
	Jekyll and GitHub Pages are now fully installed in your system. To create a new Jekyll template site, run from cmd:
	
	```bundle exec jekyll new . --force```
	
	To launch the Jekyll site, run from cmd:
	
	```bundle exec jekyll serve```
	
	Go to http://localhost:4000/ to see your site.