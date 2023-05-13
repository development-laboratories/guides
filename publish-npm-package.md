# Publish NPM Package
By [Colin Teahan](https://www.linkedin.com/in/colin-teahan/) on **May 12th, 2023**

The following guide will teach you how to build and publish a public package to npm

|software|version|resources|
|--------|-------|--------|
|bash    |3.2.57 |[learn more](https://formulae.brew.sh/formula/bash)|
|node    |16.15.0|[learn more](https://nodejs.org/en/download)|
|npm     |7.5.2  |[learn more](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm)|
|npx     |8.5.5  |[learn more](https://www.npmjs.com/package/npx)|

# Quick Start

Create a new GitHub repository, clone it to your machine and from the pojects root directory run (*otherwise skip to next section*)

```bash
curl -O -L https://github.com/development-laboratories/scripts/releases/download/v1.0.0/dnode.sh
bash dnode.sh
```

## Create GitHub Repository

The first thing we are going to want to do is create a simple project which will become our custom pacakge. Go to [GitHub](https://github.com/) and create and account if needed. Once you are logged in let's create a new repository and clone that to your local machine.

## Installation

Navigate to your projects directory on your machine and run the following

```bash
npm install -g npx
npm init -y
npx
```

## Configuring Package.json

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

now make sure to replace the following **${ITEMS}** with your values, below is an example for this repo:

>
> Example: `"${YOUR_SCOPE}/${YOUR_REPO_NAME}"` becomes `"@development-laboratories/guide"`
>

|property|description|example|
|--------|-----------|-------|
|YOUR_SCOPE|This will either be your GitHub @username or @org and your add the @ symbol in front.| @development-laboratories |
|YOUR_REPO_NAME|The name of the repository you created earlier|guide|
|YOUR_REPO_URL|The git url of the repository you created earlier| [git@github.com:development-laboratories/guides.git](git@github.com:development-laboratories/guides.git) |
|YOUR_NAME|Any name you want to be associated|John Doe|

## Create GitHub Personal Access Token (Classic)

Go to your GitHub account and create a Personal Access Token (Classic) which can be [clicking here](https://github.com/settings/tokens) or navigating to

`Profile > Settings > Developer Settings > Personal Access Token > Classic`


