---
title: "Dive Deep: Unveiling DevExpress Splash Screen in Your Winforms App"
published: true
description: "Dive Deep: Unveiling DevExpress Splash Screen in Your Winforms App"
tags: devexpress, dotnet, winforms
cover_image: 
canonical_url: https://niranjankala.in/post/dive-deep-unveiling-devexpress-splash-screen-in-your-winforms-app
layout: post
---

**Introduction:**

In today's fast-paced digital world, user experience plays a pivotal role in the success of any application. One aspect that significantly contributes to a positive user experience is the loading screen or splash screen. A well-designed splash screen enhances the aesthetic appeal of your application and provides users with visual feedback during the loading process, reducing perceived wait times.

In this tutorial, we'll explore implementing a splash screen in a Winforms application using DevExpress, a powerful suite of UI controls and components. By the end of this tutorial, you'll have a sleek and professional-looking splash screen integrated into your Winforms application, enhancing its overall user experience.

**Step 1: Setting Up Your Winforms Project**

Before we implement the DevExpress splash screen, let's set up a basic Winforms project in Visual Studio.

1. Open Visual Studio and create a new Winforms project.
2. Name your project and choose a location to save it.
3. Once the project is created, you'll see the default form in the designer view.

**Step 2: Installing DevExpress**

You need to install the DevExpress NuGet package to use the DevExpress controls and components in your Winforms project.

1. Right-click on your project in the Solution Explorer.
2. Select "Manage NuGet Packages" from the context menu.
3. In the NuGet Package Manager, search for "DevExpress" and install the appropriate package for your project.

**Step 3: Adding a Splash Screen Form**

Now, let's create a new form for our splash screen.

1. Right-click on your project in the Solution Explorer.

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhYLg0FZkr9RrpH5bSArUSCXoln1AhbzmOuXe0XAzHj-XJeLW58ObNY803JoPn0kGn5vQnuF6OofCv31la7S9jxxtkLI_2VJLk69X2LKaclVSe5fcymyP14kTXfKHYMH1b_KiKjJtZsMohbctiYT_HhBAfzWDj3vlMJVgtHQk9YHhwWPrk8Lcp76QAd0Ftr/s16000/1_DevEx_SplashScreen.png)
2. Select "Add DevExpress Item" from the context menu.

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhcXng4uc0NpOyhVodGUBThDR6wHffQ9FPpfcKI0OuALEA_lNZtjVFZpNTRju2bxuorcYPC_SsYsOkx-IP1PhT81JjjKgb2kavB0Byk3T84-opzjhVJx_WSnQRMN6OVj5K4PsPFzqRauJDoTSjZBrAlKdl4rpLCzFGTQ9bg9GeyWiUS_NxLHl605Ysa2Nnb/s16000/2_DevEx_Add_SplashScreen.png)
3. Select "Splash Screen" from the DevExpres Template Gallery.
4. Name the form "SplashScreenForm" and click "Add Item"
5. Design your splash screen form using DevExpress controls to customize its appearance according to your preferences. You can add images, animations, and progress indicators to make it visually appealing.

**Step 4: Configuring Application Startup**

Next, we must configure our application to display the splash screen during startup.

1. Open the Program.cs file in your project.
2. Locate the `Application.Run` method, typically found within the `Main` method.
3. Before calling `Application.Run`, create and display an instance of your splash screen form.
   
   ```csharp
    static void Main()
    {
        Application.EnableVisualStyles();
        Application.SetCompatibleTextRenderingDefault(false);
    
        //Application.Run(new MainFormWithSplashScreenManager());
        var form = new MainForm();
        DevExpress.XtraSplashScreen.SplashScreenManager.ShowForm(form, typeof(SkinnedSplashScreen));
        //...
        //Authentication and other activities here
        Bootstrap.Initialize();                        
    
        DevExpress.XtraSplashScreen.SplashScreenManager.CloseForm();                      
        Application.Run(form);
    }

    internal class Bootstrap
    {
        internal static void Initialize()
        {
            // Add initialization logic here
            //Authentication and other activities here
            LoadResources();            
            
        }
        private static void LoadResources()
        {
            // Perform resource loading tasks
            // Example: Load configuration settings, connect to a database, etc.

            Thread.Sleep(1000);//For testing
        }
    }
   ```

**Step 5: Adding Splash Screen Logic**

Now that our splash screen is displayed during application startup let's add some logic to control its behaviour.

1. Open the SplashScreenForm.cs file.
2. Add any initialization logic or tasks that must be performed while the splash screen is displayed. For example, you can load resources, perform database connections, or initialize application settings.

   ```csharp
    public partial class SkinnedSplashScreen : SplashScreen
    {
        public SkinnedSplashScreen()
        {
            InitializeComponent();
            this.labelCopyright.Text = "Copyright © 1998-" + DateTime.Now.Year.ToString();
        }

        #region Overrides

        public override void ProcessCommand(Enum cmd, object arg)
        {
            base.ProcessCommand(cmd, arg);
        }

        #endregion

        public enum SplashScreenCommand
        {
        }

        private void SkinnedSplashScreen_Load(object sender, EventArgs e)
        {
            
        }
    }
   ```

**Step 6: Testing Your Application**

With the splash screen implemented, it's time to test your Winforms application.

1. Build your project to ensure there are no compilation errors.
2. Run the application and observe the splash screen displayed during startup.
3. Verify that the application functions correctly after the splash screen closes.

See the following topic for information on how to execute code when your application starts: [How to: Perform Actions On Application Startup.](https://docs.devexpress.com/WindowsForms/119891/build-an-application/how-to-perform-actions-on-application-startup)

**Conclusion:**

In this tutorial, we've learned how to implement a splash screen in a Winforms application using DevExpress. By following these steps, you can enhance the user experience of your application by providing visual feedback during the loading process. You can customize the splash screen further to match the branding and style of your application and experiment with different animations and effects to create a memorable first impression for your users.

**References**   
[Splash Screen](https://docs.devexpress.com/WindowsForms/10823/controls-and-libraries/forms-and-user-controls/splash-screen-manager/splash-screen)    
[Splash Screen Manager](https://docs.devexpress.com/WindowsForms/10826/controls-and-libraries/forms-and-user-controls/splash-screen-manager)


[**Source Code**](https://github.com/niranjankala/DevExpress/tree/dev/src/WinForms/CS/SplashScreenTutorials)
