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

#Introduction
This books will try to describe how to develop a large scale JavaScript Single Page Application.
As diagram 1 suggests SPA became a huge trend in 2012 even though the term was coined by Steven Yen back in 2005. Developing SPAs is radically different than developing traditional web applications because of the nature of the client side env’, the dynamic and flexible DNA of the JavaScript programming language and the novelty of tools and frameworks that enhance/modify the development process. Hence experienced R&D managers mostly have little experience with this type of development, in addition they have valid concerns regarding managing large scale JavaScript project. 
In this book we will try to come up with solutions and recipes regarding how to manage and develop an SPA or even a generic large scale JavaScript project effectively.
![Diagram 1: Single Page Application over Google Trends (December 2012)](https://raw.github.com/eladmoshe/JavaScript-cookbook/master/images/SPA-GoogleTrendDec2012.png)

This book starts with a ‘recipe’ for an education plan for non-javascript developers.
The second chapter describes what are the key roles that has to appointed and how to structure the teams.
The third chapter discusses what architectural obstacles such a project introduces and how existing JavaScript FWs can solve those obstacles, one by one.
The fourth chapter suggests that in order to avoid most development obstacles that such a dynamic and flexible programming language introduces(such as bugs, regression etc..), the group should utilizes the TDD paradigm, this chapter introduces the TDD methodology, testing tools and some best practices. 
Later on, the fifth chapter covers how to develop in an efficient fashion and how to generate maintainable code by utilizing generic JavaScript best practices. This chapter also discusses how one should organize the file system, which IDE is recommended and additional best practices.
The last chapter recommends how to structure the build pipeline which consists of running the tests-set, concatenate and minify JavaScript and CSS assets and how to produce a deployable file.

Let’s start with defining what large scale javascript project really is:

###Large Scale JavaScript Project
Large-scale JavaScript applications are applications that mostly align to 3 properties [source](http://addyosmani.com/largescalejavascript/):
1. Most heavy lifting of data manipulation and display falls to the browser.
2. Non-trivial in terms of functional/system requirements. 
3. Requires significant number of developers(dozens) 

It seems like developing SPA is a complicated task, lets discuss why should one develop an SPA from an historical perspective.

###Historical Perspective
In the last few years it became clear that in order to produce rich internet application(desktop-like in terms of performance and usability) the browser must take a bolder role in terms of execution.

Desktop applications don’t let their backend to generate the pages that they are displaying, they also run most of the logic on the client side, they let the server side to be a shared-data broker. there are two reasons for that:
1. the network is the main bottleneck in terms of door to door timing 
  1. sending the entire pages over the network is heavy
  2. backends that contains business logic forces the application to send requests to the server via the network
2. Replacing one generated page to another is unnatural to the human eye
  1. it affects usability
  2. it takes more time to generate the entire page again rather than generating the specific region
As a side effect the code is executed is getting distributed to the different powerful clients which tremendously decrease the need to abuse your backend servers.

In order to achieve RIA abilities one should place most of the business logic and most of the page generation processes to the client side.
The rule of thumb is that only two types of business logic should reside and get executed by the backend server: [1] manipulation of shared data (e.g. batch analysis). [2] secretive logic, either in terms of intellectual property or in terms of security.
Regarding page generation: in the past the common approach was to generate the pages on the server side, mostly by utilizing server-side templates engines such as [PHP](http://en.wikipedia.org/wiki/PHP), [JSP](http://en.wikipedia.org/wiki/JavaServer_Pages) and [ASP](http://en.wikipedia.org/wiki/Active_Server_Pages). 

Some organizations such as Microsoft, Sun and Adobe took the desktop approach back in the early 2000 where they were utilizing browser plug-ins in order to generate pages and execute large amount of code on the client side, this has started with [ActiveX](http://en.wikipedia.org/wiki/ActiveX) continued with [Java Applets](http://en.wikipedia.org/wiki/Java_applet) solutions and proceeded with [Silverlight](http://www.microsoft.com/silverlight/) and [Flex](http://en.wikipedia.org/wiki/Apache_Flex ).
Yet the plug-in approach didn’t take off, there were a lot of bugs in the integration layer between the browser and the plug-in, memory leaks were a common phenomena and  security became even larger concern. recently Adobe transferred Flex to the apache foundation since it could not back it up business-wise, Apple announce that Java Applets are too risky in terms of security and for that reason none of their devices will Java Applets anymore [(source)2](http://9to5mac.com/2012/10/17/apple-removes-java-applet-plugin-from-os-x-continuing-push-for-plugin-free-web/
). ActiveXs are working only on windows machines which make them less useful.

On the other hand, in the last few years the modern browser can execute large amount of complicated JavaScript code and generate pages by it self. this is all possible due to a radical transformation of the browser Javascript virtual machine, the rendering engine and the browser engine it self. 
Chrome was the first browser that came up with a strong VM that could execute JavaScript almost as fast as the JVM executes Java bytecode. since then all the other browsers got aligned due to the browser wars and nowadays they all advise reasonable performance. 

The single page application is the most popular paradigm which supports large amount of client side processing , elimination of backend page generation and therefore an upgraded user experience.

###Single Page Application:
Traditional web pages were composed of mostly static HTML content. JavaScript allowed to add some dynamic behavior to those pages. As the web evolved, [AJAX](http://en.wikipedia.org/wiki/Ajax_(programming)) enabled updating just parts of the web page content, without reloading the entire page. As we explained above, this allowed better performance and a new level of interactivity, as web pages began to resemble desktop applications. 
Single Page Application (SPA) paradigm represents the next level of this evolution. in SPA, when the browser first requests the page of an application, it receives some static HTML file that instructs to download the static JavaScript files of the application. Those files define the behavior of the entire application. This resembles desktop applications, where the user starts the application, the entire code is usually loaded into memory. From this point on, the only calls to the server are in order to fetch data, but not behavior code. This paradigm allows very high responsiveness of the application, as the code runs locally. It also frees the developers from  modeling constraints. 
As we discussed the major performance bottleneck in traditional web applications is the network, SPA solves it by reducing the size of the HTTP responses (it contains only data instead of the entire page). Moreover since the page is no longer being generated on the server-side, the page is static and can be cached internally or even on a [CDN](http://en.wikipedia.org/wiki/Content_delivery_network). 
Since most of the logic runs on the client side, the backend is transformed into a data provider which usually get treated as an API server, this API server can start serving also different client such as mobile devices or even 3rd party applications that are allowed to use your application’s API.
Another advantage is the ability to go offline, since most of the logic is ‘written’ on static resources, it’s easy for the browser to cache those and run it offline (the developer will have to implement some HTML5 requirements in order to make the application truly offline).

There are also some disadvantages:
* **Routing**: there is only one URL for the entire application, which causes the inability to browse to a specific ‘virtual page’ and improper behavior of the back button 
  * Chapter 5 shows how this problem can be solved by using an anchor based solution
* **Complex client side development**: mostly since this is a new ground yet more specifically it’s unclear how to split logics into modules, how to organize the file system, which tools support the familiar process that most groups conduct for server-side development etc..  
  * This entire book aims to provide recipes and best practices that will make the development process much easier
* **Javascript Frameworks are still evolving**: there are huge amount of JavaScript frameworks, some of those are immature, some of those have small communities and unreadable code
  * Chapter 5 will cover what types of FWs one need and which exact FWs are recommended in the light of stability and large community. 
* **The JavaScript programming language**: the dynamic and flexible nature of javascript causes insecurity in terms of the quality of code
Chapter 4 introduces the TDD paradigm which upon implementation increases the quality of code dramatically, Chapter 6 discusses best practices for coding which upon following should decreases the number of defects. 
The dynamic and flexible nature of Javascript can also be treated as an advantage, it decreases the size of code, it enables rapid development and the existence of many of the FWs that are introduced in chapter 5.     
* **SEO difficulties**: since it’s harder for search engines to understand/parse SPAs - search engines usually read only the initial static file which doesn’t include any real data.




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
In the following section we describe the frameworks that were selected for HP MaaS.

#### Backbone.js
Backbone is an MV* framework that provides both structure and infrastructure implementation. Backbone’s main classes are Model, Collection, View and Router.
###### Model
_“Backbone Models are the heart of any JavaScript application, containing the interactive data as well as a large part of the logic surrounding it: conversions, validations, computed properties, and access control“_ (By backbone.js authors definition). Model object could  be saved to or retrieved from the server using standard REST API. 
Model definition example: 
```javascript
// We extend the Backbone.Model prototype to build our own
var Project= Backbone.Model.extend({
   // default values of attributes
  defaults : {
    name : null,
    id: null,
    PO : null
  },
   url : function() {
    //  relative URL where the model's resource would be located on the server
    // In this case, POST to '/donuts' and PUT to '/donuts/:id'
    return this.id ? '/projects/' + this.id : '/projects'; 
  }
});
```
###### Collection
A collection is an ordered set for models. The collection could be notified on adding new / deletion /modification of model item.

###### View
A Backbone View is designed to encapsulate a DOM element’s functionality .All the logic of the client side application should be within the appropriate View. A view combines both the Controller and the View of MVC, therefore handling and responding both user input and the application output. It is in charge to update the model due to ui changes and vise versa to update the DOM when the model changes.
The view has a property called template, which defines how this View is rendered in the DOM. Backbone is flexible and let the developer to choose his preferred method of HTML templating. Our templating engine is Jade (to be described later).
```javascript
ProjectView = Backbone.View.extend({
         tagName : "div",   //instance of CarView it will be created inside div element
         template: '#tpl-project-settings', //name of the template file to be used for DOM         
 
         //core function of Backbone.View - is where you tell your view how to render itself.
         render : function() { 
            //renders the view template from model data, and updates this.el with the new HTML
            $(this.el).innerHTML = this.model.get('name'); 
            return this;
        }
   });
 
   var BDD = new Project ({
         name : "BDD Project"
   });
 
   var BDDView = new ProjectView({
        model : BDD //specify view model 
   });
 
   BDDView.render();
```

###### Router
A Backbone Router is the facility that allows to implement a Single Page Application (SPA).
In SPA, as far as the browser concerns, the navigation to the application’s page occurs only once, when the user navigates to the application for the first time. At this stage, the entire application is loaded to the memory of the browser. Any links (hrefs) in the application begins with a “#” sign. The browser parses everything that comes after the “#” sign as internal page navigation and therefore doesn’t send any request to server. However, the Backbone Router intercepts this action, and invoke the method that is registered to that url. During page load, after the application has finished creating all of its routers, call to Backbone.history.start() should be done to to  subscribed to the browser’s hashchange event*. Whenever the hash changes, Backbone.history takes care of running your Router’s function.
Backbone.history is where Backbone stores all of the places you’ve been. This is necessary in order to not break the back button in your browser.
```javascript
var AppRouter = Backbone.Router.extend({
        routes: {
            "help":                 "help",    // #help
            "search/:projectName":  "search"  // #search/MyProj           
         },
 
         help: function() {
             ...
         },
 
         search: function(projectName) {
             ...
         }
    });
    // Initiate the router
    var app_router = new AppRouter;
 
    // Start Backbone history a necessary step for bookmarkable URL's
    Backbone.history.start();
```

#### Backbone.Marionette
To simplify the construction of large scale JS application we choose to use Marionette.js, which is composite application library for Backbone.js.
Marionette is a collection of common design and implementation patterns. It provides a lot of boilerplate code (i.e. identical code that needs to be written for many application) for Backbone, built-in memory management and views garbage collector, extends Backbone.View with additional View implementations(LayoutView, ItemView) which helps to create neat implementations and much more. Have a look at the sample application and Marionette website for details. 

#### Handlebars - TODO

#### JSLint (JSHint)
JSLint is a code quality and validation tool.JSLint takes a JavaScript source and scans it. If it finds a problem, it returns a message describing the problem and an approximate location within the source. The problem is not necessarily a syntax error, although it often is. JSLint looks at some style conventions as well as structural problems (It does not prove that your program is correct functionally).
JSHint is a fork of JSLint which is community driven, rather than controlled by a single, opinionated person (Douglas Crockford). It allows more flexability in rules customizations compared to JSLint.



## UI, templates, styling and widgets
In todays web application the client side get more and more responsibility and a lot of logic is being moved to the client side, the delay of a round trip between the client and the server, making the Web application more responsive.
As web applications move farther into the client, Javascript frameworks have sprung up that operate on a higher level of abstraction than DOM manipulation frameworks like jQuery.

Web apps must offer simple, intuitive and responsive user interfaces that let their users get things done with less effort and time.
There are a wide varied range of Web UI front-end frameworks for design web apps. There are few main aspects to consider choosing UI front-end framework: Look & Feel that it provides , simplicity of usage, community and documentation, supports all major browsers...
In Storm we choose to use Twitter Bootstrap toolkit, that currently is one of the leading ui toolkits in the web.

Another choice that the web application developer should make is what template engine he would like to use.The template engine produces a pure HTML web page by processing the web template source file along with data from a database.Including template engine in the web application developing process supports dynamic content, allows separation of concern - separate presentation from content, allows reuse of ui sections...
For template engine we choose NodeJS template engine “Jade”. 
Other template engines that are popular those days:handlebars.js, mustache.js

## i18n, l10n, ARIA and maybe RTL
JavaScript i18n (and the rest) support can be implemented on three levels. The server may do the replacement of strings sending the client a compiled and fully functional HTML. This approach is similar to PHP. Another approach is to replace the strings during the template rendering (as most templating engine must take some data source as an argument). This allows the client to control which strings will be replaced and when but requires the loading of the resources files into the client memory. The third approach is to replace the strings on the final DOM. Once the HTML is rendered the strings are replaced on the client using the loaded resources. The three methods are not mutually exclusive and therefore it is possible to use all three in a single application. In Storm we use a mix of the second and third approaches via the i18next library. In most cases the strings are replaced after the element DOM is created but just before it is rendered to the main DOM.

Widgets and CSS must be RTL aware (e.g. location of scroll bar, layout direction, etc)

# References for Further Education

## Books and articles
* "JavaScript: The Good Parts" by Duglas Crockford - must read
* "Maintainable JavaScript" by Nicholas C. Zakas - good practical tips on JS development
* "Learning JavaScript Design Patterns" by Addy Osmani - very good one
* "Secrets of the JavaScript Ninja" by John Resig and Bear Bibeault - from author of jQuery library
* “Backbone Fundamentals” by Addy Osmani - publically available ebook visit github to follow and get your copy.

## Recommended Blogs 
* "ECMA-262" by Dmitry Soshnikov - lots of low level JS staff
* Articles for Developers by Addy Osmani







EOF

