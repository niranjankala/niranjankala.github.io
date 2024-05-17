---
title: "Quick Start: Create and Deploy C# Functions in Azure Using CLI"
published: true
description: "Quick Start: Create and Deploy C# Functions in Azure Using CLI"
tags: dotnet, azure
cover_image: 
canonical_url: https://niranjankala.in/post/quick-start-create-and-deploy-c-functions-in-azure-using-cli
layout: post
---


#### Introduction

In this blog post, we'll guide you through the process of creating and deploying a C# function to Azure using command-line tools. This article will walk you through creating an HTTP-triggered function that runs on .NET 8 in an isolated worker process. By the end of this post, you will have a functional Azure Function that responds to HTTP requests.

#### Objective

In this article, you will:
1. Install the Azure Functions Core Tools.
2. Create a C# function that responds to HTTP requests.
3. Test the function locally.
4. Deploy the function to Azure.
5. Access the function in Azure.

#### Prerequisites

Ensure you have the following installed:
- Azure CLI (version 2.4 or later)
- .NET SDK (version 6.0 and 8.0)
- Azure Functions Core Tools (version 4.x)

#### Step 0: Install Azure Functions Core Tools

1. **Uninstall previous versions (if any):**
   - Open the **Settings** from the Start menu.
   - Select **Apps**.
   - Click on **Installed apps**.
   - Find **Azure Function Core Tools**, click the three dots next to it, and select **Uninstall**.

2. **Install the latest version:**   
   - Navigate to the Azure Function Core Tools downloads page: [Install the Azure Functions Core Tools](https://learn.microsoft.com/en-us/azure/azure-functions/functions-run-local?tabs=windows%2Cisolated-process%2Cnode-v4%2Cpython-v2%2Chttp-trigger%2Ccontainer-apps&pivots=programming-language-csharp#install-the-azure-functions-core-tools).
   - Download the appropriate version of Azure Functions Core Tools for your operating system. (Recommended. [Visual Studio Code debugging](https://learn.microsoft.com/en-us/azure/azure-functions/functions-develop-vs-code#debugging-functions-locally) requires 64-bit.)
   ![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgEH3sU4j9IlEgpRH2azeJTJih5KYXnkfHuULhw_6sCpzpXbBg1r7L2M_4S_cyD4nrWFpt5JMPHkifS6l4Q7E_o3E_SN39KkAIq2yTgPn4QhXCrS4xVN7x1g7Lrhi_c77n4keK_TdcoF1zR1raQj5YyJxpM5Yxh1KDoSRNbDaqz3cEwD5LjHRvcdsBiqakz/s16000/1_Download_Azure_Function_Core_Tools.png)
   - Follow the prompts: Click **Next**, accept the agreement, and click **Install**.

       ![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEj8Dg0ylkih8DozawwNhS-yGk3MFqkICSRepnCZlXay5KzQh1-PhqLTEm-zgf8JG8ej4TZw26DCludXnXPylzAKCSiFXYqOD68_OuUAhsPr4f1YPp4LK3NT5-uxv84Mp-6OAA8fld4f_1qOzY779r80EL6OhG4c-B6Du6nSeKQ29f1ebVqgN36PS0kY5M1R/s16000/2_Install_Azure_Function_Core_Tools.png)
   - Click **Finish** once the installation completes.

#### Step 1: Prerequisite Check

1. Open Command Prompt and execute the following commands to verify your setup:
   - **`func --version`** – This is to check that the Azure Functions Core Tools are version 4.x.
   - **`dotnet --list-sdks`** – This is to check that the required versions are installed. It should be 6.0 and 8.0
   - **`az --version`** to check that the Azure CLI version is 2.4 or later.
   
   ![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEj62V8itMu-fzpbFBzItinc6wSdKRHvgUxxVoZH30ijYnOo14TD05KMvpY2RZ03-JSiE6OQzTLiT0fYtabo2qSXUHeATcYjHWw4lRRKR-Mlh59AvgzIY4GwcKNtvc7HAjD1aAGUY6ILKxhugfy39v9P10ZtfCI4iFXg_hrQqaga8yOyyze_5vouin2zQSOe/s16000/3_Prerequisite%20check.png)

2. **`az login`** to sign in to Azure and verify an active subscription. Select your login in the browser that opens up. Log in to Azure when prompted:
   - A browser window will open. Select your Azure account to sign in.
   - The command prompt will display your Azure login details.

   ![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjSGaHagqj8WW_0Y5NeG-EaVByhNgch-3BoPYI6vETmyCvaNY8knlrLuAgtP9w9YgiCl8WWoyEj1oXcoP4YY_g9PqR4n2WuIvrkg6gbBQyf21d9S_4PRE2LaGCEoq1q3XLtivvFPIE_S5v21yeQhbGiKWE-nYAHIE58qkRUQLmNoglk0MpSFGAvBnqrBnMu/s16000/4_Prerequisite%20check_Azure_Login.png)

   ![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjCeE8iaRu-cBX4ZLE9rqumNKiair8s0aI13MN5x8Zh8oGyXHsGqp8toybwRVBEq6_LbphRrRNu9cAc1phmz2KnROhh3Pf-yeGTyUNkljdvQU0vLi7oiMNp2TAvVea7SsnirXSQYmfrs-vUPg4dgo2N_-fAEjdf5L-WugiJr4N5mw1UimlgnetRmVYOgT5e/s16000/5_Prerequisite%20check_Azure_Login_Post_Message.png)

#### Step 2: Create a Local Function Project

1. **Initialize the function project:**
   Run the func init command, as follows, to create a functions project in a folder named LocalFunctionProj with the specified runtime:
   ```sh
   func init LocalFunctionProj --worker-runtime dotnet-isolated --target-framework net8.0
   cd LocalFunctionProj
   ```

   This folder contains various files for the project, including configurations files named local.settings.json and host.json. Because local.settings.json can contain secrets downloaded from Azure, the file is excluded from source control by default in the .gitignore file.

   ![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEj6eTGfb_0R98P9uJygypC1NKn2BcFkqzxXNQ4IClbO6vNISAptX9sbSkDLvN615GXYc479s5oMREKJxU1GaEqLDcgPNXgaXsuQMUGib2skLUAXkUEWcCAlvJWhKEC6dfho1fLnVLYC9K7lp6RxD_3gp3COQysK8WcQMH8hb05KU9ckiUIaxZ3JZ58N6icA/s16000/6_Create_Azure_Function_Project.png)

2. **Add a new HTTP-triggered function:**
   ```sh
   func new --name HttpExample --template "HTTP trigger" --authlevel "anonymous"
   ```
   ![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjEdC-ikxG2CVAUpUkrechyphenhyphenUSUSBPvVD6U7UB9p6FkGkc3ZX56EikeXDq07M2Ycd0gCVkKumCUhhHmP5sl1Qdqm0vbKT62T5ajTGJU4dM_FCwBqEBBBn_WHEFkBERCpxB2XshnnSOprAPEFt9zp56BracO6TcncbyEIGZbzcxjjaNxeDmtjFOxB7prs7Rtj/s16000/7_Add_Function_To_Project.png)

3. **Examine the generated code:**
   ```csharp
    using Microsoft.Azure.Functions.Worker;
    using Microsoft.Extensions.Logging;
    using Microsoft.AspNetCore.Http;
    using Microsoft.AspNetCore.Mvc;
    
    namespace LocalFunctionProj
    {
        public class HttpExample
        {
            private readonly ILogger<HttpExample> _logger;
    
            public HttpExample(ILogger<HttpExample> logger)
            {
                _logger = logger;
            }
    
            [Function("HttpExample")]
            public IActionResult Run([HttpTrigger(AuthorizationLevel.Anonymous, "get", "post")] HttpRequest req)
            {
                _logger.LogInformation("C# HTTP trigger function processed a request.");
                return new OkObjectResult("Welcome to Azure Functions!");
            }
        }
    }
   ```
   ![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgjTv_xiHSNHi8bB49fp8XCzmGcVY5bWYsSzCpBdy7cx3vycpW2sHwalLg1lLL_RyeeHJg7vaB2e26-KbrtslWDpcL2oAHo5Qg4l6R81h_nKIgfvni3IUypSqATOTg-3LJSXMztcyMPs3DfQBeZ5tiaCd__CDRB8OLUdoeHLOTl6e5G7wAUjKmyXaZS1Yq9/s16000/7_Review_Code_For_Function.png)

#### Step 3: Run the Function Locally

1. **Start the local Azure Functions runtime host:** Run your function by starting the local Azure Functions runtime host from the LocalFunctionProj folder:
   ```sh
   func start
   ```
    The output says that the worker process started and initialized. Also, the url of the function is displayed.    
2. **Test the function:**
   - Copy the URL from the output and paste it into a browser.
   - You should see a message: "Welcome to Azure Functions!"

   ![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhEfTk5giLeihMbPQVkXGIUMyiVzz_MEHtcrch8SEirLUGs0PQTMYdKDiKMA6nBrc7DW5JpXS9Su1HiPpHmTkI0jFTR3SfVxHDYwVLjgh59_VwVYIqWddfpMxqiZJwe578RFThAT-RIxKV6iX3hK07FzNQEh_fhMC903_tbU1_J42-HR_U66STmx3ZKvpql/s16000/8_Run_Function_In_Local.png)  

3. **Stop the function host:**
   - Press `Ctrl+C` and confirm with `y`.

#### Step 4: Create Supporting Azure Resources

Before you can deploy your function code to Azure, you need to create three resources:

- A resource group, which is a logical container for related resources.

- A Storage account, which is used to maintain state and other information about your functions.

- A function app, which provides the environment for executing your function code. A function app maps to your local function project and lets you group functions as a logical unit for easier management, deployment, and sharing of resources.

Use the following commands to create these items. Both Azure CLI and PowerShell are supported.

1. **Sign in to Azure:** 

    If you haven't done so already, sign in to Azure: The `az login` command signs you into your Azure account.
    ```azurecli
    az login
    ```

   
2. **Create a resource group:**

    Create a resource group named RGForFunctionApp in your chosen region:
   ```azurecli
   az group create --name RGForFunctionApp --location eastus
   ```
   The **az group create** command creates a resource group and populates the command shell with the created RG details with the **Provisioning state - Succeeded**

   ![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiKCI38vSHDhDmiIUbcwrQsg1J9UwS2-WIzVMVG78wi2lUlPkEYvc0QPiRJEVvO50H8PUwL6nKVvH3mjV2J91FTcXJc6qKIIcuYGKLWvg-_2ym1LakkNAKXjD9dye_BfBuBgrW2ZC2TZ03YEbHh60xqODX8tQ5zH50aavCetghL0-RfXTx_lhWmqodzb58M/s16000/9_Create_RGForFunctionApp.png)

3. **Create a storage account:**

    Create a general-purpose storage account in your resource group and region.
   ```azurecli
   az storage account create --name storaccforazfuncXX --location eastus --resource-group RGForFunctionApp --sku Standard_LRS --allow-blob-public-access false
   ```
   The **az storage account create** command creates the storage account named **storaccforazfunc07** in the **EastUS** region. The details are populated in the command prompt with a provisioning state succeeded.

   ![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhBfrH3i-wZOaHMkq318zrl2NHGcznJlxtvTA8v_08dlxSogRZtoy1oh7DK5IT49vPyy8vdxEM5d85tDfVOrYcb04OPkoGiUBHqQTgpCTCPdfqe-BLa1Rqjj4X4NW5CBgMi-ZPwFqRC93BI-Lj3DHX7noYKZYF-rrallbgitrvrFDSsDjXAzTeCInU65szw/s16000/10_Create_Storage_Account_For_Function_1.png)
   ![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEi6lrUML3jHdSlc3g1ywGyorvOlmx6AjgTaq0F0QxS5Kw7h13qEk5RpjFBp1cs3ujCPGtoSHih4NVOX2lj2-QtPAkgsaL4SOxiRZ6F7aDQQN0XygQFwrBMnBhND58p2OZGB4WPMfoBKNPX87OOyQNyvYjGjFTymUAVB4VKr1cEOzfVfEJLS76z_pzV5iGgt/s16000/10_Create_Storage_Account_For_Function_2.png)

4. **Create the function app:**
   ```azurecli
   az functionapp create --resource-group RGForFunctionApp --consumption-plan-location eastus --runtime dotnet-isolated --functions-version 4 --name appforfuncXX --storage-account storaccforazfuncXX
   ```
   ![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEh_mwHNG-z9TLgFHvvQ0btzBA0vZsAIIi-JDcsfejloveFhVX9STGjgmA89Udq6odZ-uTx5qlo9nzpuiFPn7rpZK5aGmaGZoeSgtLZGG0fSQm8LcSBr_Z1t4dEDXBXPbvUek5taxpfQLTsL3Xmwy7QvG9pyIpolSx8iTBOZjRjdXGcu1cf5eKgUxyXwKxMp/s16000/11_Create_Function_In_RGForFunctionApp.png)

   ![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEh4PBHIBD1rNo_cr64IDpHNzIWG8vu_MXfgfD-71Hv35NZWO_72lRPK_vdMGz2J4V3JgqMub7mGVW7Pc7MpXklGlvksKipMpNJPQQ99p0-T02_YPrzco7CeLsPDOw-dVUxUB8p4be9VVE3iMHzeTa9iIuNkb8x7P99e7wlAYmxBzUlcKD_BUL7h3jXVgtGj/s16000/12_View_Resources_In_Azure_Portal.png)

#### Step 5: Deploy the Function Project to Azure
After you've successfully created your function app in Azure, you're now ready to deploy your local functions project by using the **func azure functionapp publish** command.
1. **Deploy the function:**
   ```sh
   func azure functionapp publish appforfunc07
   ```
   After deployment, a URL will be provided. This is the Invoke URL for your function.

   ![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjdNRLSwBC-xyce7X_ylxO3txLQHMpaYl_Oy4Ajsgh-WTIvfZ_ksJUoK2DLGKX3SGcdn_9UWms5lrzHpPOBQJqNh0iCyppCfgij4eztMiitckJT5W2Nt1oddXMfgMWADkj1q4sZQEOdNRJFekaIZpyLTv2e6qBGmQWORzEMW_JtSoKvdkbN8uBMSj_t7FLv/s16000/13_Deploy_Function_To_Azure.png)

   
#### Step 6: Invoke the Function on Azure

1. **Invoke the function using a browser:**
   - Copy the Invoke URL and paste it into a browser.
   - You should see the same "Welcome to Azure Functions!" message.

   ![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiB0gZAWLzJ_vj4RFphgzAFOJRyNnM7sMAZHo23orlBtQDPWGpwn4kPPI5fzdzQFdxsN3utMwSbJ7cfQ1JrP08tt5qfXsUJf6rzHjeL8AIEfBy5ID6G5dInYvTUsbJJbFenLXPJIO2IFQ-hulyqFTr_SWO5Efefro1_SxqosFfyMx4gucwsXJsXErizI-fW/s16000/14_Function_Preview.png)

2. **View real-time logs:**

    In a separate terminal window or in the browser, call the remote function again. A verbose log of the function execution in Azure is shown in the terminal.

   ```sh
   func azure functionapp logstream appforfunc07
   ```

   - Open another terminal or browser window and call the function URL again to see the logs in real-time.
   - Press `Ctrl+C` to end the logstream session.

#### Step 7: Clean Up Resources

1. **Delete the resource group:** 

    Execute the following command to delete the resource group and all its contained resources. ype y when prompted Are you sure you want to perform this operation and hit Enter. 

   ```sh
   az group delete --name RGForFunctionApp
   ```

##### Conclusion

Congratulations! You've successfully created, tested, and deployed a C# function to Azure using command-line tools. This step-by-step guide has walked you through installing necessary tools, setting up a local development environment, creating and running a function locally, deploying it to Azure, and finally cleaning up the resources. Azure Functions provides a powerful, serverless compute environment to build and deploy scalable, event-driven applications with ease. Happy coding!