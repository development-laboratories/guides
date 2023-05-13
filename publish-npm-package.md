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
  "name": "${YOUR_SCOPE}/${YOUR_REPO_NAME}",
  "version": "1.0.0",
  "description": "my first npm package!",
  "main": "index.js",
  "scripts": {
    "prepublish":
    "upload": "npm publish --scope=${YOUR_SCOPE} --registry=https://npm.pkg.github.com/",
    "test": "exit"
  },
  "repository": {
    "type": "git",
    "url": "${YOUR_REPO_URL}"
  },
  "author": "${YOUR_NAME}",
  "license": "MIT",
  "publishConfig": {
    "${YOUR_SCOPE}:registry": "https://npm.pkg.github.com"
  }
}
```
now make sure to replace the following items with your values, below is an example for this repo:

|property|description|example|
|--------|-----------|-------|
|YOUR_SCOPE|This will either be your GitHub @username or @org and your add the @ symbol in front.| @development-laboratories |
|YOUR_REPO_NAME|The name of the repository you created earlier|guide|
|YOUR_REPO_URL|The git url of the repository you created earlier| git@github.com:development-laboratories/guides.git |
|YOUR_NAME|Any name you want to be associated|John Doe|
