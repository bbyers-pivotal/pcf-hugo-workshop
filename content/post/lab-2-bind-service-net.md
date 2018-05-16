+++

Categories = ["lab"]
Tags = ["dotnet","microservices","services"]
date = "2017-08-29T07:49:11-04:00"
title = "Lab 2: Binding Dot NET Apps to Services"
weight = 2

+++

### Goal

To take a previously deployed .NET microservices and demonstrate how to bind it to backing services.

<!--more-->

Prerequisites
--

You have successfully completed the previous lab.

Steps
--

In ths Lab you will bind your previous .NET applications to a MySQL database. You will create an instance of a database, bind it to your applicaiton via the command line, and then see how to accomplish the same thing by using the application manifest file.

## Dot Net Environment Viewer Application
### Step 1
##### Create a Database from Marketplace

1. Review the docs on Marketplace Services:

    [Managing Services](http://docs.pivotal.io/pivotalcf/devguide/services/managing-services.html)

2. Create a mysql service, name it as `<YOUR INITIALS>-env-db`

    You can create the service from the `cli` or launch the App Manager-> Select the Development Space [https://apps.run.pcf.cloud](https://apps.run.pcf.cloud) and login.
    Navigate to the marketplace and see the available services.

    <img src="/images/pcf-marketplace.png" alt="Marketplace Services" style="width: 70%;"/>
##### ---OR---
    You can create the service using the CLI.

    ````bash
    $ cf marketplace // check if mysql service is available
    $ cf create-service p-mysql 100mb <YOUR INITIALS>-env-db
    ````

### Step 2
##### Manually Binding the Service Instance to the Dot Net Environment Viewer Application

1. Review the docs on [Binding a Service Instance](http://doc-s.pivotal.io/pivotalcf/devguide/services/bind-service.html)
2. Bind the mysql instance `<YOUR INITIALS>-env-db` to your app cities-service
    You can bind from the App Manager or from the `cli`

    ````bash
    $ cf bind-service <YOUR INITIALS>-env <YOUR INITIALS>-env-db
    ````

3. Restart your Dot Net Environment Viewer application to inject the new database.

    ````bash
    $ cf restart <YOUR INITIALS>-env
    ````

    Notice that the application is now running.

4. Check the Env variables to see if the service is bound.
    You can do it from App Manager or from the `cli`

    ````bash
    $ cf env <YOUR INITIALS>-env
    ````

5. Open the URL of the application in a browser window. Notice that there is now an attendees section available to add new attendees. Also notice the bound services now include the information required to connect to MySQL. Take a few minutes to add a few attendees.

    <img src="/images/env-with-db.png" alt="Bound Environment Viewer" style="width: 70%;"/>

6. Check the MySQL database through the `Manage` link in the App Manager to see that it now contains data.

### Step 4
##### Binding Services via the Manifest

Next, edit the Dot Net Environment Viewer application with a manifest to help automate deployment.

1. Review the documentation: http://docs.pivotal.io/pivotalcf/devguide/deploy-apps/manifest.html
2. Edit the application manifest  `manifest.yml` in your `ViewEnvironment` folder

    ````bash
    $ nano manifest.yml
    ````

    On Windows

    ````bash
    wordpad.exe manifest.yml
    ````

3. Add the services binding `<YOUR INITIALS>-env-db` to your deployment manifest for Environment Viewer. This step will require you to look at the documentation link above.

4. Test your manifest by re-pushing your app with no parameters:

    ```bash
    $ cf push <YOUR INITIALS>-env
    ```

    Notice that using a manifest, you have moved the need for multiple commands into one location. In addition, using manifests gives you consistency between your deployment environments (Dev, QA, UAT, Performance, Production, etc) so you have a repeatable and documented way of deploying software.

    __NOTE__

    > The default manifest file for an app is `manifest.yml` and it if is present, it is automatically picked without specifying the manifest file option (e.g. -f my-manifest-file.yml).

5. Once you are comfortable of what we just did, let's clean up the resources.


    ```bash
    $ cf delete -r <YOUR INITIALS>-env
    $ cf delete-service <YOUR INITIALS>-env-db
    ```

## Dot Net Core

In the interest of time, we will not be walking through the steps to bind a service to a .NET Core application because they are fundamentally the same thing we have already done. When you are starting with a brand new project the steps would look like this:

#### Step 1 - Create the Service
#### Step 2 - Push the Application
#### Step 3 - Bind the Application to the Service

If you already have an application deployed on Cloud Foundry and now want to bind a service to it, the steps look like this:

#### Step 1 - Create the Service
#### Step 2 - Bind the Application to the Service
#### Step 3 - Restart the Application

##### Discussion

* How do you provision services today?
* How long does it take to get access to those services?
* What mechanisms do you use today to provide the connection information to the service?
***
