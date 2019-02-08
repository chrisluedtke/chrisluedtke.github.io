Each day, Chicago's utilitarian design enables an incredible flow of people. Perhaps nothing bears this out better than the ridership of millions of bikes over several years. In this article, I analyze 17 million bikeshare rides and animate the most representative day of Chicago bikeshare. Skip ahead for the pretty pictures.

### Chicago Biking

Bicycles are the best way to see a city, and yes, I am including Chicago's sometimes brutal winter months. Even as temperatures approached [-50&deg;F last week](https://darksky.net/details/41.8756,-87.6244/2019-1-30/us12/en), our bikeshare [received ridership](https://twitter.com/DivvyBikes/status/1091398529975836673).

Our blissfully flat terrain, lakefront paths, and bike infrastructure investments landed us Bicycling Magazine's [most bike-friendly city in the country](https://www.bicycling.com/news/a20048181/the-50-best-bike-cities-of-2016/) (6th in [2018](https://www.bicycling.com/culture/a23676188/best-bike-cities-2018/)). This past December, Chicago completed [$12 million](https://www.chicagoparkdistrict.com/parks-facilities/lakefront-trail) and [$60 million](http://www.navypierflyover.com/) projects to secure 18 continuous miles of dedicated lakefront bike path through the heart of Chicago.

### Data Sourcing

With mounting excitement for Chicago's cyclist future, I devoted my past week to a thorough shakedown of our bikeshare's [public datasets](https://www.divvybikes.com/system-data). You can find the code for this analysis on  [GitHub](https://github.com/chrisluedtke/divvy-data-analysis), including a [notebook](https://nbviewer.jupyter.org/github/chrisluedtke/divvy-data-analysis/blob/master/notebook.ipynb) that steps through my process.

My first step was writing a [python package](https://github.com/chrisluedtke/divvy-data-analysis/tree/master/divvy) to load the data neatly. Here's the result, pulling in ~2 GB of data:
``` python
import pandas as pd

import divvy

rides, stations = divvy.historical_data.get_data(
    year=[str(_) for _ in range(2013,2019)],
    rides=True,
    stations=True
)

rides.to_pickle('data/rides.pkl')
stations.to_pickle('data/stations.pkl')
```

Divvy data come in `zip` files grouped by quarter. Columns and date formats are not standardized across files, so I manually wrote them into the loading process.

Most challenging, the ride tables don't include geographic coordinates for ride origin and destination. That information is tied to stations, which are contained in their own files. Divvy also didn't provide station information at all for 2018, so I integrated data from their [live JSON station feed](https://feeds.divvybikes.com/stations/stations.json) to get the most recent locations.

I also discovered stations had been physically moved while maintaining the same row-level ID. This greatly reduces the certainty of geographic analysis, as I can only be certain of a station's location at the end of the quarter on which the data were published. I calculated the geographic distance between these movements over time and dropped movements below a ~50 meter [precision level](https://en.wikipedia.org/wiki/Decimal_degrees#Precision).

A representative station:

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>id</th>
      <th>latitude</th>
      <th>longitude</th>
      <th>online_date</th>
      <th>source</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>2</td>
      <td>41.872293</td>
      <td>-87.624091</td>
      <td>NaT</td>
      <td>2015</td>
    </tr>
    <tr>
      <td>2</td>
      <td>41.881060</td>
      <td>-87.619486</td>
      <td>2013-06-10 10:43:46</td>
      <td>2017_Q1Q2</td>
    </tr>
    <tr>
      <td>2</td>
      <td>41.876393</td>
      <td>-87.620328</td>
      <td>2013-06-10 10:43:00</td>
      <td>2017_Q3Q4</td>
    </tr>
  </tbody>
</table>

All duplicate stations and their movements:

**map of moved stations**

Ultimately I decided to average the remaining station coordinates. I didn't suspect the difference would make a meaningful impact on my analysis.

#### Descriptives
With cleaned data, the beauty of the information could finally shine. There really are countless, never-ending lines of inquiry one may follow in this data set, and I suspect this won't be my final analysis.

For starters, since June 2013 there have been **17.5 million trips**, totaling **5.3 million** hours, and covering between **21.7** (haversine) and **27.1** (taxi cab) **million miles**. That's 56 trips to the moon and back!

The typical ride lasted **11.7 minutes** and covered between **1.0** and **1.2 miles**. The longest ride covered between **22.9** and **26.2 miles** in **2.9 hours**.

The typical bike was ridden **2.8 thousand** times for **820 hours**.

The longest ridden bike covered between **6.5** and **8.1 thousand miles**. Here's a map of all its routes.

<iframe src="divvy-data/moved_stations.html" width="100%" height="200" scrolling="no" frameBorder="0"></iframe>

#### Time Series Analysis

Ultimately I set out to characterize and visualize Chicago's bikeshare usage. Due to Chicago's layout, I suspected this line of analysis would return fascinating results.

To investigate, I took all 2,017 days of data, cut them into 30 minute segments, and calculated statistics for each station within those timeframes across all days. In particular, I was interested in each station's net arrivals and departures. For each station, I added those numbers and divided by the number of days the station had been active.

This gave me 48 'snapshots' of Chicago bikeshare activity, each representing a separate typical 30 minute period of the day. To animate this data more smoothly, I interpolated up to 720 frames to give 'snapshots' representing 2 minutes of the day.

Here's the result.

<iframe width="560" height="315" src="https://www.youtube.com/embed/SVueGQPpz14&autoplay=1&loop=1&rel=0" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Winter vs Summer plot

split plot from here: https://nbviewer.jupyter.org/github/python-visualization/folium/blob/master/examples/Plugins.ipynb
