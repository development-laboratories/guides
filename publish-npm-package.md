# Publish NPM Package
By Colin Teahan - May 12th, 2023

The following guide will teach you how to build and publish a public package to npm.

## Quick Start

Clone a GitHub repository you own and paste the following into a file called `quick-start.sh`
<detail>
  <summary>Click to view source code</summary>
```bash
#!/bin/bash
#
# create a new file named dnode.sh and paste this file into it,
# then run the following command in the terminal:
#
#     bash quick-start.sh
#

# add node_modules to .gitignore
echo "node_modules/" > .gitignore
echo "dist/" >> .gitignore

# create subdirectories
mkdir src
mkdir dist
mkdir test

# create a hellow world typescript file
echo "console.log('Hello World');" > src/index.ts

# create tsconfig.json
echo '{
  "compilerOptions": {
    "outDir": "./dist/",
    "target": "es5",
    "module": "commonjs",
    "strict": true,
    "esModuleInterop": true
  },
  "exclude": [
    "node_modules/**/*"
  ]
}' > tsconfig.json

# install jest
# npm i -d jest ts-jest @types/jest

# initialize npm and install typescript
echo '{
  "name": "dnode",
  "version": "1.0.0",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "tsc",
    "start": "node dist/index.js"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "@types/jest": "^29.5.1",
    "jest": "^29.5.0",
    "ts-jest": "^29.1.0",
    "typescript": "^5.0.4"
  },
  "description": ""
}' > package.json

# build & start the project
npm install
npm run build
npm run start
```
</details>

and the run the script with the following command

```bash
bash quick-start.sh
```

## Getting Started

This guide was written using the versions listed below, for best results use similar versions.

|npm|node|
|----|-------|
|7.5.2 |12.22.12|

## Quick State

Clone your GitHub repo to your local machine and run the following scipt:

```bash
npm init -y
```

## Create GitHub Repository

The first thing we are going to want to do is create a simple project which will become our custom pacakge. Go to [GitHub](https://github.com/) and create and account if needed.

Once you are logged in let's create a new repository and clone that to your local machine.

# Initialize Project

Navigate to the directory on your machine, open a bash shell then type:

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

now make sure to replace the following ${ITEMS} with your values, below is an example for this repo:

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

or by cli

