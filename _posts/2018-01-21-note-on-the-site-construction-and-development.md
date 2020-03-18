---
layout: post
title: Note on the site construction and development
lang: en
categories:
    - Personal
tags:
    - Env
---

In this post, I describe the setup procedure of this blog site.

This site is developed with Jekyll, a static site generator and hosted by Github pages. All contents of the site are registered in [Github public repository](https://github.com/yumaloop/yumaloop.github.io) and you can browse any source code in it.



<br>

> **Table of Contents**
>
> - [Jekyll](#jekyll)
>     + [Quick-start Instructions](#quick-start-instructions)
>     + [Commands](#commands)
>     + [Edit](#edit)
> - [Github Pages](#github-pages)
> - [File Tree](#file-tree)

<br>



# Jekyll

[Jekyll](https://jekyllrb.com/) is a simple, blog-aware, [static site generators](https://www.staticgen.com/)  perfect for personal, project, or organization sites. Think of it like a file-based CMS like WordPress without all the complexity. Jekyll takes your content, renders [Markdown](https://daringfireball.net/projects/markdown/) and [Liquid](https://github.com/Shopify/liquid/wiki) templates, and spits out a complete, static website ready to be served by Apache, Nginx or another web server. Jekyll is the engine behind [GitHub Pages](http://pages.github.com/), which you can use to host sites right from your GitHub repositories.



### Quick-start Instructions

Jekyll is distributed under the MIT license and you can get it as `ruby gem` . Your site can get up and running in seconds.

1. Install bundler & Jekyll

```bash
$ gem install bundler jekyll
```

2. Create a new blank Jekyll site scaffold

```bash
$ jekyll new my-awesome-site
$ cd my-awesome-site
```

3. Build your site and serve it locally

```bash
$ bundle exec jekyll serve
# => Now browse to http://localhost:4000
```



### Commands

You can use the following `jekyll` command in a number of ways:

- `jekyll new PATH` - Creates a new Jekyll site with default gem-based theme at specified path. The directories will be created as necessary.
- `jekyll new PATH --blank` - Creates a new blank Jekyll site scaffold at specified path.
- `jekyll build` or `jekyll b` - Performs a one off build your site to `./_site` (by default)
- `jekyll serve` or `jekyll s` - Builds your site any time a source file changes and serves it locally
- `jekyll doctor` - Outputs any deprecation or configuration issues
- `jekyll clean` - Removes all generated files: destination folder, metadata file, Sass and Jekyll caches.
- `jekyll help` - Shows help, optionally for a given subcommand, e.g. `jekyll help build`
- `jekyll new-theme` - Creates a new Jekyll theme scaffold

Typically you’ll use `jekyll serve` while developing locally and `jekyll build` when you need to generate the site for production. If you use `bundle` to manage gems, replace `jekyll` with `bundle exec jekyll`.



### Edit

**Manage Jekyll settings**

```
$ vim Gemfile
```



**Add new post**

If you would want to add new post, you could add new .md file written in Markdown format in `_posts` .

```
.
├── Gemfile
├── about.md
├── index.md
├── _config.yml
├── _includes
├── _layouts
├── _posts
│   ├── 2019-01-21-title.md
│   ├── 2019-01-22-title.md
│   ├── 2019-01-23-new-post.md /* Add new post (.md) here! */
├── _sass
├── _site
│   ├── about
│   ├── assets
│   └── jekyll
├── assets
└── vendor
```

Then please run the following as your bash command which automatically convert your Markdown file to HTML and serve your site on `localhost:4000`. So you can check/debug it in your web browser.

```
$ bundle exec jekyll serve
Configuration file: /Users/uchiumi/workspace/yumaloop.github.io/_config.yml
            Source: /Users/uchiumi/workspace/yumaloop.github.io
       Destination: /Users/uchiumi/workspace/yumaloop.github.io/_site
 Incremental build: disabled. Enable with --incremental
      Generating... 
       Jekyll Feed: Generating feed for posts
                    done in 0.353 seconds.
 Auto-regeneration: enabled for '/Users/uchiumi/workspace/yumaloop.github.io'
    Server address: http://127.0.0.1:4000/
  Server running... press ctrl-c to stop.
```



**Add new page**

You could do the same thing for new pages, but the file location changes from `_post`to  `/` (root dir) .

```
.
├── Gemfile
├── about.md
├── index.md
├── new_page.md /* Add new page (.md) here! */
├── _config.yml
├── _includes
├── _layouts
├── _posts
│   ├── 2019-01-21-title.md
│   ├── 2019-01-22-title.md
├── _sass
├── _site
│   ├── about
│   ├── assets
│   └── jekyll
├── assets
└── vendor
```

Then please run the following as your bash command which automatically convert your Markdown file to HTML and serve your site on `localhost:4000`. So you can check it in your/debug web browser.

```
$ bundle exec jekyll serve
...
```





# Github Pages

[Github Pages](https://help.github.com/en/github/working-with-github-pages/about-github-pages) is a static site hosting service that takes HTML, CSS, and JavaScript files straight from a repository on GitHub.



```bash
$ git init
$ git add .
$ git commit -m "update"
$ git remote add origin https://github.com/yumaloop/yumaloop.github.io.git
$ git push origin master 
```







# File Tree

Here is the directory structure of this site. All HTML and CSS/SCSS/Javascript code are generated in `_site/`.

```
.
├── _includes
├── _layouts
├── _posts
├── _sass
├── _site
│   ├── about
│   ├── assets
│   │   ├── css
│   │   ├── highlightjs
│   │   ├── img
│   │   ├── js
│   │   └── pdf
│   └── jekyll
│       └── 2019
│           └── 01
│               ├── 21
│               └── 22
├── assets
│   ├── css
│   ├── highlightjs
│   ├── img
│   ├── js
│   └── pdf
└── vendor
    └── bundle
        └── ruby
            └── 2.5.0
```


# Links

- Ruby - github-pages version list<br>
https://rubygems.org/gems/github-pages/versions/






