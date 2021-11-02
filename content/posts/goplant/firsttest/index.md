---
title: "Goplant's First Test"
date: 2021-10-29
description: Summary of GoPlant's first test 
menu:
  sidebar:
    name: Goplant's First Test
    identifier: firsttest 
    parent: goplant 
    weight: 40
hero: finishedPlantPotOnWindowCloseUpBanner.jpeg
---

With the Microcontrollers successfully making requests to the GoPlant API, it's finally time to plug in all sensors and try collecting some data! It just so happened that the week I wanted to try this also happened to be the week it was forecasted to rain every day and my Microcontrollers are far from water proof. Therefore, I grabbed the duct tape, one of my indoor plants, and got to work.

# Setting up Goplant

### Setting up the API
---

API setup was pretty straight forward thanks to the helm chart that automates setting up the database and API on a Kubernetes Cluster. The Cluster itself was also extremely easy to setup thanks to Docker Desktop.

### Setting up the Plant Pot
---

The plan was to setup a all-in-one "system"

{{< img src="/posts/goplant/firsttest/finishedPlantPotOnWindow.jpeg" float="right" height="700" width="600"  title="Finished Plant Pot" >}} 

##### Soil Sensors

Both soil sensors were purchased from Vegetronix. They are super easy to setup because it only has three wires, power, ground, and out, and you plug these directly into the Microcontroller.

{{< img src="/posts/goplant/firsttest/soil_sensors.jpeg" height="250" width="300" title="Soil Sensors" >}}

##### Light Intensity Sensor

The light sensor was slightly more complicated to setup because it uses a Photoresistor that can't handle 5 volts, which is the only port the Microcontroller provides. I connected it to a 220 Ohm resistor to avoid blowing it up and it worked like a charm.

Trying to tape the bread board circuit to the plant pot turned out to be more trouble than it was worth. Instead I grabbed some PCB prototype board and soldered the components together into one unit. This was my first time converting a breadboard circuit to PCB and the final product did work the *first time* ! I felt as if I passed some right of passage as a electrical hobbyist. 

{{< img src="/posts/goplant/firsttest/bread_board_photoresistor_circuit.jpeg" height="200" width="300" float="left" title=" Bread board photoresistor circuit" >}}

{{< img src="/posts/goplant/firsttest/pcbPhotoresistor.jpeg" height="200" width="300" float="left" title="Bread board photoresistor circuit" >}}

{{< vs 15 >}}

### Firmware Updates
---

##### Development

I started with three separate arduino scripts for each sensor. The difference between each script being the logic to convert the analog reading into a unit or percentage. As it turns out, the output for the analog output is converted to an integer between 0-1023. I decided to make things easy for myself and store this value in the Database, saving the conversion logic for the script that takes the data and plots it on a graph. For example, light to lumens, soil temperature to Celsius and soil humidity to a percentage.

{{< img src="/posts/goplant/firsttest/microController.jpeg" height="200" width="200" float="right" title="Microcontroller" >}}

##### Over The Air (OTA) Updates
One of the most exciting features I found for these Microcontrollers (Using Arduinos OTA example sketch) is Over The Air updates. Every time I setup a new cluster, either locally on my computer or on a cloud service, the ips and ports change. As more Microcontrollers are added to the system, it can get pretty time consuming to, plug in Microcontroller, flash firmware, plug in next controller, rinse and repeat, only to realize you didn't update some value and have to do it again. Using OTA, as long as the Microcontrollers are plugged in, all I have to do is select the port and upload the new firmware *wirellessly*! The first time this worked I woke my roommates up with a thunderous "YESSSSS!!!!!!!"

#### Known Issues with Test

I knew that there were going to be a few issues with this test that would make the data gathered unreliable. Air conditioning affects soil temperature, light coming through a window on the side of the house affects light intensity, a plant pot with a hole in the bottom for irrigation affects soil moisture. I'll have to rerun this test and see the difference between the data gathered indoors vs outdoors.

### Test Results
--- 

For over twenty four hours, the Goplant system was able to gather and store data from all sensors. This is a huge milestone for the project as the MVP I set was only to gather data wirelessly then plot that data on a cool graph. In the next post I'll be converting the data gathered and make some cool graphs! And I really like cool graphs!