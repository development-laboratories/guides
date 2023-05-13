# Publish NPM Package
By Colin Teahan - May 12th, 2023

The following guide will teach you how to build and publish a public package to npm.

## Getting Started

This guide was written using the versions listed below, for best results use similar versions.

|npm|node|
|----|-------|
|7.5.2 |12.22.12|

The first thing we are going to want to do is create a simple project which will become our custom pacakge. Go to [GitHub](https://github.com/) and create and account if needed.

Once you are logged in let's create a new repository and clone that to your local machine, navigate to the directory, and open a bash shell then type:

```bash
npm init
```
and go through the process (we can always edit later). Once this is done open your editor of choice and add the following to your `package.json`

```bash
{
  "name": "@YOUR_SCOPE/YOUR_REPO_NAME",
  "version": "1.0.0",
  "description": "my first npm package!",
  "main": "index.js",
  "scripts": {
    "upload": "npm publish --scope=@YOUR_SCOPE --registry=https://npm.pkg.github.com/",
    "test": "exit"
  },
  "repository": {
    "type": "git",
    "url": "YOUR_REPO_URL"
  },
  "author": "YOU_NAME",
  "license": "MIT",
  "publishConfig": {
    "@YOUR_SCOPE:registry": "https://npm.pkg.github.com"
  }
}
```
