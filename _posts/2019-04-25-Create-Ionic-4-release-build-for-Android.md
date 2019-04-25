---
title: "Create Ionic 4 release build for Android"
published: true
description: "Create Ionic 4 release build for Android"
tags: ionic, ionic 4
cover_image: 
canonical_url: http://niranjankala.in/post/Create-Ionic-4-release-build-for-Android
layout: post
---

# Introduction

In this article, you will learn how to create ionic 4 release build for Android platform.

# Setting up release build for Android platform

## Steps 1 

Run "npm install" on root directory
## Steps 2 

Add android platform with the CLI :
```
ionic platform add android
```
## Steps 3 
If you are working in distributed development environment then make sure to install required files. Run  [ionic cordova prepare](https://ionicframework.com/docs/cli/commands/cordova-prepare) command. it will Install platforms and plugins listed in "config.xml". 
```
ionic cordova prepare android**
```
## Steps 4
Now navigate to platforms/android with the CLI :cd platforms/android
	
Create/Copy release signing key store file under platforms/android folder
Copy the created Key store file for android application if you already have otherwise generate a key.store file with the CLI and answer all the questions:
```
keytool -genkey -v -keystore YourApp.keystore -alias YourApp -keyalg RSA -keysize 2048 -validity 10000
```
Follow the steps suggested in the documentation for [Deploying to a Device](https://ionicframework.com/docs/v3/intro/deploying/).

### Create/Specify release signing information

#### Steps 1
Create a file with name "release-signing.properties" under "platforms\android" folder 
#### Steps 2
 Add below information to this file:
 ```
key.store=YourApp.keystore
key.store.password=<YourApp keystore password>
key.alias=YourApp
key.alias.password=<YourApp alias password>
```
# Create build for application

Now go back to the root of your Ionic project with the CLI and build a release version:
```
ionic cordova build android --prod --release
```
If command run successfully then you will find release APK under -- APP_Root_Folder\platforms\android\app\build\outputs\apk\release


### Reset plugins and platforms


To install Or reinstall all cordova plugins In Package.json with Ionic, Run this command on windows command prompt with administrator privilege 
```
rd  plugins /d/s && rd platforms /d/s  && ionic cordova prepare
```


# Conclusion
There are the steps to create a build for ionic 4 application and then you can host you application on Android store.
