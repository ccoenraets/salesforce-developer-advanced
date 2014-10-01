---
layout: module
title: Module 10&#58; Using the Salesforce1 Platform APIs
---
In this module, you create an application that runs outside your Salesforce instance: it uses OAuth to authenticate with Salesforce, and the REST APIs to access Salesforce data.

![](images/api.jpg)

## Requirement

You need Node.js to perform the exercises in this module. If you don't already have Node.js installed on your system, you can install it [here](http://nodejs.org/).

> This module is recommended but optional. If you are not interested in building custom applications (applications hosted outside your Salesforce instance), you can move to Module 11.

## Step 1: Create a Connected App

1. In Setup, click **Build** > **Create** > **Apps**

1. In the **Connected Apps** section, click **New**, and define the Connected App as follows:
  - Connected App Name: MyConference
  - API Name: MyConference
  - Contact Email: enter your email address
  - Enabled OAuth Settings: Checked
  - Callback URL: http://localhost:3000/oauthcallback.html
  - Selected OAuth Scopes: Full Access (full)

    ![](images/connected-app.jpg)

1. Click **Save**.


## Step 2: Install the Supporting Files

1. Download and unzip [this file](https://github.com/ccoenraets/salesforce-developer-workshop/archive/master.zip), or clone [this repository](https://github.com/ccoenraets/salesforce-developer-workshop)

1. Using your favorite code editor, examine the code in **client/index.html**:
    - It doesn't have any HTML markup inside the body tag. The HTML is build dynamically in JavaScript in the app.js file.
    - It uses ratchet.css. [Ratchet](http://goratchet.com/) is a simple CSS toolkit that provides styles for mobile applications.
    - It uses [ForceJS](https://github.com/ccoenraets/forcejs), to integrate with Salesforce.

1. Using your favorite code editor, examine the code in **client/app.js**:
    - It includes the logic to manage a single page application and generate HTML pages on the fly.
    - The getSessionList() function is responsible for retrieving the list of sessions from your Salesforce instance. You will implement this function in this module. 
    - The getSessionDetails() function is responsible for retrieving the list of sessions from your Salesforce instance. You will implement this function in this module. 
    - The showSessionList() function is responsible for generating the HTML for the session list page
    - The showSessionDetails() function is responsible for generating the HTML for the session details page
    - The router object is responsible for detecting hashtag changes in the URL and loading the corresponding page 

1. Using your favorite code editor, examine the code in **client/oauthcallback.html**:

    At the end of the OAuth workflow, the Salesforce authentication process loads the redirect URI you specified in your Connected App and passes the access token and other OAuth values (server instance, refresh token, etc.) in the query string. Your redirect URI page simply needs to parse the query string, extract the access token and the other OAuth values, and pass that information back to your application by invoking the oauthCallback() function you will code in Step 4.

1. Using your favorite code editor, examine the code in **server.js**. server.js implements a small HTTP server that provides two features:
    - Web server for static content. The document root for the web server is the client directory.
    - Proxy for Salesforce REST requests. Because of the browser’s cross-origin restrictions, your JavaScript application hosted on your own server (or localhost) will not be able to make API calls directly to the *.salesforce.com domain. The solution is to proxy your API calls through your own server.


## Step 3: Start the Node.js server

1. Open Terminal (Mac) or a Command prompt (Windows)

1. Navigate (cd) to the **salesforce-developer-advanced** (or salesforce-developer-advanced-master) directory

1. Install the Node.js server dependencies:

    ```
    npm install
    ```

1. Start the server:  

    ```
    node server
    ```

## Step 4: Authenticate with Salesforce using OAuth

1. Using your favorite code editor, open **index.html** in **salesforce-developer-advanced**

1. In the last script block (right after the ** Initialize forcejs here** comment), initialize the force js library and initiate the login process: 

    ```
    force.init({
        appId: 'YOUR_CONSUMER_KEY',
        proxyURL: 'http://localhost:3000'
    });

    force.login(router.start, function (error) {
        alert('Login failed: ' + error);
    });
    ```

1. In **Setup** (back in Salesforce), click **Build** > **Create** > **Apps**. In the **Connected Apps** section, click **MyConference**, and copy the **Consumer Key** to your clipboard.

    ![](images/consumer-key.jpg)

1. In index.html, replace YOUR&#95;CONSUMER_KEY with the consumer key you copied to your clipboard

1. Test the application
  - Open a browser and access [http://localhost:3000](http://localhost:3000)
  - Login with your Developer Edition credentials
  - Open the browser console: you should see the **OAuth authentication succeeded** message

  > It may take a few minutes for a Connected App to be available after you create it. If you get this message: **error=invalid_client_id&error_description=client%20identifier%20invalid**, wait a few minutes and try again.


## Step 5: Using the REST APIs

1. In app.js, implement the **getSessions()** function implemented as follows:

    ```
    function getSessionList() {
        var soql = "SELECT Session__r.Id, Session__r.Name FROM Session_Speaker__c";
        force.query(soql, success, error);
    }
    ```

1. Implement the **getSessionDetails()** function as follows:

    ```
    function getSessionDetails(sessionId, success, error) {
        var soql = "SELECT Session__r.Name, " +
            "Session__r.Session_Date__c, " +
            "Speaker__r.First_Name__c, " +
            "Speaker__r.Last_Name__c " +
            "FROM Session_Speaker__c " +
            "WHERE Session__r.Id = '" + sessionId + "'";
        force.query(soql, success, error);
    }
    ```

1. Test the application
  - Open a browser and access [http://localhost:3000](http://localhost:3000)
  - Login with your Developer Edition credentials
  - You should now see the list of sessions
  - Click a session to see the details

> This is just the starting point for building a custom application written in JavaScript, authenticating with Salesforce using OAuth, and accessing Salesforce data using the REST APIs. If you are planning on building a real-life application based on this architecture, consider using a JavaScript framework such as [Backbone.js](http://backbonejs.org/) or [AngularJS](https://angularjs.org/) with [Ionic](http://ionicframework.com/).



<div class="row" style="margin-top:40px;">
<div class="col-sm-12">
<a href="Using-JavaScript-in-Visualforce-Pages.html" class="btn btn-default"><i class="glyphicon glyphicon-chevron-left"></i> Previous</a>
<a href="Testing.html" class="btn btn-default pull-right">Next <i class="glyphicon glyphicon-chevron-right"></i></a>
</div>
</div>
