+++

Categories = ["lab"]
Tags = ["blue-green","microservices","cloudfoundry","dotnet"]
date = "2017-08-29T07:49:11-04:00"
title = "Lab 6: Dot Net Blue Green Deployments"
weight = 6

+++

### Goal

To take a previously deployed microservice and demonstrate how to bind it to backing services.

<!--more-->

Prerequisites
--

You have successfully completed the previous lab.

In this lab we are going to do a green-blue deployment using cf commands. We'll also learn about alternative ways of doing the same thing through a plugin and scripting commands.

<br>

### Step 1
##### Process of Blue Green Deployment

  Review the CF Document for blue green deployment [Using Blue-Green Deployment to Reduce Downtime and Risk](https://docs.pivotal.io/pivotalcf/devguide/deploy-apps/blue-green.html)

  In summary Blue-green deployment is a release technique that reduces downtime and risk by running two identical production environments called Blue and Green.


  <img src="/images/blue-green-process.png" alt="Blue Green Deployment Process" style="width: 100%;"/>


There are three different options in this lab to do blue-green deployment. We'll walk through the first one and talk about the other optons.

##### Option 1

First push a new version of the app with a blue route.

    $ cd pcf-dotnet-environment-viewer/ViewEnvironment
    // Push the app version v1 with the hostname as blue
    $ cf push <YOUR INITIALS>-env-v1 --hostname <YOUR INITIALS>-env-blue -f manifest.yml
    // Map your outside route to this blue version
    $ cf map-route <YOUR INITIALS>-env-v1 apps.run.azure.pcf.cloud --hostname <YOUR INITIALS>-env
    $ cf apps // Check the apps and the routes

Next, you can push a new version of the app with a green route.

    // Push the app version v2 with the hostname as green
    $ cf push <YOUR INITIALS>-env-v2 --hostname <YOUR INITIALS>-env-green -f manifest.yml
    // Map the outside route to this green version. Now your outside route is mapped to both bluae and green
    $ cf map-route <YOUR INITIALS>-env-v2 apps.run.azure.pcf.cloud --hostname <YOUR INITIALS>-env
    // Unmap the outside route to the blue version. All the traffic is now directed to v2
    $ cf unmap-route <YOUR INITIALS>-env-v1 apps.run.azure.pcf.cloud --hostname <YOUR INITIALS>-env


##### Option 2

Cloud Foundry plugin [Autopilot](https://github.com/concourse/autopilot) does blue green deployment, albeit it takes a different approach to other zero-downtime plugins. It does not perform any complex route re-mappings instead it leans on the manifest feature of the Cloud Foundry CLI. The method also has the advantage of treating a manifest as the source of truth and will converge the state of the system towards that. This makes the plugin ideal for continuous delivery environments.

You can download the latest release of the autopilot plugin from the github releases page [here](https://github.com/contraband/autopilot/releases)

To install the plugin you run the following command from the location that you saved the autoplugin.

   ```bash
   cf install-plugin autopilot
   ```

Once the plugin is installed ...

   ```bash
   $ cd pcf-dotnet-environment-viewer/ViewEnvironment
   // Append the build number to the app Name
   $ nano manifest.yml // Change the app name and append the build number, on Windows use wordpad manifest.yml
   ```

The contents of the manifest should look like this

   ```bash
   ---
   applications:
   - name: <YOUR INITIALS>-env-b100
     memory: 512m
     stack: windows2012R2
     buildpack: hwc_buildpack
   ```

   ```bash
   $ cf zero-downtime-push <YOUR INITIALS>-cities-hello -f manifest.hello
   ```

##### Option 3 (This is as bash script and works only on Linux/OSX)

If you would like to inject build numbers in your app names here is a script you could use to do blue green deployments in the cities-hello directory which only works on a Mac

   ```bash
    Usage: blue-green.sh <app-name> <build-number> <domain>
   ```

  ````bash

  $ ./blue-green.sh  <YOUR INTIALS>-env 1001 <YOUR INITIALS>
  $ cf apps // You should see your app build 1001 and the Route
  ````

  Now push the new build 1002 of the app

  ````bash
  $ ./blue-green.sh  <YOUR INITIALS>-cities-hello 1002 example.com
  $ cf apps // You should see your app build 1002 and the same route mapped to the new build

  ````

##### Discussion

In this lab we deployed the application using a blue green script without any downtime.
This script / methodology can be used in your CD pipeline to build and deploy Cloud Native Apps with zero downtime.

    How do you do Continuous Deployment and Delivery with zero downtime today?
