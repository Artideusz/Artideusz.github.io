+++
date = '2024-12-02T20:57:47+01:00'
draft = true
title = 'Hugo Journey 00'
+++

# What is Hugo?

Hugo is a static site generator written in Go. It provides a very easy way to create static sites. 

## Installing hugo

Hugo is available as an official package in the [arch repository](https://archlinux.org/packages/extra/x86_64/hugo/). It can be installed via:

```
sudo pacman -S hugo
```

After that is installed, you can generate a new site using the following command:

```
hugo new site <site_name>
```

This should generate a new folder with the contents of the site.

## Importing a theme for your site

I used the [m10c](https://themes.gohugo.io/themes/hugo-theme-m10c/) theme created by [Vaga](https://vaga.io/) for this blog. I imported it by running the following command in the root site directory:

```
git submodule add https://github.com/<author>/<theme>.git themes/<theme>
```

This will import the theme contents to your site.

To use it, go to `hugo.toml` and add the following entry:

```
theme = '<theme>'
```

That's all!

## Creating a new post

You can create a new post using the following command:

```
hugo new content content/posts/<post_name>.md
```

This will create a new file with hugo variables setup for you:

```
+++
date = '2024-12-02T20:57:47+01:00'
draft = true
title = '<post_name>'
+++
```

The area between the plus signs is called the [Front matter](https://gohugo.io/content-management/front-matter/), and it is metadata that basically integrates your content with the functionality of Hugo. It describes:

- The date of a post.
- The display title of the post.
- The summary for the post.
- Custom fields using [parameters](https://gohugo.io/content-management/front-matter/#parameters).
- And many other fields.

## Combining Hugo with Github Pages

I mainly followed the steps provided by [hugo](https://gohugo.io/hosting-and-deployment/hosting-on-github/), which are the following:

1. Create a new repository on Github.
2. Import the Hugo repository to Github. You can do this by invoking the following commands:
    - `git init .`
    - ``

## Resource links

- https://gohugo.io/hosting-and-deployment/hosting-on-github/
- https://gohugo.io/content-management/front-matter/