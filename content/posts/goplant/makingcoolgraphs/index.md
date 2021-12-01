---
title: "Making Cool Graphs"
date: 2021-12-01
description: Using data collected to make graphs 
menu:
  sidebar:
    name: Making Cool Graphs
    identifier: makingcoolgraphs
    parent: goplant 
    weight: 40
hero: v1Graphs/lightThumbnail.png
---

## Plotting Data

##### Note on the data
The data being plotted was from a twenty four hour test that I setup in the previous post. It turned out to be a 72 hour test because I left it running by accident. This test was setup inside during a particularly cloudy week, data results may vary. The extra data turned out to be a huge help when looking at the correlations of the final lines.

#### Creating the first script
I started with a super simple script that generated three different graphs, one with sensor type. In order to plot each data frame, I had to do convert the output from the microcrontroller analog port to a percentage (the analog port outputs a number between 0-1023) and convert the date to something pyplot can recognize.

  ```python3

    #Read data
    df = pd.read_csv('./data/<FileName>.csv')
    #Convert value column to percentage
    df['value'] = df['value'].apply(convertValueToPercentage)
    #Convert record time into something pyplot can read
    df['recordtime'] = pd.to_datetime(df['recordtime'])
    #Plot it!
    df.plot(x="recordtime", y='value', label="<LineName>")

  ```

#### Plotting all lines on one graph
Having three separate graphs isn't very intuitive for comparing all the different lines at once. I want to super impose all three lines. This will make it super easy to see any correlations between the lines. The first step to doing this is plot all three lines on the same graph. To do this I used the 'ax' parameter. 

  ```python3

    graph = soilMoisture.plot(x="recordtime", y='value', label="Soil Moisture")
    graph = soilTemp.plot(ax=graph, x="recordtime",y='value', label="Soil Temperature")
    graph = light.plot(*ax=graph*, x="recordtime", y='value', label="Light")

  ```

{{< img src="/posts/goplant/makingcoolgraphs/v2Graphs/threeLineGraph.png" height="600" width="800" title="Three Line Graph" >}}

#### Superimposing each line
Next, to superimpose each line, I zeroed each line by subtracting the first value in the first row from every value in the data set. This brings each line down to 0.

```python3
soilTLineShiftAmount = soilTemp.iloc[0].value
soilMLineShiftAmount = soilMoisture.iloc[0].value
lightShiftAmount = light.iloc[0].value

# Correct each line so that they overlap
soilMoisture['value'] = soilMoisture['value'].apply(shiftSoilMoistureLine)
soilTemp['value'] = soilTemp['value'].apply(shiftSoilTempLine)
light['value'] = light['value'].apply(shiftLightLine)
```

#### Final Product
{{< img src="/posts/goplant/makingcoolgraphs/v2Graphs/finalGraph.png" height="600" width="800" title="Final Graph" >}}

With all the lines super imposed on each other, you clearly see correlations between them. The final graph turned out way better than I expected it to!

## Conclusions on Data

#### Light and Soil Temperature
Its worth noting that this data was taken inside during a very cloudy week. I say this because, going in, I expected the data to not make much sense. I was testing if the system would work for an extended period of time without supervision. Surprisingly, in spite of this, the soil temperature follows the day/night cycle enough to see it in the graph! The soil temperature should mirror the light intensity with a significant delay and it does this in the graph. While not as clean cut as I would like, still its exciting to see this correlation in the first attempt at collecting data.

#### Soil Moisture

This one i'm not sure what happened. I know its working because at the start of the test I watered the plant, in order to test the sensor. The pot's runoff got clogged somehow and the water just sat there for days. I hadn't expected it to go up, almost steadily, for days. It could be that the water was absorbed by the soil evenly over time, or the sensor isn't working properly. To start from 20% and go up by another 15% over the course of two days is worth looking into.

## Next Steps

- I have some sensors in an arduino kit I bought awhile back for measuring air temperature. It a digital sensor so I can throw it on any of the microcontrollers. Then its a simple matter of updating the firmware to make the requests along side the analog sensor POST requests.

- To reach the MVP with this project, I need to setup the sensors outside for a full week. This is going to be a challenge because I need to setup goplant on the cloud for 24/7 access to the api and somehow make my sensors waterproof. 

