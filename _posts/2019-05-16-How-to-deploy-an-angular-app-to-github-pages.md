---
title: "How to deploy an Angular app to Github Pages"
published: true
description: "How to deploy an Angular app to Github Pages"
tags: angular github
cover_image: 
canonical_url: http://niranjankala.in/post/how-to-deploy-an-angular-app-to-github-pages
layout: post
---
    
## Introduction

In this article, you will learn to deploy an Angular application to GitHub Pages using npm angular-cli-ghpages package to easily.

## Prerequisites:


This command has the following prerequisites for Installation & Setup:

- `Node.js 8.2.0` or higher which brings you `npm 5.2.0` which brings you [`npx`](https://medium.com/@maybekatz/introducing-npx-an-npm-package-runner-55f7d4bd282b) 
- Git 1.7.6 or higher
- __optional__: Angular project created via [angular-cli](https://github.com/angular/angular-cli)

- An Angular 5 or above version application, which is working and ready to host.
If it is not ready then follow the instructions specified in the below link for adding an existing angular project to GitHub.
[Adding an existing project to GitHub using the command line](https://help.github.com/en/articles/adding-an-existing-project-to-github-using-the-command-line)

## References:
[Deploy to GitHub Pages](https://github.com/angular/angular-cli/wiki/stories-github-pages)
[angular-cli-ghpages](https://github.com/angular-schule/angular-cli-ghpages/blob/master/README.md)   
[Deploying Angular Apps with GitHub Pages](https://rhythmandbinary.com/2019/01/05/deploying-angular-apps-with-github-pages/)



# Steps to deploy to GitHub pages


To install the command run the following:

```bash
npm install -g angular-cli-ghpages
```

It is just two command to publish your Angular application to GitHub pages.
``` 
ng build --prod --base-href https://[username].github.io/[repo]/
ngh --dir=dist/
```



### Deploying using the Angular npm scripts

You can also automatically publish an application using npm by setting script in package.json. The build and deploy command in one go by following the below approach:

To install the command as your project dependencies run the following:
```
npm i angular-cli-ghpages --save-dev
```

Open your package.json and then, in your script section add the following script to deploy an Angular 7 application.
```
{
  "name": "ng-webgl",
  "version": "0.0.0",
  "scripts": {
    "ng": "ng",
    "start": "ng serve",
    "build": "ng build",
    "test": "ng test",
    "lint": "ng lint",
    "e2e": "ng e2e",
    "deploy:gh": "ng b --prod --base-href https://niranjankala.github.io/ng-webgl/ && npx ngh --dir=dist/"
  },
```

To execute this deploy script. Run `npm run deploy:gh` on the root of your project directory.

![Publish Github-pages](https://2.bp.blogspot.com/-aCz-PHtogiY/XNz-_7tyEtI/AAAAAAAABtw/A7hhgRBHIy4PBvS5jfimToSYqXxJketXwCLcBGAs/s320/Publish%2BGithub-pages.PNG)

Note: In order to compile images correctly use the relative path `'./assets/images/image.png'`




# Conclusion
There are the steps to publish Angular application the GitHub pages.
