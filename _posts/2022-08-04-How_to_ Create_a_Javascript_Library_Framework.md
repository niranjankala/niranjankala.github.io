---
title: "How to Create a Javascript Library/Framework"
published: true
description: "How to Create a Javascript Library/Framework"
tags: javascript
cover_image: 
canonical_url: http://niranjankala.in/post/how-to-resolve-global-exception-logger-with-dependency-injection-in-asp-net-web-api
layout: post
---
    
### Introduction

In this article, you will learn how to Create a Javascript Library or Framework. 


### Creating a JavaScript library

 Library Name: Greeter

- When given a first name, last name, and options language, it generates formal and informal greetings.
- Supports English and Spanish languages. 
- Reusable library/framework.
- Easy to type 'G$()' structure.
-Support jQuery

#####  Structure Safe code

***HTML***
```html
<html>
  <head>      
  </head>
  <body>
      <script src="scripts/jquery-3.6.0.js"></script>
      <script src="scripts/greetr.js"></script>
      <script src="scripts/app.js"></script>
    </body>
</html>
```
Include jQuery first to enable the jQuery support.

***greetr.js***

We require a global variable window and jQuery. Set up an IIFE function by passing the windows and the jQuery function.

Now create an IIFE function to start with.
```javascript

(function(global, $) {
}(window, jQuery));
```

Now, this is safe to use in any of the applications and ready to use.

The next step is to set up the greeter object and the alias similar to jQuery $. You can review code the code of the jQuery library understands the safe entry method to work with any library.
```javascript

(function (global, $) {

  var Greetr = function (firstname, lastName, language) {
    return new Greetr.init(firstName, lastName, language);
  };

  // You can create your properties and function here
  Greetr.prototype = {};

  Greetr.init = function (firstName, lastName, language) {
    var self = this;
    self.firstName = firstName || "";
    self.lastName = lastName || "";
    self.language = language || "en";
  };

  Greetr.init.prototype = Greetr.prototype;

  //Set the alias
  global.Greetr = global.G$ = Greetr;
}(window, jQuery));
```

##### Adding Language support

Now we set up the lanugage support for the English and Spanish. Along with this 

```JavaScript
(function (global, $) {
  var Greetr = function (firstname, lastName, language) {
    return new Greetr.init(firstName, lastName, language);
  };

  var supportedLanguages = ["en", "es"];

  var greetings = {
    en: "Hello",
    es: "Hola",
  };

  var formalGreetings = {
    en: "Greetings",
    es: "Saludos",
  };

  var logMessages = {
    en: "Logged In",
    es: "iniciar la sesión",
  };

  // You can create your properties and function here
  Greetr.prototype = {
    fullName: function () {
      return this.firstName + " " + this.lastName;
    },
    validate: function () {
      if (supportedLanguages.indexOf(this.language) === -1) {
        throw "Invalid language";
      }
    },
    greeting: function () {
      return greetings[this.language] + " " + this.firstName + "!";
    },

    formalGreetings: function () {
      return formalGreetings[this.language] + " " + this.fullName() + "!";
    },
    greet: function (formal) {
      var msg;
      //if undefined or null, it will be coerced to 'false.'
      if (formal) {
        msg = this.formalGreetings();
      } else {
        msg = this.greeting();
      }

      if (console) {
        console.log(msg);
      }

      //'this' refers to the calling object at the execution time
      // makes the method chainable
      return this;
    },
    log: function () {
      if (console) {
        console.log(logMessages[this.language] + ": " + this.fullName());
      }
      return this;
    },
    setLanguage: function (lang) {
      this.language = lang;
      this.validate();
      return this;
    },
  };

  Greetr.init = function (firstName, lastName, language) {
    var self = this;
    self.firstName = firstName || "";
    self.lastName = lastName || "";
    self.language = language || "en";
  };

  Greetr.init.prototype = Greetr.prototype;

  //Set the alias
  global.Greetr = global.G$ = Greetr;
}(window, jQuery));
```
***Calling the library in the application***

```javascript
var g = G$("Niranjan", "Singh");
//Chained behavior and call to display greetings
g.greet().greet(true);
//Change the language and then greet
g.greet().setLang('es').greet(true);
```
##### Adding jQuery support

We need to add jQuery support and provide the functionality to give the id to Greetr library for updating the element text. 

Update the HTML page with the below text to enable/demonstrate the jQuery incorporation.
```html

<html>

<head>
</head>

<body>
  <div id="logindiv">
    <select id="lang" div>
      <option value="en">English</option>
      <option value="es">Spanish</option>
    </select>
    <input type="button" name="login" id="login" value="Login">
  </div>
  <h1 id="greeting"></h1>
  <script src="scripts/jquery-3.6.0.js"></script>
  <script src="scripts/greetr.js"></script>
  <script src="scripts/app.js"></script>
</body>

</html>
```
It requires changes in the Greetr library also. So add a new method called HTMLGreeting with a selector parameter.
```javascript

    HTMLGreeting: function (selector, formal) {
      if (!$) {
        throw "jQuery not loaded";
      }
      if (!selector) {
        throw "Missing jQuery selector ";
      }

      var msg;
      //if undefined or null, it will be coerced to 'false.'
      if (formal) {
        msg = this.formalGreetings();
      } else {
        msg = this.greeting();
      }

      $(selector).html(msg);

      return this;
    },
```

Below is the simple library/framework which we have developed. It could be referred to and used to create a library.

```javaScript
(function (global, $) {
  // 'new' an object
  var Greetr = function (firstName, lastName, language) {
    return new Greetr.init(firstName, lastName, language);
  };
  // hidden within the scope of the IIFE and never directly accessible
  var supportedLanguages = ["en", "es"];
  // informal greetings
  var greetings = {
    en: "Hello",
    es: "Hola",
  };
  // formal greetings
  var formalGreetings = {
    en: "Greetings",
    es: "Saludos",
  };
  // logger messages
  var logMessages = {
    en: "Logged In",
    es: "iniciar la sesión",
  };

  // You can create your properties and function here
  Greetr.prototype = {
    fullName: function () {
      return this.firstName + " " + this.lastName;
    },
    validate: function () {
      if (supportedLanguages.indexOf(this.language) === -1) {
        throw "Invalid language";
      }
    },
    greeting: function () {
      return greetings[this.language] + " " + this.firstName + "!";
    },

    formalGreetings: function () {
      return formalGreetings[this.language] + " " + this.fullName() + "!";
    },
    greet: function (formal) {
      var msg;
      //if undefined or null, it will be coerced to 'false.'
      if (formal) {
        msg = this.formalGreetings();
      } else {
        msg = this.greeting();
      }

      if (console) {
        console.log(msg);
      }

      //'this' refers to the calling object at the execution time
      // makes the method chainable
      return this;
    },
    log: function () {
      if (console) {
        console.log(logMessages[this.language] + ": " + this.fullName());
      }
      return this;
    },
    setLanguage: function (lang) {
      this.language = lang;
      this.validate();
      return this;
    },
    HTMLGreeting: function (selector, formal) {
      if (!$) {
        throw "jQuery not loaded";
      }
      if (!selector) {
        throw "Missing jQuery selector ";
      }

      var msg;
      //if undefined or null it will be coerced to 'false'
      if (formal) {
        msg = this.formalGreetings();
      } else {
        msg = this.greeting();
      }

      $(selector).html(msg);

      return this;
    },
  };
  // the actual object is created here, allowing us to 'new' an object without calling 'new'
  Greetr.init = function (firstName, lastName, language) {
    var self = this;
    self.firstName = firstName || "";
    self.lastName = lastName || "";
    self.language = language || "en";
    self.validate();
  };
  // trick borrowed from jQuery so we don't have to use the 'new' keyword
  Greetr.init.prototype = Greetr.prototype;

  //Set the alias, attach the Greetr to the global object and provide a shorthand '$G' for the ease our poor fingers
  global.Greetr = global.G$ = Greetr;
}(window, jQuery));
```

### Conclusion
We have created a small library that supports the jQuery framework also. We can create an extensive library or framework by following the same pattern. The best way to learn this is by reviewing the existing open source libraries and frameworks, e.g., jQuery.