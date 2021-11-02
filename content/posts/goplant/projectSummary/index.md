---
title: "Goplant Project Description"
date: 2021-10-29
description: Summary of Project
menu:
  sidebar:
    name: Goplant Project Description 
    identifier: projectDescription 
    parent: goplant 
    weight: 40
hero: finishedPlantPotOnWindowCloseUpBanner.jpeg
---

## What is Goplant?
---

Goplant is a wireless plant monitoring system that utilizes a few technologies I'm interesting in learning!

## What does Goplant do?
---

Goplant operates by programming Microcontrollers to collect sensor measurements (insert image of microcontroller) and send that data to the Goplant application that is in charge of storing that data in a database. Connecting the Microcontrollers to the internet, in order to reach the Goplant App is super easy thanks to the builtin wifi module (insert picture of wifi logo on micro controller). Both the Goplant App and the Database the cloud making it easy to connect to from anywhere! The data stored in the database can then be retrieved and plotted on graphs in order to gain insights on whats going on with your plants! Goplant supports monitoring light intensity, soil moisture, and soil temperature.  

Inspiration for this project came from [this really cool video about creating a auto-watering system with an arduino](https://www.youtube.com/watch?v=O_Q1WKCtWiA) and I wanted to try and make it myself, with the added twist of it being wireless. Less wires is always better!

## How does Goplant Work?
---

Goplant has two main parts, one being the API and the other being the parts that make requests to the API.

##### API

Goplant's API, written in Golang, supports storing data for one of each sensor type in order to monitor a single garden. Each endpoint is tied to a single table in a Postgres Database. The API functionality is tested via integration tests and Postman. The idea was to keep the database schema as simple as possible for the MVP and build off it later. 

##### Devopsy Features

The API is Dockerized and setup to run on a Kubernetes cluster. Setting up the API and Database on a cluster takes only a few seconds thanks to Goplant's Helm chart ([More info about the helm chart can be found here]((Insert link))). There is also a CICD pipeline that is setup to spin up a database and then run the integration tests against it for each commit. User is unable to merge a pull request until pipeline job passes.


### Data Gathering Components
---

##### Analog Sensor Data

The data uploaded to the database is the standard output of an analog port for any Microcontroller, an integer between 0-1023. The Microcontroller only supports a single 5V power out port. You can quickly calculate how many volts the output represents by multiplying the number by 0.0049 volts, (5/1024 = 0.0049). The benefit to uploading this value rather than try to convert it either via the Microcontroller or the API always remains in the same format in the database. Meaning I can take that data and convert it as I see fit when retrieving the data via the database.

##### Scripts for Microcontrollers

Since all Microcontrollers currently upload only the analog port output, only a single script is needed to be maintained. In order to setup each Microcontroller, all that is needed is to set the api endpoint that corresponds to the sensor type used in the script.

### Components Used
---

- Microcontroller
    - [ESP82866](https://www.amazon.com/dp/B081PX9YFV/ref=cm_sw_em_r_mt_dp_0N9PSTN298NKR1MDQD5S)
- Sensors 
    - Analog Sensors
        - [Light](https://www.amazon.com/dp/B07PF3CWW9/ref=cm_sw_em_r_mt_dp_HDKP41RBAWH86XTXAZJ4)
        - [Soil Moisture](https://vegetronix.com/Products/VH400/)
        - [Soil Temperature](https://vegetronix.com/Products/THERM200/)
    - Digital Sensors
        - (TODO) [Air Temperature + Humidity](https://www.amazon.com/dp/B01DKC2GQ0/ref=cm_sw_em_r_mt_dp_0YGAYCXRM0Y0ECR4HZV0)