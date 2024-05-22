---
title: "Real-Time Speech Translation with Azure: A Quick Guide"
published: true
description: "Real-Time Speech Translation with Azure: A Quick Guide"
tags: azure
cover_image: 
canonical_url: https://niranjankala.in/post/real-time-speech-translation-with-azure-a-quick-guide
layout: post
---

### Introduction

The Azure Speech Translation service enables real-time, multi-language speech-to-speech and speech-to-text translation of audio streams. In this article, you will learn how to run an application to translate speech from one language to text in another language using Azure's powerful tools.

### Objective

By the end of this article, you will be able to create and deploy an application that translates speech from one language to text in another language.

### Step 1: Creating a New Azure Cognitive Services Resource Using Azure Portal

#### Task 1: Create Azure Cognitive Speech Service Resource

1. Open a tab in your browser and go to the [Speech Services](https://portal.azure.com/#create/Microsoft.CognitiveServicesSpeechServices) page. If prompted, sign in with your Azure credentials.

2. On the Create page, provide the following information and click on **Review + create**:

   - **Subscription**: Select your subscription (this will be selected by default).
   - **Resource group**: Create a new group named `azcogntv-rg1`. Click on **OK**.
   - **Region**: East US
   - **Name**: CognitiveSpeechServicesResource
   - **Pricing tier**: Free F0

   ![Create Speech Service](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEi5_JHoxkIAk8-VUhwIrfG1VZ8l87sZSrPLlHBZnTtmu2ojFg3NIQnMPx0QhSopvN4eZrrJmGE-5LbaGYZY2Oc1QT5kxwQdsWkQP9hpFNvCTAcygdbzz3Un9pLzSps_r7RVvck9_vKLeFIlD_ccsfUtGiK9OT4rLmAnkkq7RWkuj1oZCw0L7GtpJMgE2jKA/s16000/create_speech_service.jpg)

   ![Create Speech Service](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEivTNaWzK1zaAMMXctEkzIjNSXsJirmT3UihHXoCiDVTrXvmFhBfBPm5puPCQLt5s8A_TwmkCmhyOFPaP0Dj52l3MyqqfoAxblFkr2lJugXFcqtO7lESSXurvgLKzCBxfC3Ni00uczOovw5kGl74736VQhgw255mVCcO9M8CCv2IVwGC9GmuSaNChoeVR1M/s16000/create_speech_service_1.jpg)


3. Once the validation passes, click on **Create**.

   ![Validation Passed](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjV6tuQEzFzxX_xDMjsT-9a6r58cuK8RvlCel0Z1NH-ATeur7JEEhG6_leVEN54-vUGmK6RlkrQ0WYIDnJmUxskYLW0KOaANXwj36n0VSRnDSnGktdLJ8gJxog2VA72KY_rOzFRYskROF_dxxfPtJMwZeQC2TZwXIp7DPUIfOCTxyTFu1CBCRyaa3_F-n0W/s16000/create_speech_service_3.jpg)

4. Wait for the deployment to complete, then click on **Go to resource**.

   ![Deployment Complete](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhaXkyDGumPU4m3mBwjRWiUD0B28DpX0H8rrI6rBjLyCRCsu7KC5ncplW8S6xtxv4WksN6jWzoL46V1yqeVDgGnNGUGx43w6Mn5zDMQoflJCSSwgxYVd3VOormrUTSYxT5NZv-PoOoJBK7pdVXwl00GizEZHza7W5C_wjv2VofzGj_68Z9BeCYdwiN6aloe/s16000/1_Speech_Service_Go%20to%20resource.png)

5. Click on **Keys and Endpoint** from the left navigation menu. Copy and save **Key 1** and **Endpoint** values in a notepad for later use.

   ![Keys and Endpoint](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgvMT9iD0dhpqaXHcGb_pjG4XwoqW8cmXi2S1ujuNjRnpS_f8i2Yykm8IIDau2jNazpvHRGbGJ-i3OE7FPj2GqUtHgKiQ_TrHdad8Zruv9afab2brTF6UR7KTYLdKKQjwp-CPLptZ42BCHxJl59slRy7wHYa8wvGrkpZXrFrAnHAexrYXG4Or4l4NVNGqZA/s16000/2_Speech_Service_Copy_Keys%20and%20Endpoint.png)

#### Task 2: Create Azure Cognitive Language Service Resource

1. Open a new browser tab and go to the [Language Services](https://portal.azure.com/#create/Microsoft.CognitiveServicesTextAnalytics) page. Sign in with your Azure credentials.

2. Without selecting any option on the page, click on **Continue to create your resource**.

   ![Continue to Create Resource](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhHrp2dtXvZx8Fg7tZpEB4Ys0TJzQBw_akzpxm4fkIcyDCAD_UVnNmxyj__sMYKOKPzwayIp1aFm3TZjBI-q43wwqyPleXxn2goLrMhlHI6Dqlc-UkiPzRb8k1MLzg41beV3KVVevZZ1gJfIlpGBkbCkdO4gxDFI7332FMwD83vwFHSmI9H8j7ubotO-S9Q/s16000/TextAnalytics_.png)

3. Update with the following details and then click on **Review + Create**:

   - **Subscription**: Your Azure subscription
   - **Resource Group**: Select `azcogntv-rg1`
   - **Region**: East US
   - **Name**: CognitivelanguageResourceXX (Replace XX with any random number)
   - **Pricing tier**: Free (F0)
   - **Select checkbox**: By checking this box, I certify that I have reviewed and acknowledged the Responsible AI Notice terms.

   ![Create Language Service](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjho7U5n9pIY5EMo3qhlrdcS9VI5eOSCpiYEjWvlmyrNOnwpqVhXABP8kllu3wZueZULmLSnMnywhb-OdiGzX1NJxYkuMEKRIoUp5bDp5g3u4RYRojauKt_tTiLK2BjZ-VMmcNyLSSje3HockldSBdrgFKUIQpemNUt6XLQXnDWz6HVAbyWJWyVRf3E0wMs/s16000/TextAnalytics_create_language_service.jpg)
   ![Create Language Service](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjTiCDBeUgrHwcl7gqMTD5luAJgcp_SrlrdUy3DxcSc5CzBy4stobhSKTN46-i4Y4XXZcSDr4kABA7lVUt5X0cYSjeJ6AKS8bwrlHJXeS0fB4mF7QO08u1Nq67PNCM4sIVBG2wxr4SlfvkIIfIe0Oi8xXy4yc7JMIAYZINhgsHorbmxgm6hqetPHqFOy6eW/s16000/TextAnalytics_create_language_service_1.jpg)

4. Review the resource details and then click on **Create**.

   ![Review and Create](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhkBVMUXRFIYKcLHXicZyvL1r2GXhv-CYDjDbrnOVunLJbqytpE9XQTGYR5kpvulDEEtcJw9d4wnrkVmCSyTBdfPonLBZ6cD8Fnjf0anOu2O4y5Pylh8qFX64MSVfrailsVyqIBSjFQAFZXlaFI4S8ylJLg3HCkU-yK-abFTiXipNHGr6RczkFWC83Nauw_/s16000/TextAnalytics_review_create.jpg)

5. Wait for the deployment to complete, and once successful, click on **Go to resource group**.

   ![Deployment Successful](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhvAumKchfOd7CS5ipXRjptsw2KtXbmySSmcfcsrZAB4bpEsr8BV6kuI7_qq1IsPhW_CUCYp_iwh6ORE4cS9FoNtVm4vZfH2zi383zi5Y_ylGC9Jr7tN2G8vMMGrlNG96G59uZ0bRfmQkZl-hgskzFQpPSXZd4iLT-GOTjdK_wrcLCOufFKNq_OmB9yIIrr/s16000/3_TextAnalytics_Go%20to%20resource.png)

6. Click on **CognitiveLanguageResource**.

   ![Cognitive Language Resource](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhfpa0RNQDGWWD-zGF82wwIZ2UVlHPNNKl6QF4lYY2gj8qc2SrheMFYTF2nJFp7OndqsStlEfDFrcHAeT5yFKW_xQtClrWaaCtTD-w1ElJInyYMYOvvG5zHbpbQPck2W_vf5I0dKWzNvXgqER80upCaaMI1g2aoY9e82a9pkhkR0zB8v6a0Ec6DWNfZxTgD/s16000/5_TextAnalytics_Click%20on%20the%20resource.png)

7. Click on **Keys and Endpoints** > **Show keys**. Copy **Key 1** and endpoint values and save them in a notepad for later use.

   ![Keys and Endpoints](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgMYeA1BZ5Sj31HDEII06_VqVTMYSrQGnwVrpseRmAEmSyGeAQzbZQmQbPFflefftwC83T0HuQHpPc9HVBFnuNEHZ9qPfVQjpYrBl5KNFDOAGk92KED3JdburlgJIDloq2ngKQZvG0ktKZzP9FUOVwYHVvu8BddOSxfayNGNMFyI4TaqbQw3DrPIQ0L_ZtH/s16000/4_Text_Analytics_Copy_Keys%20and%20Endpoint.png)

### Step 2: Recognizing and Translating Speech to Text

#### Task 1: Set Environment Variables

Your application must be authenticated to access Cognitive Services resources. Use environment variables to store your credentials securely.

1. Open Command Prompt and run `mkdir Speech-to-Text` to create a directory. Then run `cd Speech-to-Text` to navigate into it.

   ```bash
   mkdir Speech-to-Text
   cd Speech-to-Text
   ```
 

2. To set the `SPEECH_KEY` environment variable, replace `your-key` with one of the keys for your resource saved earlier.

   ```bash
   setx SPEECH_KEY your-key
   setx SPEECH_REGION eastus
   ```

   ![Set Environment Variables](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhLlYipB9IDoXfdrISqiL40WFLY7zasKwGsq0hQ-JeZCO59b1wuGf1-4sveFE1HpGAGrd2TSUl_lc0SwwNUAenj56rcrSqTC1dYkAbpiYibv1jxr6AkDhFPyKKrRcKyV69bwkUiNPHXo4rYH92QsPECQ4D21csWdI41rW-aWsPsA2J_2WG9UmN79TNnE_8U/s16000/6_Set_Environment%20variable.png)

3. After adding the environment variables, restart any running programs that need to read the environment variable, including the console window. Close the Command Prompt and open it again.

#### Task 2: Translate Speech from a Microphone

1. Open Command Prompt, navigate to your directory (`cd Speech-to-Text`), and create a console application with the .NET CLI.

   ```bash
   dotnet new console
   ```

   ![Create Console App](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgkqM0gzECEAYu3yJLuuzv_Y0LSaVj6pNrjnm-43Heypl36rwGNzBAaEpQV_j2B7kRjN4FdGiGZ2QlrsJFmDZ6meHxezOv8pjQW_yzEsJKh8C-WDihcLucZedpvFXZLyh2dRIiA2zIHVxVLaEA_ThSgOs5de_QFy8DTgMNJQ8vmLLPpR50oJ_SQk3pVG1Tl/s16000/7_Create_Console_Application.png)

2. Install the Speech SDK in your new project with the .NET CLI.

   ```bash
   dotnet add package Microsoft.CognitiveServices.Speech
   ```

   ![Install Speech SDK](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiy8XKxc6UoNc1hvc9IFcvTENvNv3A4gAuOQ30WR5HDgxQNqiVgZVBMyEdrjAgJ3Q-bJUUKcW2ybwp6DYQ32cxwPScA6OOq6Ad7pV4WQPXVN1izWTO0uohP88fcjLo-YZbA1tcRcgaYdte8jNLnhBoVfppRqCK3PKFyYJXA1qCk27brvvKDrm5H7PFRdYsI/s16000/8_Add_Reference_CognitiveServices_Speech.png)

3. Open the `Program.cs` file in Notepad from the `Speech-to-Text` project folder. Replace the contents of `Program.cs` with the following code:

   ```csharp
   using System;
   using System.IO;
   using System.Threading.Tasks;
   using Microsoft.CognitiveServices.Speech;
   using Microsoft.CognitiveServices.Speech.Audio;
   using Microsoft.CognitiveServices.Speech.Translation;

   class Program
   {
       // This example requires environment variables named "SPEECH_KEY" and "SPEECH_REGION"
       static string speechKey = Environment.GetEnvironmentVariable("SPEECH_KEY");
       static string speechRegion = Environment.GetEnvironmentVariable("SPEECH_REGION");

       static void OutputSpeechRecognitionResult(TranslationRecognitionResult translationRecognitionResult)
       {
           switch (translationRecognitionResult.Reason)
           {
               case ResultReason.TranslatedSpeech:
                   Console.WriteLine($"RECOGNIZED: Text={translationRecognitionResult.Text}");
                   foreach (var element in translationRecognitionResult.Translations)
                   {
                       Console.WriteLine($"TRANSLATED into '{element.Key}': {element.Value}");
                   }
                   break;
               case ResultReason.NoMatch:
                   Console.WriteLine($"NOMATCH: Speech could not be recognized.");
                   break;
               case ResultReason.Canceled:
                   var cancellation = CancellationDetails.FromResult(translationRecognitionResult);
                   Console.WriteLine($"CANCELED: Reason={cancellation.Reason}");

                   if (cancellation.Reason == CancellationReason.Error)
                   {
                       Console.WriteLine($"CANCELED: ErrorCode={cancellation.ErrorCode}");
                       Console.WriteLine($"CANCELED: ErrorDetails={cancellation.ErrorDetails}");
                       Console.WriteLine($"CANCELED: Did you set the speech resource key and region values?");
                   }
                   break;
           }
       }

       async static Task Main(string[] args)
       {
           var speechTranslationConfig = SpeechTranslationConfig.FromSubscription(speechKey, speechRegion);
           speechTranslationConfig.SpeechRecognitionLanguage = "en-US";
           speechTranslationConfig.AddTargetLanguage("it");

           using var audioConfig = AudioConfig.FromDefaultMicrophoneInput();
           using var translationRecognizer = new TranslationRecognizer(speechTranslationConfig, audioConfig);

           Console.WriteLine("Speak into your microphone.");
           var translationRecognitionResult = await translationRecognizer.RecognizeOnceAsync();
           OutputSpeechRecognitionResult(translationRecognitionResult);
       }
   }
   ```

4. Run your new console application to start speech recognition from a microphone:

   ```bash
   dotnet run
   ```


5. Speak into your microphone when prompted. What you speak should be output as translated text in the target language:

   ```text
   Speak this: The Speech service provides speech-to-text and text-to-speech capabilities with an Azure Speech resource and then press Enter.
   ```

   ![Speak Into Microphone](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgv3Wcw5zDcJTURuPiS43JFvjypXkwYEPYBxWcpvj4m0jIlPC-ushVO6yipD8UdgeN1AI6FGH8nG_aWEMaWkC3-UFryF4Q_s5PriNkPK2zRPRWBqSKB6qknwQEcHhQ9fDmRR70O3q1hye49bwQ43ZJ3Fzc7Whj7cNLEx273ULY5L7wsZ06evPfE88hw8a9g/s16000/9_CognitiveServices_Speech_Output.png)

### Conclusion

In this article, you translated speech from a microphone to a different language by updating the code in the `Program.cs` file. This powerful feature of Azure Cognitive Services allows for seamless and real-time translation, making it a valuable tool for various applications and industries.

[**Source Code**](https://github.com/niranjankala/Cloud-Development/tree/master/src/Azure/cognitive-services/Speech-to-Text)
