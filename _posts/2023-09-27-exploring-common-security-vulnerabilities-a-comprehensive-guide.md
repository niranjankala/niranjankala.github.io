---
title: "Unblocking Angular CLI: Resolving the PS1 Error in PowerShell"
published: true
description: "Unblocking Angular CLI: Resolving the PS1 Error in PowerShell"
tags: PowerShell
cover_image: 
canonical_url: https://niranjankala.in/post/unblocking-angular-cli-resolving-the-ps1-error-in-powershell
layout: post
---
    

## Introduction
When working with Angular CLI (Command Line Interface) and PowerShell on Windows, you may encounter an error message that says, "PS1 cannot be loaded because running scripts is disabled on this system." This error typically occurs due to the security settings on your system that prevent the execution of PowerShell scripts. However, it is essential that you continue using Angular CLI effectively. This guide will walk you through the steps to fix this error and get back to your Angular development workflow.

## Understanding the Error

Your error message is related to PowerShell's script execution policy. PowerShell has different execution policies determining whether scripts can be run and under what conditions. The default execution policy on many Windows systems is often set to "Restricted," which prevents the execution of scripts, including Angular CLI scripts.
```powershell
PS E:\DevWorkspaces\GitHub\niranjankala\ms-learn\src\dotnet-core\Summaries> ng -v
ng : File C:\Users\niran\AppData\Roaming\npm\ng.ps1 cannot be loaded. The file C:\Users\niran\AppData\Roaming\npm\ng.ps1 is not digitally signed. You cannot run this script on the current system. For 
more information about running scripts and setting execution policy, see about_Execution_Policies at https:/go.microsoft.com/fwlink/?LinkID=135170.
At line:1 char:1
+ ng -v
+ ~~
    + CategoryInfo          : SecurityError: (:) [], PSSecurityException
    + FullyQualifiedErrorId : UnauthorizedAccess
``````

## How To Fix Error "PS1 Can Not Be Loaded Because Running Scripts Is Disabled On This System" In Angular

When working with Angular CLI (Command Line Interface) and PowerShell on Windows, you may encounter an error message that says, "PS1 cannot be loaded because running scripts is disabled on this system." This error typically occurs due to the security settings on your system that prevent the execution of PowerShell scripts. However, resolving this issue is essential to continue using Angular CLI effectively. This guide will walk you through the steps to fix this error and get back to your Angular development workflow.

## Understanding the Error

Your error message is related to PowerShell's script execution policy. PowerShell has different execution policies determining whether scripts can be run and under what conditions. The default execution policy on many Windows systems is often set to "Restricted," which prevents the execution of scripts, including Angular CLI scripts.

## Solution: Change the Execution Policy

To resolve this issue, you must change the PowerShell execution policy to allow script execution. Here are the steps to do that:

1. **Open PowerShell as an Administrator:**
   - Click on the Windows Start button.
   - Search for "PowerShell."
   - Right-click on "Windows PowerShell" or "PowerShell" (depending on your Windows version) and select "Run as administrator."

2. **Check the Current Execution Policy:**
   - In the PowerShell window, type the following command and press Enter:
     ```powershell
     Get-ExecutionPolicy
     ```
   - This command will display the current execution policy. If it's set to "Restricted," you need to change it.

3. **Change the Execution Policy:**
   - To change the execution policy, you can use the following command:
     ```powershell
     Set-ExecutionPolicy RemoteSigned
     ```
     The "RemoteSigned" policy allows the execution of locally created scripts that require a digital signature for scripts downloaded from the internet.

   - You might be prompted to confirm the change. Type "Y" for Yes and press Enter.

4. **Verify the New Execution Policy:**
   - To ensure that the execution policy has been changed successfully, run the following command:
     ```powershell
     Get-ExecutionPolicy
     ```
     It should now display "RemoteSigned."

5. **Restart PowerShell:**
   - Close the PowerShell window and open a new one for the changes to take effect.

6. **Run Your Angular CLI Command:**
   - Now that you've allowed script execution, you should be able to run Angular CLI commands without encountering the "PS1 cannot be loaded" error.

## Caution

Changing the execution policy to "RemoteSigned" makes your system more permissive with regard to script execution. While this change is necessary for running Angular CLI commands, it's essential to be cautious when running scripts from untrusted sources. Ensure you run scripts from reliable and trusted locations to minimize security risks.

## Conclusion

By changing the PowerShell execution policy to "RemoteSigned," you can resolve the "PS1 cannot be loaded" error and continue using Angular CLI without issues. However, remember to exercise caution when executing scripts and only run scripts from trusted sources to maintain the security of your system. With this error resolved, you can smoothly work on your Angular projects using PowerShell.
