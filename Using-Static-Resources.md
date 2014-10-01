---
layout: module
title: Module 9&#58; Using Static Resources
---
In this module, you deploy the Conference Agenda app inside a Visualforce Page in your Salesforce instance. 


## Step 1: Upload the Agenda app as a static resource in Salesforce

1. Zip up the content of the **salesforce-developer-advanced** directory

1. In Setup, click **Build** > **Develop** > **Static Resources**

1. Click new
 
1. Specify **AgendaApp** as the **Name**
 
1. Click the **Choose File** button, and select the zip file you just created

1. Click **Save**


## Step 2: Create a Visualforce Page

1. In the **Developer Console**, select **File** > **New** > **Visualforce Page**, specify **Agenda** as the page name and click **OK**

1. Implement Agenda as follows:

    ```
    <apex:page sidebar="false" apiVersion="31" showHeader="false" docType="html" applyBodyTag="false">
    <html>
    <head>
        <meta charset="utf-8" />
        <meta name="viewport" content="initial-scale=1, maximum-scale=1, user-scalable=no" />
    	<link href="{!URLFOR($Resource.conference, 'css/ratchet.css')}" rel="stylesheet"/>
    	<link href="{!URLFOR($Resource.conference, 'css/pageslider.css')}" rel="stylesheet"/>
    	<link href="{!URLFOR($Resource.conference, 'css/styles.css')}" rel="stylesheet"/>
    </head>
    
    <body>
        <script src="{!URLFOR($Resource.conference, 'lib/jquery.js')}"></script>
        <script src="{!URLFOR($Resource.conference, 'lib/router.js')}"></script>
        <script src="{!URLFOR($Resource.conference, 'lib/pageslider.js')}"></script>
        <script src="{!URLFOR($Resource.conference, 'lib/force.js')}"></script>
        <script src="{!URLFOR($Resource.conference, 'js/app.js')}"></script>
    	<script>
    	    // Initialize forcejs here
        </script>    
    </body>
    </html>    
    </apex:page>
    ```
    
1. In the last script block (right after the ** Initialize forcejs here** comment), initialize the force js library as follows: 

    ```
    force.init({accessToken: '{!$Api.Session_ID}'});
    router.start();
    ```

1. Click the Preview button to test the application.

<div class="row" style="margin-top:40px;">
<div class="col-sm-12">
<a href="Creating-a-Controller-Extension.html" class="btn btn-default"><i class="glyphicon glyphicon-chevron-left"></i> Previous</a>
<a href="Using-the-Salesforce1-Platform-APIs.html" class="btn btn-default pull-right">Next <i class="glyphicon glyphicon-chevron-right"></i></a>
</div>
</div>
