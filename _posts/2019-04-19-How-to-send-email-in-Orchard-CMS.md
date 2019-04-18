---
title: "How to send email in Orchard CMS"
published: true
description: "How to send email in Orchard CMS"
tags: orchardcms
cover_image: 
canonical_url: http://niranjankala.in/post/How-to-send-email-in-Orchard-CMS
layout: post
---

# Introduction

In this article, you will learn how to send email in Orchard CMS using it's out of box services

# Steps to send email in Orchard CMS

* Create Email wrapper template 
* Create Email Template
* Inject required orchard services in the Controller or Custom Service 
* Specify email template to use and pass data
* Sending email using MessageService

To demonstrate the scenario, we take the example of sending a challenge email to verify the user email address. In Orchard. Users module, you will find all this code to send an email using orchard services.

![Enable docker support in visual studio](https://3.bp.blogspot.com/-w-ldBHiXoWs/XLkCg6gOu8I/AAAAAAAABtY/5n7iNSwrGmIuxVu0WYZB41tyOKx2QHVAwCLcBGAs/s640/OrchardCMS_Email_Template.png)

# Steps 1 - Create Email wrapper template

It applies default Body alteration for SmtpChannel which create the body of HTML template. create a new file "Template.User.Wrapper.cshtml" under Views folder.
```
@* Override this template to alter the email messages sent by the Orchard.Users module *@
@Display.PlaceChildContent(Source: Model)
```

# Steps 2 - Create Email Template

Now create the content of the email. It applies default Body alteration for SmtpChannel which create the body of HTML template. create a new file "Template.User.Validated.cshtml" under Views folder. It is the same razor view page where we can pass the data and place on the place holders. e.g. Model.ContactEmail is the address of the contact person of the website.

```
@T("Thank you for registering with {0}.<br /><br /><br /><b>Final Step</b><br />To verify that you own this e-mail address, please click the following link:<br /><a href=\"{1}\">{1}</a><br /><br /><b>Troubleshooting:</b><br />If clicking on the link above does not work, try the following:<br /><br />Select and copy the entire link.<br />Open a browser window and paste the link in the address bar.<br />Click <b>Go</b> or, on your keyboard, press <b>Enter</b> or <b>Return</b>.", Model.RegisteredWebsite, Model.ChallengeUrl)
@if (!String.IsNullOrWhiteSpace(Model.ContactEmail)) {
    @T("<br /><br />If you continue to have access problems or want to report other issues, please <a href=\"mailto:{0}\">Contact Us</a>.",Model.ContactEmail)
}
```

# Steps 3 - Inject required orchard services in the Controller or Custom Service 

To render the email template, we need to use a few orchard services. In Orchard CMS every view object is a shape so we need to create a shape using our created template views. We need to inject  IShapeService and IShapeFactory to create the shape for the email content. IMessageService required to send the email through the SMTP channel.

```
namespace Orchard.Users.Services {
    public class UserService : IUserService {
        private readonly IMessageService _messageService;
        private readonly IShapeFactory _shapeFactory;
        private readonly IShapeDisplay _shapeDisplay;
        public UserService(
        IMessageService messageService, 
        IShapeFactory shapeFactory,
        IShapeDisplay shapeDisplay) {

            _messageService = messageService;
            _shapeFactory = shapeFactory;
            _shapeDisplay = shapeDisplay;
        }
        ...
```

# Steps 4 - Specify email template to use and pass data

At this step, we specify the email and wrapper template to create the Shape for HTML content. In below line of code ShapeFactory.Create method takes the first parameter for the email template then we add email wrapper template in the template metadata. Along with this, we pass the model data using the anonymous object.

```
var template = _shapeFactory.Create("Template_User_Validated", Arguments.From(new {
    RegisteredWebsite = site.As<RegistrationSettingsPart>().ValidateEmailRegisteredWebsite,
    ContactEmail = site.As<RegistrationSettingsPart>().ValidateEmailContactEMail,
    ChallengeUrl = url
}));
template.Metadata.Wrappers.Add("Template_User_Wrapper");
```

# Steps 5 - Sending email using MessageService

Now specify the mail message Subject, Body and Recipients of the email message. You can also specify attachments also. Just add the "Attachments" key and List<string> as value to the parameters dictionary. Attachment should be the file path to attach with the email message.
```
var parameters = new Dictionary<string, object> {
                    {"Subject", T("Verification E-Mail").Text},
                    {"Body", _shapeDisplay.Display(template)},
                    {"Recipients", user.Email}
                };

        _messageService.Send("Email", parameters);
```
 Below is the complete code for method which sends the challenge email:
 ```
public void SendChallengeEmail(IUser user, Func<string, string> createUrl) {
    string nonce = CreateNonce(user, DelayToValidate);
    string url = createUrl(nonce);

    if (user != null) {
        var site = _siteService.GetSiteSettings();

        var template = _shapeFactory.Create("Template_User_Validated", Arguments.From(new {
            RegisteredWebsite = site.As<RegistrationSettingsPart>().ValidateEmailRegisteredWebsite,
            ContactEmail = site.As<RegistrationSettingsPart>().ValidateEmailContactEMail,
            ChallengeUrl = url
        }));
        template.Metadata.Wrappers.Add("Template_User_Wrapper");
        
        var parameters = new Dictionary<string, object> {
                    {"Subject", T("Verification E-Mail").Text},
                    {"Body", _shapeDisplay.Display(template)},
                    {"Recipients", user.Email}
                };

        _messageService.Send("Email", parameters);
    }
}
```

# Conclusion
I guess that's all to send an email in Orchard CMS.
