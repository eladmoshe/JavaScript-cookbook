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









EOF

