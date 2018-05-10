+++

Categories = ["lab"]
Tags = ["dotnet","microservices","cloudfoundry"]
date = "2017-08-29T07:49:11-04:00"
title = "Lab 1: Deploying Dot NET Applications on PCF"
weight = 1

+++

### Goal

To deploy a Microsoft Framework .NET Application and a .NET Core Application to Pivotal Cloud Foundry

<!--more-->

Prerequisites
--
1. Install .NET Core 2.0

    [.NET Core](https://www.microsoft.com/net/core)

2. Clone or Download the Dot Net Attendee Source Code

	[Dot Net Environment Viewer:  https://github.com/Pivotal-Field-Engineering/pcf-dotnet-environment-viewer](https://github.com/Pivotal-Field-Engineering/pcf-dotnet-environment-viewer)

	##### Get the source code repository app

	<img src="/images/dot-net-framework-git.png" alt="Git style="width: 40%;"/>

	Download the source code. Download as Zip file and save it in local folder

	```bash
	   unzip pcf-dotnet-environment-viewer-master.zip
	```
	##### ---OR---

	Fork and Clone

	[Dot Net Environment Viewer:  https://github.com/Pivotal-Field-Engineering/pcf-dotnet-environment-viewer](https://github.com/Pivotal-Field-Engineering/pcf-dotnet-environment-viewer)

	For Linux/Mac:
	```bash
	$git clone https://github.com/Pivotal-Field-Engineering/pcf-dotnet-environment-viewer.git
	```

	For Windows
	```
	C:\<Some Directory to save code>\> git clone https://github.com/Pivotal-Field-Engineering/pcf-dotnet-environment-viewer.git
	```

3. Clone or Download the ASP.NET Core 2 Sample

	[ASP.NET Core Repo:  https://github.com/rossr3-pivotal/asp-dot-net-core-2-sample](https://github.com/rossr3-pivotal/asp-dot-net-core-2-sample)

	##### Get the source code repository app

	<img src="/images/dot-net-core-git.png" alt="Git style="width: 40%;"/>

	Download the source code. Download as Zip file and save it in local folder

	```bash
	   unzip asp-dot-net-core-2-sample-master.zip
	```
	##### ---OR---

	Fork and Clone

	[Dot Net Core Repo:  https://github.com/rossr3-pivotal/asp-dot-net-core-2-sample](https://github.com/rossr3-pivotal/asp-dot-net-core-2-sample)

	For Linux/Mac:
	```bash
	$git clone https://github.com/rossr3-pivotal/asp-dot-net-core-2-sample.git
	```

	For Windows
	```
	C:\<Some Directory to save code>\> git clone https://github.com/rossr3-pivotal/asp-dot-net-core-2-sample.git
	```


3. Environment Proxy Setup (Optional depending on network configuration)

	If you are connected to a network that requires using a proxy, you'll need to set the appropriate environment variables. In the repository there is a file called *setenv.bat*.

	Open up a terminal/command prompt, change directory to the top level folder and run the setenv.bat file.

	For Mac
	```
	export HTTP_PROXY=<your http proxy>
	export HTTPS_PROXY=<your https proxy>
	```

	For Windows
	```
	set HTTP_PROXY=<your http proxy>
	set HTTPS_PROXY=<your https proxy>
	```

Steps
--

In this workshop we are going to follow these steps to deploy .NET Framework and .NET Core applications on Cloud foundry.

***
## Deploying .NET Framework Applications

### Step 1
##### Build the Dot Net Environment Viewer Application
By this point, you should have cloned (or forked, or downloaded) the [DotNetAttendees Repo](https://github.com/Pivotal-Field-Engineering/pcf-dotnet-environment-viewer.git).  Now prepare the application to deploy to Cloud Foundry.

For this example, we do not need to "build" the application. For other .NET Framework applicaitons, you would publish the project to a specified folder and do the "cf push" from that folder.

Change into the correct folder. Notice that we are going down two levels to the ViewEnvironment Folder:

Linux/Mac:

  ```bash
  cd pcf-dotnet-environment-viewer/ViewEnvironment
  ```

Windows:

  ```
  cd pcf-dotnet-environment-viewer\ViewEnvironment
  ```

### Step 2
##### Login into Pivotal Cloud Foundry (if necessary)

Each participant will have their own user ids and passwords.  

  ```bash
  $ cf login -a https://api.run.azure.pcf.cloud --skip-ssl-validation
  Email: myuserid
  Password: ••••••••

  Select a space (or press enter to skip):
  1. development
  2. test
  3. production

  Select any one and stick to that space for the rest of the workshop.
  ```

Login to the App Console at https://apps.run.azure.pcf.cloud

<img src="/images/pcf-console.png" alt="PCF App Console" style="width: 70%;"/>


### Step 4
##### Push the app

Push the PCF DotNet Environment Viewer

  ```bash
  $ cf push <YOUR-INITIALS>-env
  // This will give an output which is similar to this
  requested state: started
  instances: 1/1
  usage: 512M x 1 instances
  urls: env-noninductive-nursling.apps.run.azure.pcf.cloud
  last uploaded: Thu Aug 31 19:41:33 UTC 2017
  stack: windows2016
  buildpack: hwc_buildpack
  ```

### Step 5
##### Open the Application in a Browser

When you push the apps, PCF will provide an URL to access the application

  <img src="/images/pcf-env-viewer.png" alt="PCF Environment Viewer" style="width: 70%;"/>

## Deploying .NET Core Applications

### Step 1
##### Build the Dot Net Core Demo Application
By this point, you should have cloned (or forked, or downloaded) the [Dot Net Core Repo:  https://github.com/rossr3-pivotal/asp-dot-net-core-2-sample](https://github.com/rossr3-pivotal/asp-dot-net-core-2-sample).  Let's run through the steps to prepare the applicatio to be deployed to Cloud Foundry:

    ```bash
    $ cd asp-dot-net-core-2-sample
    $ dotnet restore
    $ dotnet publish -f netcoreapp2.0 -r ubuntu.14.04-x64
    $ cf push <YOUR-INITIALS>-dotnetcore -p bin\Debug\netcoreapp2.0\ubuntu.14.04-x64\publish --random-route
    // This will give an output which is similar to this
    requested state: started
    instances: 1/1
    usage: 1G x 1 instances
    urls: dotnetcore-rootless-eeriness.apps.run.azure.pcf.cloud
    last uploaded: Sat Sep 2 17:46:35 UTC 2017
    stack: cflinuxfs2
    buildpack: ASP.NET Core (buildpack-1.0.25)
    ```

### Step 2
##### Open the Application in a Browser

When you push applications, it will provide you the URL to access it.

  <img src="/images/dot-net-core-app.png" alt="Dot Net Core Demo" style="width: 70%;"/>

##### Recap:

> Cloud Foundry Haiku </br>
  Here is my source code </br>
  Run it on the cloud for me </br>
  I do not care how</br>

##### Discussion

* How do you deploy an application today?
* How does your platform understand which runtime to use to run the app?
* How do or would you deploy a .NET Framework Application today?
* How do or would you deploy a .NET Core Application today?
***
