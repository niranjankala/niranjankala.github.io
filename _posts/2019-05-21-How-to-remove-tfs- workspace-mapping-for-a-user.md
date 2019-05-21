---
title: "How to remove TFS workspace mapping for a user"
published: true
description: "How to remove TFS workspace mapping for another user"
tags: tfs
cover_image: 
canonical_url: http://niranjankala.in/post/how-to-remove-tfs- workspace-mapping-for-a-user
layout: post
---
    
## Introduction

In this article, you will know how to remove TFS workspace mapping for a different user. In a remote environment, multiple users log in to a remote machine and create their workspaces that cause an access conflict for mapped workspace folder.

## Scenario

Today I faced an issue while trying to update a mapped TFS workspace in Visual Studio 2017. Visual Studio stops responding if I try to open "Source Control Explorer" and found that some different TFS user connects to TFS and mapped some folders on my local drive. Now I need to access that mapped workspace folder because I do not want to create another folder for myself. 

I tried mapping same remote folder to my existing local folder and I got the following error:

"The working folder 'Workspace_Folder_Local_Path' is already in use by the workspace WORKSPACE_NAME:USER_NAME on computer 'MACHINE_NAME'"

## Remove TFS workspace user mapping

### Prerequisites:

- You should have administrative rights to the collection.
- TF command. ( it is located at "C:\Program Files (x86)\Microsoft Visual Studio\2017\Professional\Common7\IDE" depend upon your Visual Studio version.


### Steps to remove user workspace mapping
- Run "Developer Command Prompt for VS 2017" from Start menu.
- List the workspaces associated with the user using the below command:
    ```
    TF workspaces /collection:"http://tfsserver:8080/tfs/collection_name" /owner:owner_id
    ```
  This will return the list of workspaces owned by the user and computer they are associated with.
For owner_id, you use the user name e.g. Niranjan Singh
- To remove user workspace mapping, run the below command:
    ```
    tf workspace /delete workspacename;owner_id 
    ```

  Now it will confirm you to delete the user mapping. Enter  `'y'` to initiate the process.

  ![Remove TFS Workspace Mapping for a user](https://2.bp.blogspot.com/-_FBeQEDjD3k/XOOsMlseoHI/AAAAAAAABuE/zHq-ICKUDvMXu0uJt15vtOx4ubkBqV21gCLcBGAs/s400/Remove%2BTFS%2Bmapping.PNG)

# Conclusion
These are the steps to delete the TFS workspace mapping for a user.
