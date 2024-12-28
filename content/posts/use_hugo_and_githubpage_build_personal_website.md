+++

date = '2024-12-28T16:13:31+08:00'

draft = false

title = 'Use Hugo and Github Page Build Personal Website'

+++

# Create a site
#### Install git, hugo
 [Install Hugo](https://gohugo.io/installation/) 
 [Install Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)

#### Init setting
```
hugo new site YourSiteName
cd YourSiteName
git init
git submodule add https://github.com/theNewDynamic/gohugo-theme-ananke.git themes/ananke
echo "theme = 'ananke'" >> hugo.toml

```

#### Enable GitHub Pages and Setting hugo.yaml
Github topbar setting>page and change the source to Action
![截圖 2024-12-28 下午4.58.30](/Users/linlisa/moew_engineer_world/content/posts/use_hugo_and_githubpage_build_personal_website.assets/截圖 2024-12-28 下午4.58.30.png)

Create a file .github/workflows/hugo.yaml and paste below Hugo demo setting into the file. Make sure the branch name is align your git repo setting.
```yaml

# Sample workflow for building and deploying a Hugo site to GitHub Pages
name: Deploy Hugo site to Pages

on:
  # Runs on pushes targeting the default branch
  push:
    branches:
      - main

  # Allows you to run this workflow manually from the Actions tab
  workflow_dispatch:

# Sets permissions of the GITHUB_TOKEN to allow deployment to GitHub Pages
permissions:
  contents: read
  pages: write
  id-token: write

# Allow only one concurrent deployment, skipping runs queued between the run in-progress and latest queued.
# However, do NOT cancel in-progress runs as we want to allow these production deployments to complete.
concurrency:
  group: "pages"
  cancel-in-progress: false

# Default to bash
defaults:
  run:
    shell: bash

jobs:
  # Build job
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

  # Deployment job
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

#### Push change to your git repo
```
git add . && git commit -m 'init hugo' -a && git push
```

#### View your domain on git action:
![截圖 2024-12-28 下午4.53.17](/Users/linlisa/moew_engineer_world/content/posts/use_hugo_and_githubpage_build_personal_website.assets/截圖 2024-12-28 下午4.53.17.png)

Click the link on deply button and then you can see yourown website at https://yougitname.github.io/...

# Add New Post

Add markdown content
```
hugo new content content/posts/my-first-post.md
```

And Hugo will create a .md file content/posts/your_post_name.md, like below. And make sure you chant the deaft = true to false.
```
+++ 
date = 2024-12-28T16:13:31+08:00
draft = false 
title = 'Use Hugo and Github Page Build Personal Website' 
+++

(Add your content here ...)
```

Run below command to build your draft to html
```
hugo server --buildDrafts
```

Push change to your git repo
```
git add . && git commit -m 'new hugo content page' -a && git push
```

# Change website style



#### Change website title 

Modify hugo.toml. The baseURL notwork when deploy by github page so ignore it. Change the title if you need.

```
baseURL = 'https://example.org/'
languageCode = 'en-us'
title = 'Lisa's Engineer World'
theme = 'ananke'
```



#### Change Theme

Go to [Hugo theme](https://themes.gohugo.io/) and select s style you like and select ont them you like. Click the down load button will see the install instruction.



In most situation like you can download it by git like:

```
git clone https://github.com/adityatelange/hugo-PaperMod themes/PaperMod --depth=1
```



Push to repo
