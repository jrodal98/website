---
title: "Automatically Deploy Hugo Site With Github Actions"
date: 2022-03-20T18:15:07-07:00
description: "Automatically Deploy Hugo Site With Github Actions"
tags: [
"hugo",
"github-actions",
"web-dev",
]
type: "post"
---

This tutorial provides a step-by-step guide on how to automatically deploy a Hugo site to GitHub pages using GitHub Actions. The tutorial covers all the necessary steps, including committing the blog source to GitHub, enabling GitHub pages, and setting up GitHub Actions to automatically deploy the site. The tutorial also includes helpful tips and troubleshooting advice, making it a useful resource for anyone looking to automate the deployment of their Hugo site.

<!--more-->

{{< toc >}}

## Step 0: Follow Hugo Quick Start

This post assumes you already followed the Hugo quick start guide, which you can find [here](https://gohugo.io/getting-started/quick-start/).

## Step 1: Commit Blog Source to Github

1. Create a new repository on Github [here](https://github.com/new).

2. Push your local repository to Github

```bash
git remote add origin URL_OF_YOUR_GITHUB_REPO
git push -u origin master
```

3. Create and push a branch called `gh-pages` - this is where the generated page will be deployed to.

```bash
git checkout -b gh-pages
git push --set-upstream origin gh-pages
git checkout master
```

## Step 2: Enable Github Pages

1. Go to your repository settings
2. Navigate to the "Code and automation" section
3. Click the "Pages" button
4. Click the "Branch" button and select "gh-pages"
5. Hit save
6. Go to `https://YOUR_USERNAME/YOUR_REPONAME`. You should see your README.

## Step 3: Deploy Hugo Site with Github Actions

Put this file at `.github/workflows/gh-pages.yml`:

```.github/workflows/gh-pages.yml {hl_lines=["6-7","31-32","36-37"]}
name: GitHub Pages

on:
  push:
    branches:
      # change this to main if that's what you use
      - master # Set a branch name to trigger deployment
  pull_request:

jobs:
  deploy:
    runs-on: ubuntu-22.04
    concurrency:
      group: ${{ github.workflow }}-${{ github.ref }}
    steps:
      - uses: actions/checkout@v3
        with:
          submodules: true # Fetch Hugo themes (true OR recursive)
          fetch-depth: 0 # Fetch all history for .GitInfo and .Lastmod

      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: "latest"
          extended: true

      - name: Build
        run: hugo --minify

      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        # change this to main if that's what you use
        if: ${{ github.ref == 'refs/heads/master' }}
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./public
          # if you are using a custom domain, put it here
          # cname: www.example.com
```

Commit the change.

## Step 4: Verify Everything is Working

- Wait a few minutes
- Go to `https://YOUR_USERNAME/YOUR_REPONAME` (or your custom domain)
- Your site should be there with new changes.
- Go into incognito mode or open a new browser if nothing has changed (it's possible your browser is returning a cached result)
