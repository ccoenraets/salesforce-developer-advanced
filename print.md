---
layout: print
title: Advanced Salesforce Developer Workshop
---
# Overview

In this tutorial, you use the Salesforce Platform to build a conference management application that allows conference
administrators to manage all the
aspects of a conference: sessions, speakers, hotels, etc. You also create a simple consumer-facing application that allows conference attendees to view the conference schedule, and learn more about sessions and speakers.

## What You Will Learn

- Use JavaScript in Visualforce Pages
- Authenticate using OAuth
- Access Salesforce data from JavaScript using JavaScript Remoting
- Create a Custom Application using the REST APIs
- Using Static Resources
- Host a single page application inside a Visualforce Page
- Create Canvas Applications
- Create Unit Tests
- Create Batch Processes

## Prerequisites

- To complete this workshop, all you need is a modern browser and a connection to the Internet
- No prior knowledge of Salesforce is required
- A working knowledge of Object-Oriented Programming is assumed


## Browser Requirements

The following browsers are supported when working with the Developer Console:

  - Most recent version of Google Chrome
  - Most recent version of Mozilla Firefox
  - Most recent version of Safari
  - Internet Explorer 9 or higher


<!-- ############################# -->

# Module 1&#58; Creating a Developer Edition Account

In this module, you create a Developer Edition Account that provides you with a full-featured but isolated Salesforce environment to perform the exercises in this workshop.

> If you already signed up for a Developer Edition account, you can skip the instructions below and go directly to [Module 2](Creating-the-Data-Model.html).


## Steps

1. Open a browser and access the following URL: [http://developer.salesforce.com/signup](http://developer.salesforce.com/signup)

2. Fill in the signup form:
  - Enter your First Name and Last Name
  - For **Email**, enter an email address you have access to at this time (you will need to open an activation email)
  - For **Username**, specify a unique user name in the form of an email address. For example: **firsname.lastname@workshop.com** (It doesn't have to be an existing email address: the Username is not used to send you emails)
  - Check the Master Subscription Agreement checkbox and click the **Sign Me Up** button

3. Check your email. You will receive an activation email for your Developer Edition Account.

4. Click the link in the activation email. Enter your new password information, and click **Save**.

<!-- ############################# -->

# Module 2&#58; Importing Workshop Assets

In this module, you import an unmanaged package that loads custom objects, and tabs for the conference application.

## Step 1: Import the Package

1. Login into your Developer Edition account

1. Access the following URL:

    [https://login.salesforce.com/packaging/installPackage.apexp?p0=04tj0000000ToNE](https://login.salesforce.com/packaging/installPackage.apexp?p0=04tj0000000ToNE)

1. On the **Package Installation Details** screen, click the **Continue** button

1. Click **Next**, **Next**, **Install**

## Step 2: Examine the Imported Assets

1. Click the **Setup** link (upper right corner)

    ![](images/setup.jpg)

1. To examine the imported **Custom Objects**:
    - In the left navigation, select **Build** > **Create** > **Objects**
    - Click any of the objects (Session, Speaker, Session_Speaker), and examine the **Standard Fields** and **Custom Fields & Relationships** sections.

1. To examine the imported **Tabs**:
    - In the left navigation, select **Build** > **Create** > **Tabs**

1. To examine the imported **App**:
    - In the left navigation, select **Build** > **Create** > **Apps**
    - Click the **Conference** link, click the **Edit** button, and examine the **Selected Tabs** and the user profiles that have access to the application (System Administrator).

## Step 3: Enter Sample Data

1. Select **Conference** in the App selector (upper right corner of the screen)

    ![](images/conference-app.jpg)

1. Click the **Speakers Tab**, click **New**, and add a few sample speakers

1. Click the **Sessions Tab**, click **New**, and  add a few sample sessions

1. To assign speakers to a session:
  - In the details view for a session, click **New Session Speaker**
  - Click the magnifier icon next to the Speaker field, select a speaker in the Speaker lookup dialog and click **Save**


<!-- ############################# -->

# Module 3&#58; Using JavaScript in Visualforce Pages

In this module, you create a custom controller with a method that returns a list of conference hotels. You create a Visualforce page that invokes that method using JavaScript Remoting and uses the Google Maps SDK to display the hotels on a map.

![](images/hotelmap.jpg)

## Step 1: Create the Hotel Object

1. In **Setup**, select **Build** > **Create** > **Objects**

2. Click **New Custom Object**, and define the Hotel Object as follows:
  - Label: **Hotel**
  - Plural Label: **Hotels**
  - Object Name: **Hotel**
  - Record Name: **Hotel Name**
  - Data Type: **Text**

3. Click **Save**

4. In the **Custom Fields & Relationships** section, click **New**, and create a **Location** field defined as follows:
    - Data Type: **Geolocation**
    - Field Label: **Location**
    - Latitude and Longitude Display Notation: **Decimal**
    - Decimal Places: **7**
    - Field Name: **Location**

    Click **Next**, **Next**, **Save**

5. Create a Tab for the Hotel object
  - In **Setup**, select **Build** > **Create** > **Tabs**
  - In the **Custom Object Tabs** section, click **New**
  - Select the **Hotel** object and **Building** as the Tab Style Icon
  - Click **Next**, **Next**
  - Uncheck the **Include Tab** checkbox, check the **Conference** checkbox, and click **Save**

    ![](images/hotel-tab.jpg)

6. Enter a couple of hotels with location information. For example:
  - Marriott Marquis (Latitude: 37.785143 Longitude: -122.403405)
  - Hilton Union Square (Latitude: 37.786164 Longitude: -122.410137)
  - Hyatt (Latitude: 37.794157 Longitude: -122.396311)

    > Make sure you include the minus sign when entering the longitude.

    ![](images/marriott.jpg)



## Step 2: Create the HotelRemoter Controller

1. In the Developer Console, select **File** > **New** > **Apex Class**, specify **HotelRemoter** as the class name and click **OK**

1. Implement the class as follows:

    ```
    global with sharing class HotelRemoter {

        @RemoteAction
        global static List<Hotel__c> findAll() {
            return [SELECT Id, Name, Location__Latitude__s, Location__Longitude__s
                        FROM Hotel__c];
        }

    }
    ```

1. Save the file

## Step 3: Create a Visualforce Page with Google Maps

1. In the Developer Console, select **File** > **New** > **Visualforce Page**, specify **HotelMap** as the page name and click **OK**

1. Implement HotelMap as follows:

    ```
    <apex:page sidebar="false" showheader="false">

    <head>
    <style type="text/css">
      html { height: 100% }
      body { height: 100%; margin: 0; padding: 0 }
      #map-canvas { height: 100% }
    </style>
    <script src="https://maps.googleapis.com/maps/api/js?sensor=false"></script>
    <script>
    var map;

    function initialize() {
        var mapOptions = {
            center: new google.maps.LatLng(37.784173, -122.401557),
            zoom: 15
        };
        map = new google.maps.Map(document.getElementById("map-canvas"), mapOptions);
    }

    google.maps.event.addDomListener(window, 'load', initialize);

    </script>
    </head>
    <body>
      <div id="map-canvas"/>
    </body>

    </apex:page>
    ```

1. Save the file

1. Click the **Preview** button (upper left corner) to test the HotelMap page in the browser

## Step 4: Display the Hotels on the Map

1. Assign **HotelRemoter** as the controller for the **HotelMap** Visualforce page:

    ```
    <apex:page sidebar="false" showheader="false" controller="HotelRemoter">
    ```

1. Define a function named loadHotels() implemented as follows (right after the initilize() function):

    ```
    function loadHotels() {
        Visualforce.remoting.Manager.invokeAction('{!$RemoteAction.HotelRemoter.findAll}',
            function(result, event){
                if (event.status) {
                    for (var i=0; i<result.length; i++) {
                        var id = result[i].Id;
                        var name = result[i].Name;
                        var lat = result[i].Location__Latitude__s;
                        var lng = result[i].Location__Longitude__s;
                        addMarker(id, name, lat, lng);
                    }
                } else {
                    alert(event.message);
                }
            },
            {escape: true}
        );
    }
    ```

1. Define the addMarker() function implemented as follows (right after the loadHotels() function):

    ```
    function addMarker(id, name, lat, lng) {
        var marker = new google.maps.Marker({
      			position: new google.maps.LatLng(lat, lng),
      			map: map,
      			title: name
        });
        google.maps.event.addListener(marker, 'click', function(event) {
            window.top.location = '/' + id;
        });
  	}
    ```

1. Invoke loadHotels() as the last line of the **initialize()** function:

    ```
    loadHotels();
    ```

1. Save the file

1. Click the **Preview** button (upper left corner) to test the HotelMap page in the browser. You should now see markers on the map representing the hotels you entered in Step 1.


<!-- ############################# -->

# Module 4&#58; Using the Salesforce1 Platform APIs

In this module, you create an application that runs outside your Salesforce instance: it uses OAuth to authenticate with Salesforce, and the REST APIs to access Salesforce data.

![](images/api.jpg)

## Requirement

You need Node.js to perform the exercises in this module. If you don't already have Node.js installed on your system, you can install it [here](http://nodejs.org/).

## Step 1: Create a Connected App

1. In Setup, click **Build** > **Create** > **Apps**

1. In the **Connected Apps** section, click **New**, and define the Connected App as follows:
  - Connected App Name: **MyConference**
  - API Name: **MyConference**
  - Contact Email: **enter your email address**
  - Enabled OAuth Settings: **Checked**
  - Callback URL: **http://localhost:3000/oauthcallback.html**
  - Selected OAuth Scopes: **Full Access (full)**

    ![](images/connected-app.jpg)

1. Click **Save**.


## Step 2: Install the Supporting Files

1. Download and unzip [this file](https://github.com/ccoenraets/salesforce-developer-advanced/archive/master.zip), or clone [this repository](https://github.com/ccoenraets/salesforce-developer-advanced)

1. Using your favorite code editor, examine the code in **client/index.html**:
    - It doesn't have any HTML markup inside the body tag. The HTML is built dynamically in JavaScript in the app.js file.
    - It uses ratchet.css. [Ratchet](http://goratchet.com/) is a simple CSS toolkit that provides styles for mobile applications.
    - It uses [ForceJS](https://github.com/ccoenraets/forcejs) to integrate with Salesforce.

1. Using your favorite code editor, examine the code in **client/js/app.js**:
    - It includes the basic logic to manage a single page application and generate HTML pages on the fly.
    - The **getSessionList()** function is responsible for retrieving the list of sessions from your Salesforce instance. You will implement this function in this module.
    - The **getSessionDetails()** function is responsible for retrieving the details of a specific session from your Salesforce instance. You will implement this function in this module.
    - The **showSessionList()** function is responsible for generating the HTML for the session list page
    - The **showSessionDetails()** function is responsible for generating the HTML for the session details page
    - The **router** object is responsible for detecting hashtag changes in the URL and loading the corresponding page

1. Using your favorite code editor, examine the code in **client/oauthcallback.html**:

    At the end of the OAuth workflow, the Salesforce authentication process loads the redirect URI you specified in your Connected App (in this case: http://localhost:3000/oauthcallback.html) and passes the access token and other OAuth values (server instance, refresh token, etc.) in the query string. oauthcallback.html simply passes that information to the ForceJS library which uses it to make REST API calls to your Salesforce instance.

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

    If you are on **Windows** and run into issues with npm after installing Node.js, try this:
    - Add **c:\Program Files\Nodejs** to your path or run **"C:\Program Files\Nodejs\npm" install** instead of npm install
    - Create an **npm** directory in C:\Users\[yourname]\Appdata\Roaming

1. Start the server:

    ```
    node server
    ```

## Step 4: Implement Salesforce Authentication using OAuth

1. Using your favorite code editor, open **index.html** in **salesforce-developer-advanced/client**

1. In the last script block (right after the **Initialize forcejs here** comment), initialize the ForceJS library and initiate the login process:

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
  - Login with your Developer Edition credentials. After you successfully authenticate with Salesforce, you will see a blank screen. In the next step you make REST API calls to display the list of sessions.

  > It may take a few minutes for a Connected App to be available after you create it. If you get this message: **error=invalid_client_id&error_description=client%20identifier%20invalid**, wait a few minutes and try again.


## Step 5: Using the REST APIs

1. In client/js/app.js, implement the **getSessionList()** function as follows:

    ```
    function getSessionList(success, error) {
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

<!-- ############################# -->

# Module 5&#58; Using Static Resources

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

## Note on Data Access Strategy

When an application is running inside a Visualforce page, it may be advantageous to use JavaScript Remoting or Remote Objects instead of the REST APIs because JavaScript Remoting or Remote Objects calls don't count towards your governor limits for API calls. [Module 3](Using-JavaScript-in-Visualforce-Pages.html) provides an example of using JavaScript Remoting.

<!-- ############################# -->

# Module 6&#58; Using Canvas Applications

In this module, you deploy a Node.js application to Heroku using the Heroku button. You define a Canvas application to integrate the Node.js application in Salesforce, and you add the Canvas application to the Contact Page Layout.

## Step 1: Create a Connected App

1. In Setup, click **Build** > **Create** > **Apps**

1. In the **Connected Apps** section, click **New**, and define the Connected App as follows:
  - Connected App Name: **MyCanvas**
  - API Name: **MyCanvas**
  - Contact Email: **enter your email address**
  - Enabled OAuth Settings: **Checked**
  - Callback URL: **http://localhost** (you will change this later)
  - Selected OAuth Scopes: **Full Access (full)**
  - Force.com Canvas: **Checked**
  - Canvas App URL: **http://localhost/signedrequest** (you will change this later)
  - Access Method: **Signed Request (POST)**
  - Locations: **Layouts and Mobile Cards, Publisher**

1. Click **Save**

1. On the Connected App details page, click the **Click to reveal** link next to **Consumer Secret**, and copy your secret to your clipboard. You will need it in step 4.

    ![](images/reveal_secret.png)


## Step 2: Configure Permissions

1. On the Connected App details page, click the **Manage** button

    ![](images/manage_canvas.png)

1. Click **Edit**

1. For Permitted Users, select **Admin approved users are pre-authorized**

1. Click **Save**

1. Click **Manage Profiles**

1. Check **System Administrator**

1. Click **Save**


## Step 3: Sign Up for Heroku

> You can skip this step if you already have a Heroku account.

1. Open a browser and access the following URL:

    [http://heroku.com/signup](http://heroku.com/signup)

1. Enter an email address you have access to at this time (you will need to open an activation email) and click **Signup**

1. Check your email. You will receive an activation email for your Heroku account.

1. Click the link in the activation email. Enter your new password information, and click **Save**.


## Step 4: Deploy the App to Heroku

1. Access the following GitHub repository:

    [https://github.com/ccoenraets/salesforce-canvas-demo](https://github.com/ccoenraets/salesforce-canvas-demo)

1. Click the server.js file and examine the code:

    The server defines a **/signedrequest** endpoint configured to respond to **POST** requests. This is the endpoint you specified in the Canvas parameters of your Connected App. When loading your Canvas app, your Salesforce instance will post encoded data to that endpoint including contextual information (logged in user, current object, ...), and an authenticated session id that your app can use to make API calls to your Salesforce instance.

1. Go back to the repository home page and click the **Deploy to Heroku** button
    ![](images/heroku_deploy.png)
    - For **App Name**, specify a name for your application. For example, if you specify my-canvas, your application will be available at http://my-canvas.herokuapp.com. Your app name has to be unique on the herokuapp.com domain.
    - For **CONSUMER_SECRET** in the deployment wizard, paste the consumer secret of the connected app you copied in step 1.
    - Click the **Deploy For Free** button

1. In Salesforce, go back to your Connected App and adjust the URLs based on your Heroku app name:
     - For **Callback URL**, specify: https://my-canvas.herokuapp.com
     - For **Canvas App URL**, specify: https://my-canvas.herokuapp.com/signedrequest

     > Replace **my-canvas** with your own app name.


## Step 5: Add the Canvas App to the Page Layout

1. In **Setup** mode, select **Build** > **Customize** > **Contacts** > **Page Layouts**

1. Click **Edit** next to **Contact Layout**

1. Select **Canvas Apps** in the upper left corner, drag **MyCanvas**, and drop it immediately after the Description field.

1. Mouse over the Canvas in the layout, click the wrench in the upper right corner, specify **550** for the Canvas Height, and click OK.

1. Click **Save** (upper left corner) to save the layout

![](images/canvas_page_layout.png)


## Step 6: Test the Application

1. Access a Contact

1. The Canvas application should appear in the middle of the page layout

    ![](images/canvas.png)

1. Scan the QR code with your mobile phone to create a new contact entry

<!-- ############################# -->

# Module 7&#58; Testing

In this module, you write tests for the RejectDoubleBooking trigger you created in module 6.

## Step 1: Create a Trigger that Rejects Double Bookings

1. In the Developer Console, click **File** > **New** > **Apex Trigger**

1. Specify **RejectDoubleBooking** as the trigger name, **Session&#95;Speaker__c** as the sObject, and click **Submit**

1. Implement the trigger as follows:

    ```
    trigger RejectDoubleBooking on Session_Speaker__c (before insert, before update) {

        for(Session_Speaker__c sessionSpeaker : trigger.new) {

            // Retrieve session information including session date and time
            Session__c session = [SELECT Id, Session_Date__c FROM Session__c
                                    WHERE Id=:sessionSpeaker.Session__c];

            // Retrieve conflicts: other assignments for that speaker at the same time
            List<Session_Speaker__c> conflicts =
                [SELECT Id FROM Session_Speaker__c
                    WHERE Speaker__c = :sessionSpeaker.Speaker__c
                    AND Session__r.Session_Date__c = :session.Session_Date__c];

            // If conflicts exist, add an error (reject the database operation)
            if(!conflicts.isEmpty()){
                sessionSpeaker.addError('The speaker is already booked at that time');
            }

        }

    }
    ```

1. Save the file

1. Test the trigger:
  - Assign a speaker to a session scheduled at a time the speaker is available and make sure it still works
  - Assign a speaker to a session scheduled at the same time as another session the speaker is already assigned to: you should see the error message

## Step 2: Create a Test Class

1. In the Developer Console, select **File** > **New** > **Apex Class**, specify **TestRejectDoubleBooking** as the class name and click **OK**

1. Make the class **private**, and add the **@isTest** class annotation:

    ```
    @isTest
    private class TestRejectDoubleBooking{

    }
    ```

## Step 3: Add a Test Method to Test Single Bookings

1. Add a **TestSingleBooking()** method to the TestRejectDoubleBooking class to make sure the trigger does not prevent a valid speaker booking:

    ```
    static testmethod void TestSingleBooking() {
        Datetime now = System.now();

        Speaker__c speaker = new Speaker__c(First_Name__c='John', Last_Name__c='Smith');
        insert speaker;

        Session__c session = new Session__c(Name='Some Session', Session_Date__c=now);
        insert session;

        Session_Speaker__c assignment =
            new Session_Speaker__c(Session__c=session.Id, Speaker__c=speaker.Id);
        Test.startTest();
        Database.SaveResult result = Database.insert(assignment, false);
        Test.stopTest();

        System.assert(result.isSuccess());
    }
    ```

1. Save the file

1. Click **Run Test** in the upper right corner of the code editor

1. Click the **Tests** tab at the bottom of the code editor, and examine the test results.

    ![](images/test1.jpg)


## Step 4: Add a Test Method to Test Double Bookings

1. Add a **TestDoubleBooking()** method to the TestRejectDoubleBooking class to make sure trigger actually rejects double bookings:

    ```
    static testmethod void TestDoubleBooking() {
        Datetime now = System.now();

        Speaker__c speaker = new Speaker__c(First_Name__c='John', Last_Name__c='Smith');
        insert speaker;

        Session__c session1 = new Session__c(Name='Session 1', Session_Date__c=now);
        insert session1;
        Session__c session2 = new Session__c(Name='Session 2', Session_Date__c=now);
        insert session2;

        Session_Speaker__c assignment1 =
            new Session_Speaker__c(Session__c=session1.Id, Speaker__c=speaker.Id);
        insert assignment1;

        Session_Speaker__c assignment2 =
            new Session_Speaker__c(Session__c=session2.Id, Speaker__c=speaker.Id);
        Test.startTest();
        Database.SaveResult result = Database.insert(assignment2, false);
        Test.stopTest();

        System.assert(!result.isSuccess());
    }
    ```

1. Save the file

1. Click **Run Test** in the upper right corner of the code editor

1. Click the **Tests** tab at the bottom of the code editor, and examine the test results.

    ![](images/test2.jpg)

<!-- ############################# -->

# Module 8&#58; Batch and Schedule

In this module, you create and execute a batch process to send reminder emails to the conference speakers.

## Step 1: Create the EmailManager class

1. In the Developer Console, click **File** > **New** > **Apex Class**. Specify **EmailManager** as the class name and click **OK**

1. Implement the class as follows:

    ```
    public class EmailManager {

        public static void sendMail(String address, String subject, String body) {
            Messaging.SingleEmailMessage mail = new Messaging.SingleEmailMessage();
            String[] toAddresses = new String[] {address};
            mail.setToAddresses(toAddresses);
            mail.setSubject(subject);
            mail.setPlainTextBody(body);
            Messaging.sendEmail(new Messaging.SingleEmailMessage[] { mail });
        }

    }
    ```

1. Click **File** > **Save** to save the file

## Step 2: Create the Batch Class

1. In the Developer Console, select **File** > **New** > **Apex Class**, specify **SendReminderEmail** as the class name and click **OK**

1. Make the class **global**, implement the **Batchable** interface, and define the three methods of the interface:

    ```
    global class SendReminderEmail implements Database.Batchable<sObject> {

        global Database.QueryLocator start(Database.BatchableContext bc) {

        }

        global void execute(Database.BatchableContext bc, List<Speaker__c> scope) {

        }

        global void finish(Database.BatchableContext bc) {

        }

    }
    ```

1. Declare three instance variables to store the query, the email subject, and the email body:

    ```
    global String query;
    global String subject;
    global String body;
    ```

1. Define a **constructor** implemented as follows:

    ```
    global SendReminderEmail(String query, String subject, String body) {
        this.query = query;
        this.subject = subject;
        this.body = body;
    }
    ```

1. Implement the **start()** method as follows:

    ```
    global Database.QueryLocator start(Database.BatchableContext bc) {
        return Database.getQueryLocator(query);
    }
    ```

1. Implement the **execute()** method as follows:

    ```
    global void execute(Database.BatchableContext bc, List<Speaker__c> scope) {
        for (Speaker__c speaker : scope) {
            EmailManager.sendMail(speaker.Email__c, this.subject, this.body);
        }
    }
    ```

1. Save the file.


## Step 3: Run the Batch

1. Make sure you have assigned your own email address to one of the speakers

1. In the Developer Console, click **Debug** > **Open Execute Anonymous Window**

1. Type the following Apex code:

    ```
    String q = 'SELECT First_Name__c, Last_Name__c, Email__c FROM Speaker__c WHERE Email__c != null';
    SendReminderEmail batch = new SendReminderEmail(q, 'Final Reminder', 'The conference starts next Monday');
    Database.executeBatch(batch);
    ```

1. Click **Execute**

1. Check your email
