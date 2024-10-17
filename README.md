---
title: "Automating npm Package Management with Semantic Release"
source: https://gist.ly/youtube-summarizer/automating-npm-package-management-with-semantic-release
date: 2024-10-17 05:57
tags: []
---

[![semantic-release: angular](https://img.shields.io/badge/semantic--release-angular-e10079?logo=semantic-release)](https://github.com/semantic-release/semantic-release)

# Lab: Automating npm Package Management with Semantic Release

#### Tool: [Semantic-release](https://github.com/semantic-release/semantic-release) 

Lab on how to automate npm package workflow using Semantic Release. Create, maintain, update, and distribute packages seamlessly with plugins for Git and changelogs. Complete guide from project setup to GitHub and npm release.

![](https://i.ytimg.com/vi/0Z0nAJjmQRg/maxresdefault.jpg)

Reference: [By Adam S. video](https://www.youtube.com/watch?v=0Z0nAJjmQRg) - 5 min read
    


## Introduction

Creating and maintaining an npm package can be a daunting task, particularly with the numerous steps involved in the publishing process. Enter **semantic release**, a tool that automates the entire npm package workflow'from creating, maintaining, and updating packages to handling the versioning and distribution. In this comprehensive guide, we will walk you through the process of setting up semantic release to manage your npm package publishing seamlessly.

------------------------------------------------------------------------

## What is Semantic Release?

Semantic release is a tool designed to automate the entire package publishing workflow. According to their own words, semantic release automates the entire package workflow, including:

- Creating the package
- Distributing the package
- Calculating the next version
- Updating the changelog

Moreover, it comes with a plethora of plugins to customize and extend its functionality, making it an excellent tool for developers who aim for a streamlined and automated release process.

In order to use **semantic-release** you must [follow these steps](https://semantic-release.gitbook.io/semantic-release/usage/getting-started).

------------------------------------------------------------------------

# Lab: step-to-step


### 1. Setup the Project Directory

First, you'll need to create a new directory for your npm package. Open your terminal and run the following commands:

``` sh
mkdir my-npm-package
cd my-npm-package
npm init -y
git init

// also configure a remote 
```

These commands create a new directory, initialize an npm package with default settings, and also initialize a Git repository.

### 2. Create the Entry File

Next, create an `index.js` file at the root of your project. This will be the entry point of your package. Add some basic code for demonstration purposes:

``` js
const message = "Hello, world!";
console.log(message);
```

### 3. Add `.gitignore`

Make sure to add a `.gitignore` file to exclude the `node_modules` directory and other unnecessary files:

    node_modules

------------------------------------------------------------------------

### 4. Update `package.json`

Your `package.json` file is the core of your npm package. Update a few key fields in this file:

1.  **author**: Add your name and email.
2.  **name**: Ensure the package name is unique.
3.  **description**: Add a description for your package.

Example `package.json`:

``` json
{
  "name": "my-unique-package-name",
  "version": "1.0.0",
  "description": "A demo package for semantic release",
  "main": "index.js",
  "author": "Your Name <>"
}
```

------------------------------------------------------------------------

### 5. Install Semantic Release (and optional not baked-in plugins)

Install semantic release:


``` sh
pnpm add -D semantic-release
```

And its necessary plugins as development dependencies:
``` sh
pnpm add -D @semantic-release/git @semantic-release/changelog
```

### 6. Configuration File

Create a configuration file named `.releaserc.json` in the root of your project. This file will contain settings for semantic release:

``` json
{
  "branches": [
    "main"
  ],
  "plugins": [
    // baked-in plugins
    "@semantic-release/commit-analyzer",
    "@semantic-release/release-notes-generator",
    "@semantic-release/github",
    "@semantic-release/npm",

    // optional plugins
    "@semantic-release/changelog",
    "@semantic-release/git"
  ],
  "preset": "angular",
  "pluginsConfig": {
    "@semantic-release/git": {
      "assets": [
        "CHANGELOG.md",
        "package.json",
        "pnpm-lock.json"
      ]
    }
  }
}
```

------------------------------------------------------------------------

### 7. Create GitHub Workflow

In order to automate the release process, create a GitHub action workflow. Create a file named `.github/workflows/release.yml` with the following content:

``` yaml
name: Release
on:
  push:
    branches:
      - main
jobs:
  release:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v4
      - name: Install dependencies
        run: npm install
      - name: Run semantic release
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          NPM_TOKEN: ${{ secrets.NPM_TOKEN }}
        run: npx semantic-release
```

------------------------------------------------------------------------

## 9. Set Up GitHub and npm Credentials

### 9.1  Generate GitHub Token

Go to GitHub Actions Settings and add write permission to to default GITHUB_TOKEN.

### 9.2 Generate npm Token (only if you want ot deploy to npm)

Log into your npm account and generate a new token:

1.  Go to npmjs.com
2.  Click on your profile and navigate to `Access Tokens`
3.  Create a new token with automation privileges

### 9.3. Add Tokens to GitHub Secrets

1.  Go to your GitHub repo settings
2.  Navigate to `Secrets` \> `Actions`
3.  Add a new secret named `NPM_TOKEN` and paste the npm token value
4.  The `GITHUB_TOKEN` will be automatically present in the GitHub Actions environment.

------------------------------------------------------------------------

### 10. Commit and Push

Now commit your changes and push them to GitHub:

``` sh
git add .
git commit -m "feat: initial commit"
git push origin main
```

------------------------------------------------------------------------

## Addressing Potential Issues

After pushing, the GitHub action should trigger. However, you might encounter permission issues. If this happens, go to your GitHub repo settings and ensure that under `Actions` \> `General`, the `Workflow permissions` is set to `Read and write`.

------------------------------------------------------------------------

## Handling Semantic Versioning

Semantic release uses conventional commits to determine the next version of your package:

- **fix**: Patch release
- **feat**: Minor release
- **BREAKING CHANGE**: Major release

#### Example Conventional Commit

``` sh
git commit -m "feat: add new feature"
```

------------------------------------------------------------------------

## Adding a README

A good npm package should always have a comprehensive README. Create a `README.md` file in the root of your project:

``` markdown
# My NPM Package
This is a demo npm package to illustrate semantic release usage.
```

Commit this file:

``` sh
git add README.md
git commit -m "docs: add README"
git push origin main
```

This will trigger semantic release to publish a new version if using a conventional commit message.

------------------------------------------------------------------------

## Conclusion

By following this guide, you should now have a fully automated npm package release process using semantic release. Whether you are creating an npm package from scratch or maintaining an existing one, semantic release simplifies the entire process, allowing you to focus more on code and less on configuration and deployment hassles.

This comprehensive automation not only saves developers' time but also enhances consistency and reliability in publishing npm packages. For more detailed steps and ready-to-use code snippets, make sure to check out the associated blog post linked below.

Happy Coding!
