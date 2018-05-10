+++

Categories = ["lab"]
Tags = ["logging-metrics","cloudfoundry", "dotnet"]
date = "2017-08-29T07:49:11-04:00"
title = "Lab 4: Dot NET Logging and Metrics"
weight = 4

+++

### Goal

To take a previously deployed microservice and demonstrate how to view logs and metrics.

<!--more-->

Prerequisites
--

You have successfully completed the previous lab.

### Step 1
##### Tailing Logs from the Command Line

When you want to see the logs as you interact with your microservice, you can use the cf logs command as shown below. This will open a view into the application logs until you stop the process.

Run the following command and then open up the Dot Net Environment Viewer Application endpoint in a browser and watch the logs

   ````bash
   $ cf logs <YOUR INITIALS>-env
   ````

### Step 2
##### Viewing Recent Logs from the Command Line

When you want to view logs that have already occured, use the cf logs command with the --recent option. Run the command below and see what happens.

   ````bash
   $ cf logs <YOUR INITIALS>-env --recent
   ````

Please note that there is an internal limit on how far back the logs go back. This command is meant to retrieve logs that have just recently occurred. We'll talk about other alternatives a bit later.

### Step 3
##### Viewing the Logs inside Apps Manager

Another way of viewing the applicaiton logs is using Apps Manager. Navigate to your Org and Space, click on your application and click on the Logs tab towards the top left. The output you see here is the same as the cf logs command you previously ran. Notice the play button that allows you to tail the logs in the UI.  

### Step 4
##### PCF Metrics for Health, logging & events

Learning about how your application is performing is critical to help you diagnose and troubleshoot potential issues. Cloud Foundry gives you options for viewing the logs.

Open the metrics dashboard by opening up Apps Manager, navigating to your org and space, and clicking on your application. In the Instances section, you'll see a View in PCF Metrics link. Click this link.

<img src="/images/dotnet-pcf-metrics.png" alt="Metrics" sytle="width: 70%; "/>

This will open up the PCF Metrics window for your application. If you are prompted to log in again, enter the same credentials that you have used before.

<img src="/images/pcf-metrics-for-env.png" alt="Metrics" style="width: 70%;"/>

You can Monitor your Container Metrics, Network Metrics and Events for your app. Explore your logs, which shows all your app logs streamed using the Loggregator.

<img src="/images/metrics-architecture.png" alt="Metrics" style="width: 70%;"/>

### Step 5
##### Search & Highlight Log entries

Scroll down to the Logs section in PCF Metrics. Notice that you have the option to change the sort order of the logs, use the Type selection to choose which components you want to see in the logs as well as filtering the logs by keywords and highlighting words within them. You can also select a timeframe on the timeline to further limit the logs you see within the logging area.

Take a few minutes to navigate and interact with PCF Metrics
