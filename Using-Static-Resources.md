---
layout: module
title: Module 5&#58; Using Static Resources
---
In this module, you explore a different deployment option for the Conference app you created in the previous module. Instead of deploying it on your own server, you deploy it inside a Visualforce Page in your Salesforce instance. 


## Step 1: Upload the Application as a Static Resource

1. In the **salesforce-developer-advanced/client** directory, select the **css**, **fonts**, **js**, and **lib** directories and zip them up (compress them).

    > **Important:** Make sure you don't zip up the client directory itself. In other words, if you open the zip file, the css, fonts, js, and lib directories should be at the root level.

1. In Setup, click **Build** > **Develop** > **Static Resources**

1. Click **New**
 
1. Specify **ConferenceApp** as the **Name**
 
1. Click the **Choose File** button, and select the zip file you just created

1. Click **Save**


## Step 2: Create a Visualforce Page

1. In the **Developer Console**, select **File** > **New** > **Visualforce Page**, specify **ConferenceApp** as the page name and click **OK**

1. Implement the ConferenceApp page as follows:

    ```
    <apex:page sidebar="false" showHeader="false" docType="html" applyBodyTag="false">
    <html>
    <head>
        <meta charset="utf-8" />
        <meta name="viewport" content="initial-scale=1, maximum-scale=1, user-scalable=no"/>
    	<link href="{!URLFOR($Resource.ConferenceApp,'css/ratchet.css')}" rel="stylesheet"/>
    	<link href="{!URLFOR($Resource.ConferenceApp,'css/pageslider.css')}" rel="stylesheet"/>
    	<link href="{!URLFOR($Resource.ConferenceApp,'css/styles.css')}" rel="stylesheet"/>
    </head>
    
    <body>
        <script src="{!URLFOR($Resource.ConferenceApp,'lib/jquery.js')}"></script>
        <script src="{!URLFOR($Resource.ConferenceApp,'lib/router.js')}"></script>
        <script src="{!URLFOR($Resource.ConferenceApp,'lib/pageslider.js')}"></script>
        <script src="{!URLFOR($Resource.ConferenceApp,'lib/force.js')}"></script>
        <script src="{!URLFOR($Resource.ConferenceApp,'js/app.js')}"></script>
    	<script>
    	    // Initialize forcejs here
        </script>    
    </body>
    </html>    
    </apex:page>
    ```
    
1. In the last script block (right after the **Initialize forcejs here** comment), initialize the force js library as follows: 

    ```
    force.init({accessToken: '{!$Api.Session_ID}'});
    router.start();
    ```
    
    > Note that because you are running inside an authenticated session, you don't have to login using OAuth like you had to do in the previous module. All you have to do is initialize ForceJS with the existing access token (or session id).

1. Click the Preview button to test the application.

## Notes

<div class="row" style="margin-top:40px;">
<div class="col-sm-12">
<a href="Using-the-Salesforce1-Platform-APIs.html" class="btn btn-default"><i class="glyphicon glyphicon-chevron-left"></i> Previous</a>
<a href="Using-Canvas.html" class="btn btn-default pull-right">Next <i class="glyphicon glyphicon-chevron-right"></i></a>
</div>
</div>
