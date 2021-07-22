---
title: "Azure DevOps Server - How to fix indexing isn't working issue?"
published: true
description: "Azure DevOps Server - How to fix indexing isn't working issue?"
tags: azuredevopsserver
cover_image: 
canonical_url: http://niranjankala.in/post/azure-devops-server-how-to-fix-indexing-isn-t-working-issue
layout: post
---
    
### Introduction

In this article, you will learn how to fix the Azure DevOps Server indexing issues. 


### How to fix indexing isn't working, or is in progress issue


In our scenario, the Search was not working and was completely broken. Nobody was able to search in the code and work items. 

To fix this, we have referenced the Microsoft documentation - [Manage Search indexing](https://docs.microsoft.com/en-us/azure/devops/project/search/manage-search?view=azure-devops-2020) to create the search index again.

Below are the step to fix the search indexing:
* Download the scripts from the [Code-Search GitHub repository](https://github.com/Microsoft/Code-Search) on the server.
* Extract the zip somewhere and open the _Powershell_ in _Administrative_ mode.
* Change the directory to your Azure DevOps Server version. I have to reindex the entire collection. 
* Now execute the script TriggerCollectionIndexing.ps1 to reindex the collection but first, you need to change the policy to execute the command
```powershell
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass
```
* After that run the `TriggerCollectionIndexing.ps1` file. You need to enter the SQL server instance name of the Azure DevOps Server, Collection database name, Configuration database name and the entities to reindex.



![](https://1.bp.blogspot.com/-G6L6aIVp81Y/YPj8jD4AeBI/AAAAAAAAB1o/dX3YlglLLJUaZNfjLblaHylphuUA3dWIwCLcBGAsYHQ/w640-h374/Azure-DevOps-Search-Index-Fix.jpg)


### Conclusion
Search indexing is important feature in the Azure DevOps Server, use the scripts to get the status of the search indexing and fix the issues.  