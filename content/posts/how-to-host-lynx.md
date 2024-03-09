---
title: "How to Host Lynx on GitHub Pages"
author: "Hayden Plumley"
date: "2022-06-24"
lastmod: "2024-03-09"
description: "How to setup & host Lynx on GitHub Pages."
tags: ["Linux","MacOS"]
categories: ["Tutorials"]
---

[Lynx](https://github.com/jpanther/lynx) is a Hugo theme designed to be a simple links page, similar to Linktree. Lynx differs by being extremely simple and performant. In this tutorial, I will tell you how to host your own Lynx site on GitHub Pages.

![Lynx Preview](../../images/lynx.webp)

## Installing Hugo
To progress, you will need to install Hugo and Git. [Hugo](https://gohugo.io) is an open-source static site generator known for its speed and flexibility, it will be the base for our GitHub Pages site. Git is a command-line tool used to interface with the Git system.

``` shell
sudo apt install hugo git
# or MacOS
brew install hugo git
```

## Setting Up Your Site
Now let's set up your site and add the Lynx theme. First, create a site with Hugo's `new site` command. Next open your site's folder and initialize it, once completed add the Lynx theme as a submodule. This will create a symlink to the Lynx repository, so new versions will be pulled when it's updated.

``` shell
# Create site named "lynx"
hugo new site lynx

# Change directory to lynx
cd lynx

# Initialize folder & add theme as a submodule so it gets updated as Lynx does.
git init && git submodule add -b stable https://github.com/jpanther/lynx.git themes/Lynx
```

## Example Config
Next, copy this base config to `config.toml` and tweak to your liking. You can preview your site by running `hugo server` and clicking the link it generates (e.g. localhost:1313). Full configuration options [here](https://github.com/jpanther/lynx/blob/stable/config.toml).

``` toml
# Site Config
baseURL = "https://username.github.io" # Add /reponame for project site
languageCode = "en-us"
title = "name | Lynx"
theme = "Lynx"

enableRobotsTXT = true
[outputs]
  home = ["HTML"]

# SEO
[params]
  env = "production"
  title = "page title"
  author = "Your name here"
  description = "My social links, powered by Hugo & Lynx"
  keywords = [ "Lynx", "Linktree", "About" ]

# Lynx Config
[author]
  image = "img/image.jpg" # Image path or link
  name = "Your name here"
  headline = "Description/bio"
  links = [
    { github = "https://github.com/username" },
    { x = "https://x.com/username" },
    { email = "mailto:hello@your_domain.com" },
    { link = "https://link-to-some-website.com" }
  ]

enableEmoji = true
disableKinds = ["taxonomy", "term"]

[markup.highlight]
  noClasses = false
[markup.goldmark.renderer]
  unsafe = true
```

## Adding GitHub Workflow & Deploying
Once you feel like your site is complete, save this workflow file to `.github/workflows/gh-pages.yml` in your site's folder. Now all that's left to do is upload your site-folder's contents to GitHub and set GitHub Pages' deployment branch to `gh-pages`.

``` yaml
name: github pages

on:
  push:
    branches:
      - main  # Set a branch to deploy
  pull_request:

jobs:
  deploy:
    runs-on: ubuntu-20.04
    steps:
      - uses: actions/checkout@v2
        with:
          submodules: true  # Fetch Hugo themes (true OR recursive)
          fetch-depth: 0    # Fetch all history for .GitInfo and .Lastmod

      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: 'latest'
          extended: true

      - name: Build
        run: hugo --gc --minify

      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        if: github.ref == 'refs/heads/main'
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./public
```
