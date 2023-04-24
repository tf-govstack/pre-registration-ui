[![Maven Package upon a push](https://github.com/mosip/pre-registration-ui/actions/workflows/push_trigger.yml/badge.svg?branch=release-1.2.0.1)](https://github.com/mosip/pre-registration-ui/actions/workflows/push_trigger.yml)
[![Quality Gate Status](https://sonarcloud.io/api/project_badges/measure?branch=release-1.2.0.1&project=mosip_pre-registration-ui&metric=alert_status)](https://sonarcloud.io/dashboard?branch=release-1.2.0.1&id=mosip_pre-registration-ui)

## Pre-Registration Portal

This repository contains UI code for Pre-Registration portal. Refer [here](https://docs.mosip.io/1.2.0/modules/pre-registration/pre-registration-user-guide) for details about the functions that a resident can perform via this portal.

## Build &amp; Deployment Guide

The code is written in Angular JS. 
  
- Install node.js – To build the angular code using angular cli that runs on node. 
  We recommend Node: 14.17.3, Package Manager: npm 6.14.13
  
- Install angular cli – To install angular cli for building the code into deployable artifacts. Follow the following steps to install angular cli on your system.
  - <code>npm install -g @angular/cli</code> (to install angular cli)
  - <code>ng --version</code> (to verify angular is installed in system)
  We recommend Angular CLI: 13.3.2
  
- Check out the source code from GIT – To download the source code from git. Follow the following steps to download source code on your system.
  - <code>git clone https://github.com/mosip/pre-registration-ui.git</code> (to clone the source code repository from git)

## For Production build:

- Build the code – Follow the following steps to build the source code on your system.
  - Navigate to the pre-registration-ui directory inside the cloned repository. Then run the following command in that directory
  - <code>ng build "--prod" "--base-href" "." "--output-path=dist" </code>(to build the code)

- Build Docker Image – Follow the following steps to build docker image on your system.
  - <code>docker build -t name . </code>(to build the docker image, replace <code>name</code> with the name of the image you want, &quot;.&quot; Signifies the current directory from where the docker file has to be read.
  - Example: <code>docker build -t preregui .</code>
- Run the docker image – Follow the following steps to build docker image on your system.
  - <code>docker run –d –p 80:80 --name container-name image-name</code> (to run the docker image created with the previous step,-d signifies to run the container in detached mode, -p signifies the port mapping left side of the&quot;:&quot; is the external port that will be exposed to the outside world and right side is the internal port of the container that is mapped with the external port. Replace <code>container-name</code> with the name of your choice for the container, replace <code>image-name</code> with the name of the image specified in .the previous step)
  - Example: <code>docker run -d -p 8080:8080 --name nginx preregui</code>
- Now you can access the user interface over the internet via browser.
  - Example: <code>http://localhost:8080/pre-registration-ui/#/eng</code>
  
### For Local build:
- Build & deploy the code locally – Follow the following steps to build the source code on your system.
  - Navigate to the pre-registration-ui directory inside the cloned repository. Then run the following command in that directory
  - <code>ng serve</code>
- Now you can access the user interface via browser.
  - Example: <code>http://localhost:4200</code>
- But this will give you CORS issue in accessing backend prereg services. To get around the CORS issue, angular CLI proxy can be used.
  - Update the API services BASE_URL in config.json
  ```
  {  
	"BASE_URL": "https://localhost:4200/proxyapi/",    
	"PRE_REG_URL": "preregistration/v1/"  
	}
  ```
  - Create a new file named proxy.conf.json. Replace ${servicesUrl} with correct url.
  ```
  {
    "/proxyapi": {
      "target": ${servicesUrl},
      "secure": true,
      "changeOrigin": true,
      "pathRewrite": {
        "^/proxyapi": ""
      }
    }    
  }
  ```
  - Now start the server by typing <code>ng serve --proxy-config proxy.conf.json --ssl true</code>
  - Open the browser, load the app with <code>https://localhost:4200</code>
