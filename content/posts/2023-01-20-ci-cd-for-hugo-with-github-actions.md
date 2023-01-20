---
title: 'CI/CD for Hugo with GitHub Actions'
date: 2023-01-20T06:00:00+00:00
author: Marcel Jurtz
layout: post
permalink: /2023/01/ci-cd-for-hugo-with-github-actions
categories:
  - devops
tags:
  - webdevelopment
  - devops
  - development
  - hugo
  - tools
---

I've been moving this site to hugo quite a while ago, but only recently stumbled across the idea to create a workflow on GitHub Actions to build a hugo site straight after each push. Contrary to many other articles however, I didn't have the option to publish the page to GitHub Pages or similar, simply because the project still is under heavy construction.

So, I had the following requirements:
* Start workflow after each push, but also enable manual triggering for testing purposes
* Build the site, considering it requires additional dependencies due to relying on Googles Docsy-Theme
* Bonus: Use a tool to verify the integrity of all the internal links and images to verify correct paths

Let's keep it simple: I started with a regular setup of the repo on an up-to-date Ubuntu container:

```yaml
name: Build Hugo and Publish as Release

on: [ push, workflow_dispatch ]

jobs:
  build:
    name: Build and Deploy
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Repository
        uses: actions/checkout@v2
```

The triggers specify that the action is executed on each push, and also manually via the GitHub Website (`workflow_dispatch`). The next step after checkout is to setup hugo. Since I'm relying on the Docsy-Theme, I need the extended version of hugo. This also includes Node DevDependencies, which requires to install these before building the website. If you only need the regular version of hugo, adjust the URL accordingly and remove the Node-Modules step from the workflow.

```yaml
- name: Setup Hugo
  env:
    HUGO_VERSION: 0.101.0
  run: |
    mkdir ~/hugo
    cd ~/hugo
    curl -L "https://github.com/gohugoio/hugo/releases/download/v${HUGO_VERSION}/hugo_extended_${HUGO_VERSION}_Linux-64bit.tar.gz" --output hugo.tar.gz
    tar -xvzf hugo.tar.gz
    sudo mv hugo /usr/local/bin

- name: Setup Node-Modules / Dependencies
  run: npm i
```

After these two steps, the environment is all set up. Subsequently, I need to run the hugo build to generate my site. I customized the builds output directory to be able to specify that later for the verification of used resources:

```yaml
- name: Build Site
  run: hugo --minify -d $GITHUB_WORKSPACE/dist
```

So far, so good. The build is now running as it should. As I said, I also wanted to include a way to verify the internal references - links to other pages as well as images. While searching for an option to achieve that, I stumbled across ![htmltest](https://github.com/wjdp/htmltest). This tool looked to exactly support my requirements, so I decided to set it up. It also includes a prebuilt action you can use for your own workflows.

To use the tool, you simply give it a source directory to analyze and receive results for your page after a few seconds. Per default, it also includes verification of external links, but you can turn this off via configuration.

I decided to add another step to the workflow to store the analyzation results as an artifact. This allows me to review the results later without having to scroll through the workflow-log. Also, note that I set the `continue-on-error` flag to `true`, which makes the build finish successfully even if htmltest finds issues with the site. Remove that line if you prefer crashing the workflow on these errors.

```yaml
- name: Run HtmlTest
  continue-on-error: true
  uses: wjdp/htmltest-action@master
  with:
    config: .htmltest.yml

- name: Archive htmltest results
  uses: actions/upload-artifact@v3
  with:
    name: htmltest-report
    path: tmp/.htmltest/htmltest.log
    retention-days: 7
```

HtmlTest relies on a config-file (.htmltest.yml), which I added to the main repository folder. For an overview of all available config options, see the [GitHub-Repo](https://github.com/wjdp/htmltest).

```yaml
DirectoryPath: "dist"
IgnoreDirectoryMissingTrailingSlash: true
CheckExternal: false
```

And that's it! Here's the full config file:

```yaml
name: Build Hugo and Publish as Release

on: [ push, workflow_dispatch ]

jobs:
  build:
    name: Build and Deploy
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Repository
        uses: actions/checkout@v2

      - name: Setup Hugo
        env:
          HUGO_VERSION: 0.101.0
        run: |
          mkdir ~/hugo
          cd ~/hugo
          curl -L "https://github.com/gohugoio/hugo/releases/download/v${HUGO_VERSION}/hugo_extended_${HUGO_VERSION}_Linux-64bit.tar.gz" --output hugo.tar.gz
          tar -xvzf hugo.tar.gz
          sudo mv hugo /usr/local/bin

      - name: Setup Node-Modules / Dependencies
        run: npm i

      - name: Build Site
        run: hugo --minify -d $GITHUB_WORKSPACE/dist

      - name: Run HtmlTest
        continue-on-error: true
        uses: wjdp/htmltest-action@master
        with:
          config: .htmltest.yml

      - name: Archive htmltest results
        uses: actions/upload-artifact@v3
        with:
          name: htmltest-report
          path: tmp/.htmltest/htmltest.log
          retention-days: 7
```