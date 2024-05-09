---
title: "Unleashing the Power of Azure AI: A Comprehensive Guide to Text-to-Speech Applications"
published: true
description: "Unleashing the Power of Azure AI: A Comprehensive Guide to Text-to-Speech Applications"
tags: dotnet, azure
cover_image: 
canonical_url: https://niranjankala.in/post/unleashing-the-power-of-azure-ai-a-comprehensive-guide-to-text-to-speech-applications
layout: post
---


**Introduction:**

In the dynamic landscape of application development, efficiency and cost-effectiveness are paramount. For developers working on projects that involve video narration, traditional methods of hiring vocal talent and managing studio resources can be both cumbersome and expensive. Enter Microsoft's Azure AI services, offering a suite of APIs that empower developers to integrate cutting-edge text-to-speech capabilities into their applications. In this comprehensive guide, we'll delve into the intricacies of Azure AI, providing step-by-step instructions and code snippets to help you harness its full potential for creating text-to-speech applications.

**Objective:**
- Establish an Azure AI services account
- Develop a command-line application for text-to-speech conversion using plain text
- Provide detailed insights and code snippets for each stage of the process

**Creating a Text-to-Speech Application using a Text File**

**Step 1: Creating an Azure AI Services Account**
1. Begin by navigating to the Azure portal and signing in with your credentials.
2. Once logged in, locate the Azure AI services section and proceed to create a new account.
![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEivwhLAYBsKF4-PZE0us7M8k_FjLvdfSqjRt77Nu1LTcZPqNas45rqNJeeC61pAaeqUYI66E3hjxUrwfOXavzL89sfhnu6PqqOPbAaU5GZgSP_dIFM99ogfSh5_VppJE2d9e0HKepMU-4j_fjcslFWS5sRSiwkMRjfCRXTkJgMzDJPOlzYnBXbAJ7_LU9pd/s16000/1_Create_AI_Service.png)
3. In the Create Azure AI window, under the Basics tab, enter the following details and click on the Review+create button.
![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgvRaB4sL8ybpw-zn5xl-xHNfqUR050AFmeA4n1-X4OkUttCXbi0a-rl4vDVD-bZFnEwwTHrKxG7rNAkvCym4BzgBM2f5KSfXpArbZsmm8iZXKtFMZI7Ol6dL1qA1SeUAnd-P-WWbDk6EhBJcpTW0mL_N84itzUQEcdn0EV6FW1noyCKdAcphPFq1J3p0U8/s16000/2_Create_AI_Service.png)
4. In the Review+submit tab, once the Validation is Passed, click on the Create button.
![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEj6naELEmIF4MGnCIvO5e7XAyFe89ngARG_F_hs2W-LdicTW_cwAkimUKlVqezbDEL4V1ruPr1MKqxiHrSrLtp9r88dPF9g6K8uwsvoJcKVN0aJ3TXZvQfbuYYexVKv8zqsdGw3jloOEg124m1AfzrkPrUO-Wuvop01MD_i1CJqNnxh0aqZrMR0h7dkGSwA/s16000/3_Create_AI_Service.png)
5. Wait for the deployment to complete. The deployment will take around 2-3 minutes.
6. After the deployment is completed, click on the Go to resource button.
![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjgpYYceNITLa8dodlTX5IjoS_-zCS-973TLyVzcR31XaMV-_DMs4Y96vh7sdKRyZ7ihwwQsGunj4tlyOEBjkt-AbeNTEa3uLm-OZUUyIXNqKIseGH3cSlWcK3dkyn3F65LDcbq4uI4CiCwNKB-kL-p6-fjpNo8UTgrvdqEl7Pnf2v3gfNC_Xd9mxU-esID/s16000/4_Create_AI_Service.png)
7. In your AzureAI-text-speechXX window, navigate to the Resource Management section, and click on Keys and Endpoints.
![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEisllN83LC4SDPCqzHja9w9CC8IrMhTG9TUnJCfUXf32_4_kReX_SfSmcSTddBIVnXPFVc5XXH7h4e3qCuS5K30_jnzH6m0Kjh7E4yw-qRd6_kO2z-wBUCAV_SWqE_9gfY9UAML9Ucs4nTTRY0Kra3alescfefxY6bCnELDLWtBOo1aaZiyRQxu4FQ5CnZi/s16000/5_AI_Service_Keys_Endpoints_1.png)
8. Configure the account settings according to your requirements.In Keys and Endpoints page, copy KEY1, Region, and Endpoint values and paste them in a notepad as shown in the below image, then Save the notepad later use.
![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjSAMWhXbO7h6R5k3mxNOEBvQ8JEdjUcY2IBaSsamdLHw-fPBpJ70SUEso6lHHOtxnNfmUbz9Iiehl7OmiuInkU-4s5EG-LHEAl-GNsVridKACyu63pSwm6ODqMvB__txu7gEkhOb4uowKTU8lRTE8H5ff5g8ZWor3ImguTFPE1ifXnOpDbltFM8YvWtKs5/s16000/6_AI_Service_Copy_Keys_Endpoints.png)
   

**Step 2: Create your text to speech application**

1.  In the Azure portal, click on the **\[>\_\] (Cloud Shell)** button at the top of the page to the right of the search box. A Cloud Shell pane will open at the bottom of the portal. The first time you open the Cloud Shell, you may be prompted to choose the type of shell you want to use (**Bash** or **PowerShell**). Select **Bash**. If you don't see this option, then skip this step.
    
    ![6wavjic5.jpg](https://labondemand.blob.core.windows.net/content/lab146372/instructions237197/6wavjic5.jpg)
    
2.  In **You have no storage mounted** dialog box, click on the **Create storage.**
    
    ![i8pikt8d.jpg](https://labondemand.blob.core.windows.net/content/lab146372/instructions237197/i8pikt8d.jpg)
    
3.  Ensure the type of shell indicated on the top left of the Cloud Shell pane is switched to _**Bash**_. If it's _**PowerShell**_, switch to _**Bash**_ by using the drop-down menu.
    
    ![qbb1qkgf.jpg](https://labondemand.blob.core.windows.net/content/lab146372/instructions237197/qbb1qkgf.jpg)
    
4.  In the Cloud Shell on the right, create a directory for your application, then switch folders to your new folder. Enter the following command
    
        
    ```css
    mkdir text-to-speech
    cd text-to-speech
    ```
    
    ![s1xdm90b.jpg](https://labondemand.blob.core.windows.net/content/lab146372/instructions237197/s1xdm90b.jpg)
    
5.  Enter the following command to create a new .NET Core application. This command should take a few seconds to complete.
    
        
    `dotnet new console`
    
    ![lfvq8jtr.jpg](https://labondemand.blob.core.windows.net/content/lab146372/instructions237197/lfvq8jtr.jpg)
    
6.  When your .NET Core application has been created, add the Speech SDK package to your application. This command should take a few seconds to complete.
    
        
    `dotnet add package Microsoft.CognitiveServices.Speech`
    
    ![0wp2tdec.jpg](https://labondemand.blob.core.windows.net/content/lab146372/instructions237197/0wp2tdec.jpg)
    

**Step 3:Add the code for your text to speech application**

1.  In the Cloud Shell on the right, open the _Program.cs_ file using the following command.
    
       
    ```dotnetcli
        code Program.cs
    ```
    
2.  Replace the existing code with the following using statements, which enable the Azure AI Speech APIs for your application:
    
        
    ```css
    using System.Text;
    using Microsoft.CognitiveServices.Speech;
    using Microsoft.CognitiveServices.Speech.Audio;
    ```
    
    ![uo6fzs9i.jpg](https://labondemand.blob.core.windows.net/content/lab146372/instructions237197/uo6fzs9i.jpg)
    
3.  Below the using statements, add the following code, which uses Azure AI Speech APIs to convert the contents of the text file that you'll create to create a WAV file with the synthesized voice. Replace the **azureKey** and **azureLocation** values with the ones you copied in the last task 1.
    
   
    
    ```css
    string azureKey = "ENTER YOUR KEY FROM THE FIRST EXERCISE";
    string azureLocation = "ENTER YOUR LOCATION FROM THE FIRST EXERCISE";
    string textFile = "Shakespeare.txt";
    string waveFile = "Shakespeare.wav";
    
    try
    {
        FileInfo fileInfo = new FileInfo(textFile);
        if (fileInfo.Exists)
        {
            string textContent = File.ReadAllText(fileInfo.FullName);
            var speechConfig = SpeechConfig.FromSubscription(azureKey, azureLocation);
            using var speechSynthesizer = new SpeechSynthesizer(speechConfig, null);
            var speechResult = await speechSynthesizer.SpeakTextAsync(textContent);
            using var audioDataStream = AudioDataStream.FromResult(speechResult);
            await audioDataStream.SaveToWaveFileAsync(waveFile);       
        }
    }
    catch (Exception ex)
    {
        Console.WriteLine(ex.Message);
    
    }
    ```
    
    ![nq1qs7oa.jpg](https://labondemand.blob.core.windows.net/content/lab146372/instructions237197/nq1qs7oa.jpg)
    
4.  This code uses your key and location to initialize a connection to Azure AI services, then reads the contents of the text file you\\'ll create, then uses the SpeakTextAsync() method of the speech synthesizer to convert the text to audio, then uses an audio stream to save the results to an audio file.
5.  To save your changes, press **Ctrl+S** to save the file, and then press **Ctrl+Q** to exit the editor

**Step 4: Create a text file for your application to read**

1.  In the Cloud Shell on the right, create a new text file that your application will read:
    
     `code Shakespeare.txt`
    
2.  When the code editor appears, enter the following text.
    
        
    ```css
    The following quotes are from act 2, scene 7, of William Shakespeare's play "As You Like It."
    
    Thou seest we are not all alone unhappy:
    This wide and universal theatre
    Presents more woeful pageants than the scene
    Wherein we play in.
    
    All the world's a stage,
    And all the men and women merely players:
    They have their exits and their entrances;
    And one man in his time plays many parts,
    His acts being seven ages.
    ```
    
    ![dbjulb6c.jpg](https://labondemand.blob.core.windows.net/content/lab146372/instructions237197/dbjulb6c.jpg)
    
3.  To save your changes, press **Ctrl+S** to save the file, and then press **Ctrl+Q** to exit the editor

## **Step 5:Run your application**

1.  To run your application, use the following command in the Cloud Shell on the right:
    
        
    `dotnet run`
    
2.  If you don't see any errors, your application has run successfully. To verify, run the following command to get a list of files in the directory.
    
    
    
    `ls -l`
    
3.  You should get a response like the following example, and you should have the _Shakespeare.wav_ file in the list of files
    
    ![ru8br6zg.jpg](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjpxfotRNWaCy_2d37XwT-lpQSYKgetxoI6y3lFlPKldQIzLsbAtqF0SZji328B-bbHJTBCu8a9UaRGyWZDPXQqjrN8kTMA47nx79mLeLxXpsV6PdgWFX7scsfdxCuKTp6yOERSQL0qVxtgp4TnqrDbMxQUKMh3lx1sWplFI44M5pO7CkBsLs1Yx8aSVydp/s16000/7_AI_Service_Run_The_Application_Result.png)
    

## **Step 6: Listen to WAV file**

In order to listen to the WAV file that your application created, you'll first need to download it. To do so, use the following steps.

1.  In the Cloud Shell on the right, use the following command to copy the WAV file to your temporary cloud drive:
   
   
    
    `cp Shakespeare.wav ~/clouddrive`
    
    ![dputtw5l.jpg](https://labondemand.blob.core.windows.net/content/lab146372/instructions237197/dputtw5l.jpg)
    
2.  In the Azure portal search box, type **Storage account**, then click on **Storage account** under **Services**.
    
    ![shyvtngw.jpg](https://labondemand.blob.core.windows.net/content/lab146372/instructions237197/shyvtngw.jpg)
    
3.  In the **Storage accounts** page, navigate and click on **cloud storage account** .
    
    ![eq320wdc.jpg](https://labondemand.blob.core.windows.net/content/lab146372/instructions237197/eq320wdc.jpg)
    
4.  In the **Storage account** page left-sided navigation menu, navigate to the **Data storage** section, then click on the **File shares**.
    
    ![p1b7cwn3.jpg](https://labondemand.blob.core.windows.net/content/lab146372/instructions237197/p1b7cwn3.jpg)
    
5.  Then select your **cloudshellfilesXXX** file share.
    
    ![xzj0kvaf.jpg](https://labondemand.blob.core.windows.net/content/lab146372/instructions237197/xzj0kvaf.jpg)
    
6.  When your **cloudshellfilesXXX** file shares page is displayed, select **Browse**, then select the **Shakespeare.wav** file, then select the **Download** icon.
    
    ![0yl4iwyw.jpg](https://labondemand.blob.core.windows.net/content/lab146372/instructions237197/0yl4iwyw.jpg)
    
    ![7godv6yk.jpg](https://labondemand.blob.core.windows.net/content/lab146372/instructions237197/7godv6yk.jpg)
    
7.  Download the **Shakespeare.wav** file to your computer, where you can listen to it with your operating system's audio player.
    
    ![b1e0awv0.jpg](https://labondemand.blob.core.windows.net/content/lab146372/instructions237197/b1e0awv0.jpg)
    
    ![w2pcp960.jpg](https://labondemand.blob.core.windows.net/content/lab146372/instructions237197/w2pcp960.jpg)
    

**Conclusion:**   
By following the comprehensive instructions and utilizing the provided code snippets, you can leverage Azure AI services to seamlessly integrate text-to-speech capabilities into your applications. Whether converting plain text or utilizing SSML for customized audio output, Azure AI empowers developers to enhance user experiences and streamline workflow processes. Embrace the power of Azure AI and unlock new possibilities for your projects.