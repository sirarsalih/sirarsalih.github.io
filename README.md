# Setup

I used the [Hydejack](http://qwtel.com/hydejack/) [Jekyll](http://jekyllrb.com) theme for this blog. Here are the steps required to set up the development environment:

1. Fork this repo.
2. Setting up Jekyll on Windows: https://jekyllrb.com/docs/windows/, http://jekyll-windows.juthilo.com/1-ruby-and-devkit/
3. Setting up GitHub pages with Jekyll: https://help.github.com/articles/setting-up-your-github-pages-site-locally-with-jekyll/
4. On Windows, make sure your Gemfile has this:

source 'http://rubygems.org'
gem 'github-pages'
gem 'wdm', '>= 0.1.0' if RbConfig::CONFIG['target_os'] =~ /mswin/i

5. Start cmd, navigate to the repo and launch the site:

<code>bundle exec jekyll serve</code>

# Hydejack

Hydejack is a pretentious two-column [Jekyll](http://jekyllrb.com) theme, stolen by [`@qwtel`](https://twitter.com/qwtel) from [Hyde](http://hyde.getpoole.com). You could say it was.. [hydejacked](http://media3.giphy.com/media/makedRIckZBW8/giphy.gif).

## Features
Unlike Hyde, it's opinionated about how you are going to use it (as a blog!)
Features included are:

* Touch-enabled sidebar / drawer for mobile, including fallback when JS is disabled.
* Github Pages compatible tag support based on [this post][tag].
* Customizable link color and sidebar image, per-site, per-tag and per-post.
* Optional author section at the bottom of each post.
* Posts grouped by year on front and tag page.
* Social media icons (github, twitter) on sidebar.
* Optional comment section powered by Disqus.
* Math blocks via [KaTeX](https://khan.github.io/KaTeX/).

## Download

Hydejack is developed on and hosted with GitHub. Head to the [GitHub repository](https://github.com/qwtel/hydejack) for downloads, bug reports, and feature requests.

## Sidebar
I love the original Hyde theme, but unfortunately the layout isn't as great on small screens.
Since the sidebar moves to the top, the user has to scroll just to read the title of a blog post.

By using a drawer component I was able to retain the original two column layout. It's possible to move the drawer via touch input (with the help of a little JavaScript).

Since the background image contributes to the feel of the page I'm letting it peek over the edge a bit. This also provides a hint to the user that an interaction is possible.

## Manual

### Configuration
You can configure important aspects of the theme via [`_config.yml`](https://github.com/qwtel/hydejack/blob/master/_config.yml). This includes:

* the blog description in the sidebar
* the (optional) author description and photo
* default image and link color of the blog
* the github and twitter usernames

### How to Change the Image and Color of a Post
In the manifest of a blog post, simply add an url as `image` and a CSS color as `color`:

~~~yml
layout: post
title: Introducing Hydejack
image: http://qwtel.com/hydejack/public/img/hyde.jpg
color: '#949667'
~~~

### How to Add a New Tag

Tags are not meant to be used #instagram #style: #food #goodfood #happy #happylife #didimentionfood #yougetthepoint, as each tag requires some setup work. I tend to think of it as categories that can be combined.

1.  Add an entry to `_data/tags.yml`, where the key represents a slug and provide at least a `name` value and optionally `image`, `color` and `description`.

    Example `/_data/tags.yml`:

    ~~~yml
    mytag:
      name: My Tag
    ~~~

2.  Make a new file in the `tag` folder, using the same name you've used as the key / slug and change the `tag` and `permalink` entries.

    Example `/tag/mytag.md`:

    ~~~yml
    layout: blog_by_tag
    tag: mytag
    permalink: /tag/mytag/
    ~~~

3.  Tag your blog posts using the `tags` key (color and image will only depend on the first tag).

    ~~~yml
    layout: post
    title: Introducing My New Tag
    tags: [mytag, othertag]
    ~~~

4. (optional) Add the tag to the sidebar, by adding it to `sidebar_tags` in `_config.yml`.
   They will appear in the listed order.

   ~~~yml
   sidebar_tags: [mytag, othertag]
   ~~~

[tag]: http://www.minddust.com/post/tags-and-categories-on-github-pages/
