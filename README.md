JavaScript-cookbook
===================

A cookbook for developing a large scale single page JavaScript application

# Table of Content
1. Education
  1. Self-training
  2. Internal JavaScript as a programming language course
  3. Diving into the frameworks
  4. Common Infra training
2. Choosing FWs and architecture
3. Combining Few FWs together under one roof
  1. Single Page Application
  2. MV*
  3.Modularity and message passing
    1.Dependency and package management
  4.UI, templates, styling and widgets
    1.i18n, l10n, ARIA and maybe RTL
  5.Extensibility approach
  6.How to start

#Education

## Self-training
Each developer will have to learn the basics of both JS and HTML on its own.
Such training should take around 3-5 days of work according to the expertise of the developer.
We have the following online training that can help us to achieve this goal:
* Basic training (video based) for beginners: http://channel9.msdn.com/Series/Javascript-Fundamentals-Development-for-Absolute-Beginners
* Interactive online training for JS: http://www.codecademy.com/tracks/javascript-combined
* Interactive online training for HTML & CSS http://www.codecademy.com/tracks/htmlcss, http://htmldog.com/ and http://www.html5rocks.com/en/
* *“JavaScript. The Core.”* http://dmitrysoshnikov.com/ecmascript/javascript-the-core/ very deep tutorial on JS core concepts: Object, Prototype Chain, Scope, Functions, Closures, Execution Context Stack, etc. Read it to get fluent with JS as programming language.

## Internal JavaScript as a programming language course 
The course is focused about becoming a real JS developer
It will discuss about how to use the language its strengths and weaknesses.
This course is based on 3 meetings, 4 hours each.
In addition, developers will be given a one full day about relevant JavaScript frameworks (Backbone.js, Backbone.Marionette, Require.js, Kendo, Handlebars)

# Application Architecture and Frameworks

## Single Page Application
Traditional web pages were composed of mostly static HTML content. JavaScript allowed to add some dynamic behavior to those pages. As the web evolved, AJAX enabled updating just parts of the web page content, without reloading the entire page. This allowed better performance and a new level of interactivity, as web pages began to resemble desktop applications. 
Single Page Application (SPA) paradigm represents the next level of this evolution. in SPA, when the browser first requests the page of an application, it receives some HTML file that instructs to download the JavaScript files of the application. Those files define the behavior of the entire application. This resembles desktop applications, where when the user starts the application, the entire code is usually loaded into memory. From this point on, the only calls to the server are in order to fetch data, but not behavior code. This paradigm allows very high responsiveness of the application, as the code runs locally. It also frees the developers from  modeling constraints. 

## MV* 
The traditional client MVC design pattern implement the separation of concerns principle by dividing the code into three distinct elements:
* Model - represents the domain specific knowledge and data in the application. Models notify whoever chooses to observe about changes to their state. 
* View - the user interface of the application. The view knows the existence of the model to observe it, but doesn’t directly interact with it. 
* Controller - handles the input in the application and updates the model with changes.
MV* is a family of variations over MVC and is being implemented on many modern JavaScript frameworks. MV* framework have model and view, and usually some additional components such as router. For example, in Backbone.js the controller is integrated with the view.
Knockout.js implements  MVVM (Model View ViewModel).
Using a modern and mature framework prevents the application’s code from becoming very quickly into a “spaghetti code” which characterized previous JavaScript implementation. The frameworks add a layer of abstraction between the code and the DOM, reducing or eliminating DOM constraints on the structure of the application’s code. 

Building a modular and modern single page application in JavaScript is not different than building any other application. In fact, the same design patterns of traditional desktop application are implemented in JavaScript. The structure of a single page application is nearly identical to desktop applications. Using mature frameworks such as Backbone.js and Knockout.js facilitates clean separation of code to well defined domains.
In general, the model 

## JavaScript Frameworks
In recent years many JavaScript framworks appeared to help developers better structure JavaScript applications. Choosing frameworks is therefore not an easy task. 
For HP MaaS platform the following frameworks were selected:
Backbone.js - MV* frameworks
Backbone.Marionette - Complements many features which lack in backbone.js
Handlebars.js - Template engine






EOF

