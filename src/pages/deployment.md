---
layout: layouts/documentation.njk
permalink: /deployment/
eleventyNavigation:
  key: Deployment
  parent: Documentation
header_title: Deployment with GitHub Actions & GitHub Pages
header_subtitle: Now that you’ve built a web site with Eleventy you might be ready to put it on the web for everyone to see! There are a bunch of different ways to do it!
---

Setting up GitHub Pages
---
# 1. Now that our repository is created, we can enable GitHub Pages. Go to the page of the repository you've just created and click on the settings button in the top right. </br></br>

# 2. Click on the Pages button on the sidebar menu and select your default branch in the dropdown, this is probably `main` or `master`, leave `/(root)` as the selected folder. Click on `save` to confirm the settings.</br></br>

Build your website with GitHub Actions
---
# 1. By default, GitHub pages uses Jekyll to generate your website. In our case, we're using Eleventy, so we want to inform GitHub Pages about this. To do this, create an empty file called .nojekyll and put it in the root directory. </br></br>

# 2. Next, create .github folder also in the root directory, and inside that folder, create another folder called workflows. In this folder, we put the workflows which GitHub Actions execute. </br></br>

# 3. We're going to create two workflows, the first one performs a build when a pull request is created. Create a file with the name `build.yml`. If your default branch is something other than main make sure to change this in the branches array.</br></br>

This workflow only performs a build and does not yet deploy your changes. This way, there is a fail-safe solution.</br></br>

```
build.yml

name: Build on PR

on:
  pull_request:
    branches: [ "main" ]

jobs:
  build:

    runs-on: ubuntu-latest

    strategy:
      matrix:
        node-version: ['14.17.6']

    steps:
    - uses: actions/checkout@v3
    - name: Use Node.js ${{ matrix.node-version }}
      uses: actions/setup-node@v3
      with:
        node-version: ${{ matrix.node-version }}
        cache: 'npm'

    - name: Install packages
      run: npm ci

    - name: Run development build
      run: npm run build:dev
```
</br>

# 4. Next create `build-and-deploy.yml`. This workflow builds and deploys our website to GitHub Pages when a push is done on main branch, for example when you merge a PR. </br></br>

```
name: Build & Deploy

on:
  push:
    branches: ['main']

jobs:
  build:
    runs-on: ubuntu-latest

    strategy:
      matrix:
        node-version: ['14.17.6']

    steps:
      - uses: actions/checkout@v3

      - name: Use Node.js ${{ matrix.node-version }}
        uses: actions/setup-node@v3
        with:
          node-version: ${{ matrix.node-version }}
          cache: 'npm'

      - name: Install packages
        run: npm ci

      - name: Run production build
        run: npm run build:prod

      - name: Deploy to gh-pages
        uses: peaceiris/actions-gh-pages@v3
        with:
          deploy_key: ${{ secrets.ACTIONS_DEPLOY_KEY }}
          publish_dir: _site

```
