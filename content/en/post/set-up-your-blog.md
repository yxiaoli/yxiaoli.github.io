+++
title = "Set up a Blog based on Hugo"
date =  2015-12-21T16:22:23+02:00
tags = ["techniques","frontend"]
categories = ["tech"]
description = "Hugo is the world’s fastest framework for building (static) websites. For bloger, it saves us a lot of technical trouble."
# 此处可以写摘要！
menu = ""
banner = "banners/set-up-your-blog.jpg"
+++

# Hello GitHub!

GitHub is more than a collaborative tool to make your software development more effective. In fact, it is more like a community where you share and contribute your code.

Since we want to build our own blog, so the whole static websites should be stored in Github. Now here we go. sign up in Github

Okay！ you are now a member of github, enjoy your coding journey!

# Bravo！ Hugo

A powerful and fast Website engine is Hugo， which is designed for go lovers！ Hugo flexibly works with many formats and is ideal for blogs, docs, portfolios and much more. Hugo’s speed fosters creativity and makes building a website fun again.

It helps you to build your blog, you don’t have to know the details of css, js, MySQL etc. It saves you a lot of trouble, and you can concentrate more on your writing and coding.

# Usage of Hugo

## Install Hugo

- [Follow the quickstart](https://gohugo.io/getting-started/installing/)

- [Build from the source](https://github.com/gohugoio/hugo/releases)
   1. Download and unzip
   2. move the "hugo/" into "/usr/bin/hugo"
   3. test hugo
## Relevant Commands 

### 1. new a website

    $hugo new site tintinsnowy.github.io

If you find something wrong with your path, then use the  absolute path. Or configure the path of Hugo.
    Then you can see the tree:

      ▸ archetypes/
      ▸ content/
      ▸ layouts/
      ▸ static/
      config.toml
The content of blog

### 2. Create the content 
your articles should be store in the path of : content/post. adll the articles are in markdown format.

    $ cd tintinsnowy.github.io
    $ hugo new about.md
    $ hugo new post/firstblog.md

  Then open the `firstblog.md` , you can see the automatically generated file

    +++
    date = "2015-12-26T08:36:54-07:00"
    draft = true
    title = "firstblog"
    +++ 

### 3. Find a joyful theme for your blog

There are a few opensource-themes Every theme has detailed description of usage, but NOT all the configurations are the same. You should customize what you’ve chosen. For example,

    $ pwd
    >/home/sherry/code/hugo/tintinsnowy.github.io
    $ mkdir themes
    $ cd themes
    $ git clone https://github.com/spf13/hyde.git
now you can see that the theme has been in your themes/

### 4. Set configuration of your theme

This step is extremely important, different theme has different ways of setting, so it’s your time to design. But before you do, you should read the homepage of your theme.

Hugo can read your configuration as JSON, YAML or TOML. Hugo supp orts parameters custom configuration too. Refer to the theme that you’ve chosen for details.

…….. you can see how it looks like in localhost:(the hugo-icarus is the name of theme, you need to change it), open http://localhost:1313/
```commandline
 sherry@sherry:~/code/hugo/tintinsnowy.github.io
 $sudo /home/sherry/code/hugo/hugo server --buildDrafts --watch
```
If you are satisfied with your blog in localhost, you want to display it on web right? Okay do the following steps!

## Create your blog’s repository on Github

Aha, I suppose that you are familiar with git. If not, there is a whale of tutorial of git and github. So shall we start?

### 1. create a new repository

`User & Organization` Pages live in a special repository dedicated to GitHub Pages files. You will need to name this repository with the `account name`.

You must use the `username.github.io` naming scheme. Content from the master branch will be used to build and publish your GitHub Pages site. A repository like `joe/bob.github.io` will not build a User Pages site. [see here](https://help.github.com/articles/user-organization-and-project-pages/) .
When User Pages are built, they are available at `http(s)://<username>.github.io.`

For example, my account name is `tintinsnowy` , so I have to name my repository `tintinsnowy.github.io`

### 2. Build your static website

    $hugo --theme=hugo-icarus --baseUrl="https://tintinsnowy.github.io/"

Upload your website

    $ cd public
    $ git init
    $ git remote add origin https://github.com/tintinsnowy/tintinsnowy.github.io.git
    $ git add -A
    $ git commit -m "first commit"
    $ git push -u origin master

See your website

In your github repository setting, you can see this:

Your site is published at `https://tintinsnowy.github.io`

Now, it seems like the end of our Project. Yet if you wish your blog’s address more unique, you may buy a custom domain.

# Buy domain

A few suggestion:

* godaddy
* aliyun

# Setting

1. Adding a CNAME file to your repository: 

* In the file name field, type CNAME (with all caps!).
* Use `tintinsnowy.com`, not `https://tintinsnowy.com`. Note that there can only be `one domain` in the CNAME file. 

2. Setting your account

| CNAME | @ | tintinsnowy.github.io. |
|-----|:--:|--------------------:|
|A|www|192.30.252.***|
|A|	www	|192.30.252.***|

Don’t forget the `.` after `tintinsnowy.github.io` If you want to know why should be setting like this, see the references.

waiting..& checkout: tintinsnowy.com
Cheers!

## Reference:

1. [Hugo](https://gohugo.io/)
2. [Setting up a custom domain with GitHub Pages](https://help.github.com/articles/using-a-custom-domain-with-github-pages/)
3. [Github Pages Setting](https://help.github.com/articles/user-organization-and-project-pages/)
4. [cnfeat.com](http://www.cnfeat.com/)
5. [coderzh](https://blog.coderzh.com/2015/08/29/hugo/)
6. [quantumman.me](http://quantumman.me/blog/setting-up-a-domain-with-gitHub-pages.html)
7. [tonybai’s blog](http://tonybai.com/2015/09/23/intro-of-gohugo/)