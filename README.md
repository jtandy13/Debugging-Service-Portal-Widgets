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
# Advanced Techniques
# Running the Examples