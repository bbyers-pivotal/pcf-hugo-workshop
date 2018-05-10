+++

Categories = ["lab"]
Tags = ["dotnet","microservices","cloudfoundry","steeltoe"]
date = "2017-08-29T07:49:11-04:00"
title = "Lab: Steeltoe Config Server"
weight = 70

+++

### Goal

Learn how to write applications that retreive their configuration from Spring Cloud Config Server  

<!--more-->

Prerequisites
--

1. Install .NET Core 2.0

    [.NET Core](https://www.microsoft.com/net/core)

2. Clone or Download the Steeltoe Samples

	[Steeltoe Samples:  https://github.com/rossr3-pivotal/Samples](https://github.com/rossr3-pivotal/Samples)

	##### Get the source code repository app

	<img src="/images/steeltoe-samples-git.png" alt="Git style="width: 40%;"/>

	Download the source code. Download as Zip file and save it in local folder

	```bash
	unzip Samples-master.zip
	```
	##### ---OR---

	Fork and Clone

	[Steeltoe Samples:  https://github.com/rossr3-pivotal/Samples](https://github.com/rossr3-pivotal/Samples)

	For Linux/Mac:
	```bash
	$ git clone https://github.com/rossr3-pivotal/Samples.git
	```

	For Windows
	```
	C:\<Some Directory to save code>\> git clone https://github.com/rossr3-pivotal/Samples.git
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

In this workshop we are going to follow these steps to deploy a .NET Core application that communicates to the Configuration Server to retrieve its configuration.

***
## Deploying the Configuration Sample Application

### Step 1
##### Build the Configuration Sample Application

By this point, you should have cloned (or forked, or downloaded) the [Steeltoe Samples:  https://github.com/rossr3-pivotal/Samples](https://github.com/rossr3-pivotal/Samples).  Let's prepare the application to deploy to Cloud Foundry.

Change into the correct folder, which is Configuration/src/AspDotNetCore/SimpleCloudFoundry Notice that we are going down several levels to the SimpleCloudFoundry Folder:

Linux/Mac:

   ```bash
   cd Configuration/src/AspDotNetCore/SimpleCloudFoundry
   ```

Windows:

   ```
   cd Configuration\src\AspDotNetCore\SimpleCloudFoundry
   ```

### Step 2
##### Login into Pivotal Cloud Foundry (if necessary)

Each participant will have their own user ids and passwords.  

  ````
  cf login -a https://api.sys.cloud.rick-ross.com --skip-ssl-validation
  Email: myuserid
  Password: ••••••••

  Select a space (or press enter to skip):
  1. development
  2. test
  3. production

  Select any one and stick to that space for the rest of the workshop.

   ````

Login to the App Console at https://app.cloud.rick-ross.com

   <img src="/images/pcf-console.png" alt="PCF App Console" style="width: 70%;"/>

### Step 3
##### Create a Config Server within Pivotal Cloud Foundry

Let's create a config server that the application will use to get configuration information from:

   ```
   cf create-service p-config-server standard <YOUR INITIALS>-ConfigServer -c ./config-server.json
   ```

Note that this step might take a bit to complete. Please wait until you see that the last operation resulted is "create succeeded". To do that:

   ```bash
   cf services
   Getting services in org pivotal / space development as admin...
   OK

   name              service           plan       bound apps   last operation
   rr-ConfigServer   p-config-server   standard                create in progress
   ```

Once this is successful, you will see this:

   ```bash
   cf services
   Getting services in org pivotal / space development as admin...
   OK

   name              service           plan       bound apps   last operation
   rr-ConfigServer   p-config-server   standard                create succeeded
   ```

You can now proced to the next step.

### Step 4
#### Prepare the .NET Core application for deployment

   ```bash
   dotnet restore --configfile nuget.config
   dotnet publish -f netcoreapp2.0 -r ubuntu.14.04-x64
   ```

### Step 5
##### Modify the Manifest to Match the Name of Your Service

In a text editor, open up the manifest.yml file and change the <YOUR INITIALS> placeholder with your initials.

   ```
   ---
   applications:
   - name: foo
     env:
       ASPNETCORE_ENVIRONMENT: development
     services:
      - <YOUR INITIALS>-ConfigServer
   ```

Be sure to save the file before continuing.

### Step 6
##### Push the app

Push the Fortune Teller Service

On Linux/Mac:

  ```bash
  $ cf push <YOUR INITIALS>-fortuneservice -f manifest.yml -p bin/Debug/netcoreapp2.0/ubuntu.14.04-x64/publish
  ```

 On Windows:

  ```bash
  cf push <YOUR INITIALS>-fortuneservice -f manifest.yml -p bin\Debug\netcoreapp2.0\ubuntu.14.04-x64\publish
  ```

Which will result in output of

   ```bash
   // This will give an output which is similar to this
   requested state: started
   instances: 1/1
   usage: 1G x 1 instances
   urls: rr-configapp.app.cloud.rick-ross.com
   last uploaded: Sun Sep 3 21:16:55 UTC 2017
   stack: cflinuxfs2
   buildpack: ASP.NET Core (buildpack-1.0.25)
   ```

### Step 7
##### View the application in a Browser

Navigate the URL of the application in a browser. Then click on the CloudFoundry Settings menu / tab to view information from VCAP_APPLICATION and VCAP_SERVICES that are associated with this application running in Cloud Foundry.

Here is the type of detail you see for the CloudFoundry Settings. Notice that you can see the instance ID and other information about this aplication.

   <img src="/images/cloud-foundry-settings.png" alt="CloudFoundry Settings" style="width: 70%;"/>

Here is the type of detail you see for the Config Server Settings.  

   <img src="/images/config-server-settings.png" alt="Config Server Settings" style="width: 70%;"/>

And these are the details from the Config Server repository.

   <img src="/images/config-server-data.png" alt="Config Server Data" style="width: 70%"/>

To see what values it is pulling from files in the git repository, open a browser and navigate to the backing repo which is located here: [https://github.com/rossr3-pivotal/config-repo](https://github.com/rossr3-pivotal/config-repo). Look for properties files that have "foo" in their name.

For additional information on Steeltoe Configuration please see the official documentation.  [http://steeltoe.io/docs/steeltoe-configuration/](http://steeltoe.io/docs/steeltoe-configuration/)

And if you have time, go through the same exercise using the .NET Framework which is located in the Configuration/src/AspDotNet4/SimpleCloudFoundry folder. Remember to modify the manifest file to use your configuration service instance name.
