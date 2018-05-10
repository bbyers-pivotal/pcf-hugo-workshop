+++

Categories = ["lab"]
Tags = ["scaling","microservices","cloudfoundry","dotnet"]
date = "2017-08-29T07:49:11-04:00"
title = "Lab 5: Dot Net Scale an Application"
weight = 5

+++

### Goal

To take a previously deployed microservice and demonstrate how to scale the applicaiton in the Application Manager and through the command line.

<!--more-->

Prerequisites
--

### Step 1
##### Scale the Application in Apps Manager

Applications can be scaled via the command line or the console. When we talk about scale, there are two different types of scale: Vertical and Horizontal. Read [Scaling Apps](http://docs.cloudfoundry.org/devguide/deploy-apps/cf-scale.html) doc on more details on scaling applications.

When you vertically scale your application, you are increasing the amount of memory made available to your application. You would vertically scale your application while profiling your app, do performance tuning and to find the best memory settings before you deploy it in production.
Scaling your application horizontally means that you are adding application instances to increase your application throughput and performance under load.

Lets vertically scale the application to 1 GB of RAM.

   ````bash
   $ cf scale <YOUR INITIALS>-env -m 1G
   ````


### Step 2
##### Scale the Application in the Console

Now scale your application down to 700 MB.

Next, lets scale up your application to 2 instances

   ````bash
   $ cf scale <YOUR INITIALS>-env -i 2
   ````


To check the status of your applications you can check from the command line to see how many instances your app is running and their current state

   ````bash
   $ cf app <YOUR INITIALS>-env
   ````


Once the second instance as started, scale the app back down to one instance.

You can also use the Autoscaler service from the marketplace and bind it to your app, so automatically scale it up or down.
<img src="/images/pcf-autoscaler.png" alt="Autoscaler" style="width: 70%;"/>


<br>
### Step 3
##### Verify the app from the Browser

To verify that the application is running, use the following curl commands (or use your browser) to retrieve data from the service or use a browser to access the URL:


<br>

---

## Using the Autoscaler
### Step 4 Scaling with Autoscaler

The Autoscaler is a Marketplace Service available to bind to your applications just like any other service. You have the ability to select what criteria causes your applicatton to scale up and down. For the purpose of this lab, we'll scale the application down from 2 instances to 1.

**Using either the App Manager or the CLI, be sure you have 2 application instances running of your Environment Viewer application. **

### Step 5 - Create & Bind to Autoscaler

Let's bind the Autoscaler Service to the Environment Viewer application. Navigate to the Marketplace Services.

  <img src="/images/dev-space-services.png" alt="Services"  style="width: 70%; "/>

Select the Autoscaler Service.

  <img src="/images/auto-scaler-service.png" alt="Auto Scaler" style="width: 70%; "/>

Click the Select this Plan button and enter in a few details

   ```
   Instance Name: <YOUR INITIALS>-env-as
   Space: Default should be fine. It needs to match the name of the space where cities-service lives
   Bind to App: Select your <YOUR INITIALS>-env application
   ```

Once the data is entered, click the Add button.    

   <img src="/images/env-create-as.png" alt="Services"  style="width: 70%; "/>

Now click on the App Autoscaler link that is next your newly created service instance.

   <img src="/images/env-as.png" alt="Autoscaler Details"  style="width: 70%; "/>

### Step 6 - Configure Autoscaler

Next, click the Manage link in the upper right.

<img src="/images/env-as-parms.png" alt="Autoscaler Parameters" style="width: 70%; "/>

Before we can enable the Autoscaler, set the minimum and maximum number of instances. Click the edit button next to the Instances and enter these values:

   ```
   Minimum: 1
   Maximum: 3
   ```
And save those values. Now, edit the Scaling rules and enter in values of your chosing. For example, change the rule to HTTP Throughput and enter 10 for scaling down and 50 for scaling up. Save these values. Finally, slide the button next to Disabled to enable the service.

<img src="/images/env-as-enbled.png" alt="Autoscaler Enabled" style="width: 70%; "/>

Navigate to your application in Apps Manager and watch the number of instances go down to one. Wait a few minutes and refresh your browser to see the change.

##### Discussion

In this part of the lab we app and scaled the app. How do you horizontally scale your applications today?
