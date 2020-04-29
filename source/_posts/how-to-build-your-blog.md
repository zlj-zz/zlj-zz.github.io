---
title: How to build your blog use hexo + github
category: Front-end
date: 2020-04-29 10:08:13
tags: hexo

---
Building a personal blog is simpler than it seems
<!--more-->

## Introduction

Today, we can't live without th Internet, and we can do a lot of things on it. For example, write something.

Ok, next I will tell you how to build a personal blog that belongs to you, and now it's actually very simple. We will use `hexo` and `github`. So I should introduce them to you.

First, let's talk about `github`. **Github** is where people build sofware, and more than 40 million people use github ti discover, fork, and contribute to over 100 million projects. Here we will use github to do our blog server.

Next, let's talk about `hexo`. **Hexo** is a static blog framework based on `Node.js`. It has few dependencies and is easy to install and use. It can easily generate static web pages and be hosted on Github and Heroku.

## Just do it

#### Create warehouse

Log in to github first, if you don't have an account then go and register one. Next, create a new warehouse, the name of the warehouse must be like: `<github username>.github.io`.

Since you need to push files to github, don’t forget to install git. If you are using a **Linux** or **macos** system, you may easily install it in the terminal, *like*:
```shell
sudo apt-get install git
```
*or like*:
```shell
sudo pacman -S git
```

If these do not work, you can find the corresponding command for you system distribution on the Internet.

If you are using windows, then you may need to go to git’s official website to download it.

#### Configure Hexo

**Hexo** based on `Node.js`, so we need to install Node.js. As before, **Linux** and **macos** can be easily installed by command, like:
```shell
sudo pacman -S nodejs
```

Please go to the official website to [download](https://nodejs.org/en/download/), if you are **windows**.

Don't forget to check after installation.The command aret `node -v` and `npm -v`
>NPM is a package management tool installed with `Node.js`, which can solve many problems in `Node.js` code deployment.Common usage scenarios are as follows:
>- allow users to download third-party packages written by others from the NPM server for local use.
>- allow users to download and install command line programs written by others from the NPM server for local use.
>- allow users to upload their own packages or command line programs to the NPM server for others to use.
>The new version of `Node.js` has integrated npm.

After doing this, we can finally start to install `hexo`, we just use this command: 
```shell
sudo npm install -g hexo-cli
```

This installation takes a long time to wait patiently. When the installation is complete, initialize our blog,this command:
```shell
hexo init blog
```

Then you will find that there is a `blog` folder in the current directory, let's go in. It's almost the same here, but we want to deploy to `github`, so we need to do some configuration. Edit `blog/_config.yml`, modify like these:
```yml
# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git
  repo: git@github.com:zlj-zz/zlj-zz.github.io.git
  branch: master
```

After modifying the configuration file, we install the git deployment plugin to deploy.
```shell
npm install hexo-deployer-git --save
```

start deploy:
```shell
hexo clean
hexo g
hexo d
```

If there is no error in executing the command, then the website is deployed. Let's open the browser to visit it, [https://\<github username\>.github.io](#) 



**Let's look at some common command of `hexo`.**
```shell
npm install hexo -g # install hexo
npm update hexo -g # update hexo
hexo init # initialize

hexo n "your blog" == hexo new "your blog" # new article
hexo g == hexo generate 
hexo s == hexo server
hexo d == hexo deploy
```

## Choose theme

We can change the theme of the blog, you can find many themes [here](https://hexo.io/themes/) and choose to use. The theme I use is [hexo-theme-pln](https://github.com/gaoryrt/hexo-theme-pln).

**Download theme like this:** 
```shell
git clone https://github.com/gaoryrt/hexo-theme-pln.git themes/pln
```

Edit `blog/_config.yml`, modify like these to use theme:
```yml
# Extensions
## Plugins: https://hexo.io/plugins/
## Themes: https://hexo.io/themes/
theme: pln
```



