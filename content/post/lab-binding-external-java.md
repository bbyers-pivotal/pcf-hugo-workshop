+++

Categories = ["lab"]
Tags = ["external-services","microservices","cloudfoundry"]
date = "2017-08-29T07:49:11-04:00"
title = "Lab: Bind to an External Service"
weight = 5

+++

### Goal

Demonstrate how a microservice can use services that are not available within the Pivotal Cloud Foundry marketplace.

<!--more-->

Prerequisites
--

You have successfully completed the previous lab.

Steps
--

The `cities` directory also includes a `cities-ui` application which uses the `cities-client` to consume from the `cities-service`.

The `cities-client` demonstrates using the [Spring Cloud Connector](http://cloud.spring.io/spring-cloud-connectors) project to consume from a microservice.  This is a common pattern for Cloud Native apps.  For more details on building 12 Factor Apps for the Cloud (Cloud Foundry) refer to [12 Factor](http://12factor.net/) website.

The goal of this exercise is to use what you have learned to deploy the `cities-ui` application.

### Step 1
##### Build the Cities UI and Cities Client App


The cities-ui and cities-client can be both built at once by running `./gradlew assemble` in the parent directory. Run this command now if you did not run this in step # 2 above.


### Step 2
##### Create a User Provided Service Instance.

In this section we will create a backend microservice end point for cities-service.

1. Review the documentation on [User Provided Service Instances](http://docs.pivotal.io/pivotalcf/devguide/services/user-provided.html)
2. Look for the details by running `cf cups --help`.

3. You will need to specify the parameter citiesuri to the user defined service instance .

```bash
// Use the interactive prompt to create user defined service
// It will prompt you for the parameters

$ cf create-user-provided-service <YOUR INITIALS>-cities-ws -p "citiesuri"

citiesuri>   http://<YOUR INIITALS>-cities-service.app.cloud.rick-ross.com/

Creating user provided service....
```

<br>
### Step 3
##### Deploy cities-ui project


A `manifest.yml` is included in the cities-ui app.  Edit this manifest with your student number and add the service binding to your cities-service


```bash
$ cd cities-ui
$ nano manifest.yml (Or your favorite editor) (on Windows use wordpad manifest.yml)

---
applications:
- name: <YOUR INITIALS>-cities-ui
  memory: 512M
  instances: 1
  path: build/libs/cities-ui.jar
  services: [ <YOUR INITIALS>-cities-ws ]
  env:
    SPRING_PROFILES_ACTIVE: cloud
```

Push the `cities-ui` without specifying the manifest.yml. It will by default pick the manifest.yml file and deploy the app.

```bash
$ cf push
```

Note the URL once the application has been successfully pushed.

### Step 4
##### Verify the backend service is bound to cities-ui


```bash
$ cf env <YOUR INIITALS>-cities-ui

System-Provided:
{
 "VCAP_SERVICES": {
  "user-provided": [
   {
    "credentials": {
     "tag": "cities",
     "uri": "https://rj-cities-service.app.cloud.rick-ross.com/"
    },
    "label": "user-provided",
    "name": "cities-ws",
    "syslog_drain_url": "",
    "tags": []
   }
  ]
 }
}

{
 "VCAP_APPLICATION": {
  "application_name": "rj-cities-ui",
  "application_uris": [
   "rj-cities-ui.app.cloud.rick-ross.com"
  ],
  "application_version": "dceb111b-3a68-45ad-83fd-3b8b836ebbe7",
  "limits": {
   "disk": 1024,
   "fds": 16384,
   "mem": 512
  },
  "name": "rj-cities-ui",
  "space_id": "56e1d8ef-e87f-4b1c-930b-e7f46c00e483",
  "space_name": "development",
  "uris": [
   "rj-cities-ui.app.cloud.rick-ross.com"
  ],
  "users": null,
  "version": "dceb111b-3a68-45ad-83fd-3b8b836ebbe7"
 }
}

User-Provided:
SPRING_PROFILES_ACTIVE: cloud
```

### Step 5
##### Access the cities-ui to verify it is connected to your microservice.

Open the App Manager (Console) and navigate to your apps. You will see the cities-ui app, with a link to launch the cities-ui application. Alternatively you can open up your browser and navigate to the URL listed from a successful cf push command.


<img src="/images/cities-ui.png" alt="Cities UI" style="width: 100%;"/>

##### Discussion

In this part of the workshop we created a cities-ui app which is loosely bound and independently developed from the backend service. We bound that app to the cities-service microservice.

How do you design your applications today? Are they loosely coupled? Do they follow 12 Factor Application design principles?
