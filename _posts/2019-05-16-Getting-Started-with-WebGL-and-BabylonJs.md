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

In this article, we will learn deploy an Angular application to GitHub Pages using npm angular-cli-ghpages package to easily .

## Prerequisites:


This command has the following prerequisites for Installation & Setup:

- `Node.js 8.2.0` or higher which brings you `npm 5.2.0` which brings you [`npx`](https://medium.com/@maybekatz/introducing-npx-an-npm-package-runner-55f7d4bd282b) 
- Git 1.7.6 or higher
- __optional__: Angular project created via [angular-cli](https://github.com/angular/angular-cli)

- An Angular 5 or above version application, which is working and ready to host.
If it is not ready then follow the instructions specified in the below link for adding an existing angular project to Github.
[Adding an existing project to GitHub using the command line](https://help.github.com/en/articles/adding-an-existing-project-to-github-using-the-command-line)

#### References:
[Deploy to GitHub Pages](https://github.com/angular/angular-cli/wiki/stories-github-pages)
[]()




# Steps to deploy to GitHub pages


To install the command run the following:

```bash
npm install -g angular-cli-ghpages
```
``` 

ng build --prod --base-href https://[username].github.io/[repo]/
ngh --dir=dist/[project-name] //
```
In order to compile images correctly use path as following:

```
'./assets/images/image.png'
```
- 
## What is WebGL?

Definition from: [WebGL - Mozilla] (https://developer.mozilla.org/en-US/docs/Web/API/WebGL_API) and [khronos](https://www.khronos.org/webgl/)
WebGL (Web Graphics Library) is a JavaScript API for rendering interactive 3D and 2D graphics within any compatible web browser without the use of plug-ins. WebGL does so by introducing an API that closely conforms to OpenGL ES 2.0.

1. It is a cross-platform, free web standard for 3D graphics API based on OpenGL ES. 
2. It is exposed to ECMAScript via the HTML5 Canvas element. Developers familiar with OpenGL ES 2.0 will recognize WebGL as a Shader-based API using GLSL, with constructs that are semantically similar to those of the underlying OpenGL ES API. 
3. It stays very close to the OpenGL ES specification, with some concessions made for what developers expect out of memory-managed languages such as JavaScript. 
4. WebGL 1.0 exposes the OpenGL ES 2.0 feature set; WebGL 2.0 exposes the OpenGL ES 3.0 API.
WebGL brings plugin-free 3D to the web, implemented right into the browser. Major browser vendors Apple (Safari), Google (Chrome), Microsoft (Edge), and Mozilla (Firefox) are members of the WebGL Working Group.

It is maintained by [khronos](https://www.khronos.org/webgl/).

## What is Babylon.js?

Babylon.js is a Powerful, Beautiful, Simple, Open source Web-Based 3D WebGL-based graphics engine.

**Pros:**
 1. Equipped with the powerful Inspector.
 2. Best in class physically-based-rendering, countless optimizations.
 3. Powerful, beautiful, simple, and open 3D to everyone on the web.




### 3D engine vocabulary or terms:
* A point in the 3D world = **vertex**.
* Multiple vertex = **vertices**.
* **Vector3** (x,y,z) is used for a 3D position or a direction.
* Triangle = **face**
* 3D object = **mesh**

# Setting up release build for Android platform


# Conclusion
There are the steps to create a build for ionic 4 application and then you can host you application on Android store.
