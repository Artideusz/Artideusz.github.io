+++
date = '2024-12-02T20:57:47+01:00'
draft = false
title = 'Playing with Hugo - Part 1'
menus = 'main'
tags  = ['miscellaneous']
+++

# What is Hugo?

Hugo is a static site generator written in Go. It provides a very easy way to create static sites. It has good and painless integration with Github Pages.

# Installing hugo

Hugo is available as an official package in the [arch repository](https://archlinux.org/packages/extra/x86_64/hugo/). It can be installed via:

```
sudo pacman -S hugo
```

After that is installed, you can generate a new site using the following command:

```
hugo new site <site_name>
```

This should generate a new folder with the contents of the site.

# Importing a theme for your site

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

# Creating a new post

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

# Combining Hugo with Github Pages

I mainly followed the steps provided by [hugo](https://gohugo.io/hosting-and-deployment/hosting-on-github/), which are the following:

1. Create a new repository on Github.
    - Name the repository `<github username>.github.io`
2. Import the Hugo repository to Github. You can do this by invoking the following commands:
    - `git init .`
    - `git add .`
    - `git commit -m "Initial commit"`
    - `git remote add origin git@github.com:<Author>/<new repository>.git`
    - `git push -u origin master`
3. In the github repository, go to `Settings > Pages > Source` and change `Deploy from a branch` to `Github Actions`.
4. Create a new file: `.github/workflows/hugo.yml`. If you do not have the folders, create them.
5. Paste the following configuration:

```
name: Deploy Hugo site to Pages

on:
  push:
    branches:
      - master

  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: "pages"
  cancel-in-progress: false

defaults:
  run:
    shell: bash

jobs:
  build:
    runs-on: ubuntu-latest
    env:
      HUGO_VERSION: 0.137.1
    steps:
      - name: Install Hugo CLI
        run: |
          wget -O ${{ runner.temp }}/hugo.deb https://github.com/gohugoio/hugo/releases/download/v${HUGO_VERSION}/hugo_extended_${HUGO_VERSION}_linux-amd64.deb \
          && sudo dpkg -i ${{ runner.temp }}/hugo.deb          
      - name: Install Dart Sass
        run: sudo snap install dart-sass
      - name: Checkout
        uses: actions/checkout@v4
        with:
          submodules: recursive
          fetch-depth: 0
      - name: Setup Pages
        id: pages
        uses: actions/configure-pages@v5
      - name: Install Node.js dependencies
        run: "[[ -f package-lock.json || -f npm-shrinkwrap.json ]] && npm ci || true"
      - name: Build with Hugo
        env:
          HUGO_CACHEDIR: ${{ runner.temp }}/hugo_cache
          HUGO_ENVIRONMENT: production
          TZ: America/Los_Angeles
        run: |
          hugo \
            --gc \
            --minify \
            --baseURL "${{ steps.pages.outputs.base_url }}/"          
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: ./public

  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

6. Push the change to Github.
    - `git add .`
    - `git commit -m "Added workflow"`
    - `git push`
7. Wait until the workflow finishes running.
8. Verify if the page is live by going to the following page: `https://<username>.github.io`.

And that's all! You should now have your own blog set up and running on Github Pages!

# Modifying the content of the hugo site

When using an imported theme, you may want to modify some aspects of the theme, such as:

- Colors.
- The page title.
- Other information related to the site itself.

Usually, imported themes provide a way to change the properties of the site by populating pre-built or custom field in the `hugo.toml` file. For example:

You can change the color of your theme by appending the following content to your theme (assuming you use the `m10c` theme):

```
[params.style]
  darkestColor = "#0f0f0f"
  darkColor = "#212121"
  lightColor = "#eeeeee"
  lightestColor = "#22dd22"
  primaryColor = "#00aa00"
```

You can find more information about configuration options [here](https://gohugo.io/getting-started/configuration/#all-configuration-settings), in the theme documentation or in the theme source code.

## Menus

You can also add menus by adding the `menus` attribute in a post. For example:

```
+++
date = '2024-12-02T20:57:47+01:00'
draft = false
title = 'Playing with Hugo - Part 1'
menus = 'main'
+++
```

More in the next part!

## Resource links

- https://gohugo.io/hosting-and-deployment/hosting-on-github/
- https://gohugo.io/content-management/front-matter/
- https://gohugo.io/getting-started/configuration/#all-configuration-settings
- https://gohugo.io/content-management/menus/