# Debugging Service Portal Widgets - Basic and Advanced Techniques
Developing on ServiceNow's Service Portal can be tricky sometimes, especially if you are new to web development or new to AngularJS. This article attempts to make your job just a little bit easier by setting out a standard set of Service Portal Widget debugging techniques and is intended both for informational and training purposes. The techniques range in complexity from simply logging to the console to pulling the strings of your widget from the JavaScript console like a puppet master! In most situations, an understanding of the [Basic Techniques](#basic-techniques) section will suffice. For more complicated scenarios, a firm grasp of the [Advanced Techniques](#advanced-techniques) may be required. The techniques in this article are a compilation of techniques gathered from currently available ServiceNow resources and new techniques which are documented here for the first time.

If you are completely new to Service Portal and are starting from scratch, you may want to have a look at these resources below before you start running the examples from this article.

#### Pre-requisites:
[Service Portal Introduction](https://developer.servicenow.com/app.do#!/training/article/app_store_learnv2_serviceportal_jakarta_service_portal_introduction/app_store_learnv2_serviceportal_jakarta_service_portal_introduction_objectives?v=jakarta)

[AngularJS Tutorial w3schools.com](https://www.w3schools.com/angular/default.asp)

[Creating Custom Widgets](https://developer.servicenow.com/app.do#!/training/article/app_store_learnv2_serviceportal_jakarta_creating_custom_widgets/app_store_learnv2_serviceportal_jakarta_creating_custom_widget_objectives?v=jakarta)

[Service Portal fundamentals: AngularJS scopes](https://www.dylanlindgren.com/2017/10/28/service-portal-fundamentals-angularjs-scopes/)

All examples in this article are run on a widget called "Global Objects Demo Widget". To run the examples in the article you will need to see section [Running the Examples](#running-the-examples) at the end of the article to get setup.

# Basic Techniques
## Logging to the JavaScript console
The **console.log()** function will log data to the JavaScript console in the browser. This technique will work in the Client Script of the widget and also in the Server Script. The fact that it works in the Service Script as well is a great advantage! I can't think of anywhere else in the ServiceNow platform where you can log to the JavaScript console from server-side JavaScript.
### Example
If you look at line 3 and line 16 of the Server Script of the Global Objects Demo Widget, you'll notice that the "input" and "data" objects are logged to the console.

![consoleLogging](https://user-images.githubusercontent.com/22809154/37747539-d1f9e5f4-2dd3-11e8-832c-1118b4358818.png)

Open the JavaScript console in Chrome developer tools. Refresh the preview pane and you'll notice the following result in the console:

The input object is undefined and the data object is printed to the console.

![loggingresult](https://user-images.githubusercontent.com/22809154/37747847-3b9a377e-2dd5-11e8-9ae8-680b5302da49.png)
## Using the inbuilt *debugger* function in Chrome and Firefox
The debugger function can be used in the Client Script of the widget but not the Server Script. Adding the debugger function is like inserting a break point into your code allowing you to step through the code line by line.
### Example
In the Client Script of the widget, add the code **debugger;** at line 23.

![debuggerexample](https://user-images.githubusercontent.com/22809154/37747957-c75804e4-2dd5-11e8-8d63-050712ef7124.png)

Open Chrome Developer Tools. Save the widget and refresh the preview pane. Press the **server.get({collectionName: "presidents"})** button. The JavaScript execution will stop at the word debugger. To see the "response" object contents, hover your mouse over the word "response" in line 14 of the code in devtools.

![mousehover](https://user-images.githubusercontent.com/22809154/37748017-1c97f270-2dd6-11e8-9548-da1b60ceb703.png)

To step into the next line in the code use the down arrow button. To allow the JavaScript execution to proceed without stopping, press the "Resume script execution" button.

![resumecodeexecution](https://user-images.githubusercontent.com/22809154/37748188-0924ca96-2dd7-11e8-9768-fe96bafa80d6.png)
## Log the widget's "scope" object to the console
Hold down the control key and right-click on the widget.  Choose "Log to console: $scope.data" or "Log to console: $scope". The only difference is whether you want to log the entire scope object to the console or only the data property of the scope object.

### Example
Navigate the to the following URL: https://<yourinstancename>.service-now.com/sp?id=demo_widget_example

Hold down the control key and right-click on the widget. Choose "Log to console $scope.data".  Open Chrome Developer Tools and expand the object dumped to the console and verify that the value of $scope.data.prop1 is "Apple".

![prop1](https://user-images.githubusercontent.com/22809154/37748416-d55959e2-2dd7-11e8-8622-d9319868157a.png)
# Advanced Techniques
These advanced debugging techniques are not only cool but can be highly affective when troubleshooting on a production instance where it's not possible to make any changes. All the techniques below are run through Chrome Developer Tools.
## Creating a reference to the widget's scope in the console ("The puppet master")
You can think of this technique as an analogy to a puppet show. The puppeteer activates and manipulates the puppets (widgets) with a set of strings. In this case we're creating a second set of strings to the puppet so that we can activate and manipulate the widget from the JavaScript console!

This technique allows you to do the following from the console:
* Change the widget's scope data
* Run widget scope functions
* Re-run the widget Server Script

### Steps to get a reference to the widget's scope in the JavaScript console:
1. Right-click on the widget and choose "Inspect"
2. In devtools Elements tab, click on the element with attribute widget=”widget”. It should be a few elements above the currently inspected element. This points the $0 scripting tool at the widget.
3. In the Javascript console, run the following code: 
```javascript 
var scopeRef = angular.element($0).scope();
```
### Changing the widget's scope data:
Once you have a reference to the widget in the JavaScript console, you can take any piece of data in the widget's scope and just change it. After changing a value, run the AngularJS $apply() function on the scope to apply your changes to the page.
#### Example
After getting a reference to the Global Objects Demo Widget in the console, run the following code:
```javascript 
scopeRef.data.prop1 = "Peach";
scopeRef.$apply();
```
![changeScopeData](https://user-images.githubusercontent.com/22809154/37750454-3a943102-2de1-11e8-8e63-1e8310eb7af4.png)
### Running the widget's Client Controller functions from the console
Any function that is defined in the Client Scipt (client controller) of the widget is available from the widget's scope. This means that once you have a reference to a widget's scope in the console you will not only be able to change data in the scope object but you can also run any of the client controller functions!
#### Example
The getPrettyData() function is defined in the Client Script of the Global Objects Demo Widget. Now that we have a reference to the widget's scope in the console, we can run the function directly from the JavaScript console.

![consolefunction](https://user-images.githubusercontent.com/22809154/37750884-51d2820e-2de3-11e8-99e5-db36e32adeed.png)
### Re-run the widget's Server Script
Let's say that we've made some changes to the scope of the widget with the techniques above, we've got our reference to the widget in the console, and we want to see what happens when the server refreshes the data sent to the client controller. We can re-run the widget's Server Script from the console as per below:
```javascript 
scopeRef.server.refresh();
```
## Debugging and editing the widget client script directly from the Chrome devtools "Sources" panel
The client controller scripts for all widgets on the page can be found in the "Sources" panel of Chrome Developer Tools. If you look at the screen capture below, you'll notice that they are all listed under the "top" window then under the "(no domain)" section. Clicking on the script will open it in the Sources panel code editor window. Most of the widget client controller scripts are listed as <widget_id>.js, others will be listed by the id attribute value of the top level HTML element of the widget.

![sourcespanel 1](https://user-images.githubusercontent.com/22809154/37751095-7d3f259a-2de4-11e8-9068-6071e8b2ec03.png)

Once the client controller script is open in devtools you can begin debugging directly from there.
### Making local changes to the client controller code from devtools
#### Example
Navigate the to the following URL: https://<yourinstancename>.service-now.com/sp?id=demo_widget_example.

Open Chrome developer tools.

Click on the "Sources" panel in devtools.

Open the global-objects-demo-widget.js file

![sourcespanelcode](https://user-images.githubusercontent.com/22809154/37751162-05d489f4-2de5-11e8-833b-9ab6a7096523.png)

Between lines 7 and 8 add the following line of code:
```javascript 
alert("Server script now refreshed");
```
Right-click in the script editor window and Save. Do not refresh the page.

![alert](https://user-images.githubusercontent.com/22809154/37751279-98007b62-2de5-11e8-8116-b5afbf58de92.png)

Click the **server.refresh()** button in the widget.

Notice the alert window pops up showing that you've been able to alter the widget directly from devtools!
### Adding break points to widget client controllers from within devtools
Another great thing about having access to the client controller code from within devtools is that you can add in break points. Break points can be added to the code by clicking on the line numbers in the code editor window of the "Sources" panel.
#### Example
Navigate the to the following URL: https://<yourinstancename>.service-now.com/sp?id=demo_widget_example

Open Chrome developer tools.

Click on the "Sources" panel in devtools.

Open the global-objects-demo-widget.js

Click on line number 15 to add in a break point.

![breakpoint](https://user-images.githubusercontent.com/22809154/37751391-4f9937b4-2de6-11e8-8606-f03096fcd8a6.png)

Click on the **server.get({collectionName: "presidents"})** button.

Notice that JavaScript execution stops directly at line 15.

Hover the mouse over the "data" in line 15 to inspect the response.

![breakpointhover](https://user-images.githubusercontent.com/22809154/37751442-af4978a4-2de6-11e8-9d4d-aedd7b56ed5f.png)

Resume JavaScript execution with the button to the right of the code editor window.
# Running the Examples