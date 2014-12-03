---
layout: module
title: Module 2&#58; Importing Workshop Assets
---
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


<div class="row" style="margin-top:40px;">
<div class="col-sm-12">
<a href="Creating-a-Developer-Edition-Account.html" class="btn btn-default"><i class="glyphicon glyphicon-chevron-left"></i> Previous</a>
<a href="Using-JavaScript-in-Visualforce-Pages.html" class="btn btn-default pull-right">Next <i class="glyphicon glyphicon-chevron-right"></i></a>
</div>
</div>
