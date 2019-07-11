---
title: The Ultimate Day of Chicago Bikeshare
subtitle: Analysis of 17 million rides over 6 years
header_img: /assets/images/divvy-data/cover.png
tags:
- gis
- chicago
- data-visualization
---

Each day, Chicago's utilitarian design enables an incredible flow of people. Perhaps nothing bears this out better than the ridership of millions of bikes over several years. In this article, I analyze 17 million bikeshare rides and animate the most representative day of Chicago bikeshare. Skip ahead for the pretty pictures.

<h3 id="chicago-biking">Chicago Biking</h3>
Bicycles are the best way to see a city, and yes, I am including Chicago's sometimes brutal winter months. Even as temperatures approached <a href="https://darksky.net/details/41.8756,-87.6244/2019-1-30/us12/en">-50&#xB0;F last week</a>, our bikeshare still <a href="https://twitter.com/DivvyBikes/status/1091398529975836673">received ridership</a>.

Our blissfully flat terrain, lakefront paths, and bike infrastructure investments landed us Bicycling Magazine's <a href="https://www.bicycling.com/news/a20048181/the-50-best-bike-cities-of-2016/">most bike-friendly city in the country</a> (6th in <a href="https://www.bicycling.com/culture/a23676188/best-bike-cities-2018/">2018</a>). This past December, Chicago completed <a href="https://www.chicagoparkdistrict.com/parks-facilities/lakefront-trail">$12 million</a> and <a href="http://www.navypierflyover.com/">$60 million</a> projects to secure 18 continuous miles of dedicated lakefront bike path through the heart of Chicago.

<h3 id="data-sourcing">Data Sourcing</h3>
With mounting excitement for Chicago's cyclist future, I devoted my past week to a thorough shakedown of our bikeshare's <a href="https://www.divvybikes.com/system-data">public datasets</a>. You can find the code for this analysis on  <a href="https://github.com/chrisluedtke/divvy-data-analysis">GitHub</a>, including a <a href="https://nbviewer.jupyter.org/github/chrisluedtke/divvy-data-analysis/blob/master/notebook.ipynb">notebook</a> that steps through my process.

My first step was writing a <a href="https://github.com/chrisluedtke/divvy-data-analysis/tree/master/divvy">python package</a> to load the data neatly. Here's the result, pulling in ~2 GB of data:
<script src="https://gist.github.com/chrisluedtke/588da34440196b146c364f47fc9bda7e.js"></script>

Divvy data come in <code>zip</code> files grouped by quarter. Columns and date formats are not standardized across files, so I manually wrote them into the loading process.

Most challenging, the ride tables don't include geographic coordinates for ride origin and destination. That information is tied to stations, which are contained in their own files. Divvy also didn't provide station information at all for 2018, so I integrated data from their <a href="https://feeds.divvybikes.com/stations/stations.json">live JSON station feed</a> to get the most recent locations.

I also discovered stations had been physically moved while maintaining the same row-level ID. This greatly reduces the certainty of geographic analysis, as I can only be certain of a station's location at the end of the quarter on which the data were published. I calculated the geographic distance between these movements over time and dropped movements below a ~50 meter <a href="https://en.wikipedia.org/wiki/Decimal_degrees#Precision">precision level</a>.

A representative station:

<table border="0", style="font-family: monospace;width: 100%;text-align: right;border-collapse: collapse">
<thead style="border-bottom: 1px solid black">
    <tr>
    <th>id</th>
    <th>latitude</th>
    <th>longitude</th>
    <th>online_date</th>
    <th>source</th>
    </tr>
</thead>
<tbody style="border-bottom: 1px solid black">
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

<iframe src="/assets/images/divvy-data/moved_stations.html" width="100%" height="405" scrolling="no" frameborder="0"></iframe>

Ultimately I decided to average the remaining station coordinates. I didn't suspect the difference would make a meaningful impact on my analysis.

<h4 id="descriptives">Descriptives</h4>
With cleaned data, the beauty of the information could finally shine. There really are countless, never-ending lines of inquiry one may follow in this data set, and I suspect this won't be my final analysis.
Since June 2013 there have been <strong>17.5 million trips</strong>, totaling <strong>5.3 million</strong> hours, and covering between <strong>21.7</strong> (haversine) and <strong>27.1</strong> (taxi cab) <strong>million miles</strong>. That's 56 trips to the moon and back!

The typical ride covered <strong>1.0</strong> to <strong>1.2 miles</strong> in <strong>11.7 minutes</strong>. The longest ride covered <strong>22.9</strong> to <strong>26.2 miles</strong> in <strong>2.9 hours</strong>.

The typical bike covered <strong>4.3 thousand miles</strong> over <strong>2.8 thousand rides</strong>, lasting a total of <strong>820 hours</strong>.

The farthest ridden bike covered between <strong>6.5</strong> and <strong>8.1 thousand miles</strong>. Here's all the routes it covered.

<iframe src="/assets/images/divvy-data/longest_ridden.html" width="100%" height="405" scrolling="no" frameborder="0"></iframe>

<h4 id="time-series-analysis">Time Series Analysis</h4>

Ultimately I set out to characterize and visualize Chicago's bikeshare usage. Due to Chicago's layout, I suspected this line of analysis would return fascinating results.

To investigate, I took all 2,017 days of data, cut them into 30 minute segments, and calculated statistics for each station within those timeframes across all days. In particular, I was interested in each station's net arrivals and departures. For each station, I added those numbers and divided by the number of days the station had been active.

This gave me 48 'snapshots' of Chicago bikeshare activity, each representing a separate typical 30 minute period of the day. To animate this data more smoothly, I interpolated up to 720 frames to give 'snapshots' representing 2 minutes of the day.

Here's the result. Best viewed at 4K resolution, and right-click to loop. Each dot represents a bike station. Circle size corresponds to station usage. <strong><font color="#f06e18">Orange</font></strong> stations are radiating bikes. <strong><font color="#18f0da">Blue</font></strong> stations are collecting bikes. Gray stations are neutral.

<iframe width="100%" height="405" src="https://www.youtube.com/embed/SVueGQPpz14?modestbranding=1&loop=1&rel=0" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Finally, here are two interactive frames from the animation. Click or tap the circles for more detail. Let me know what you discover. (<a href="https://chrisluedtke.github.io/img/divvy-data/am_v_pm.html">full screen here</a>)

<table style="width:100%; text-align:center">
<tr>
    <th>8:00am - 8:30am</th>
    <th>5:00pm - 5:30pm</th>
</tr>
</table>

<iframe src="/assets/images/divvy-data/am_v_pm.html" width="100%" height="500" scrolling="no" frameborder="0"></iframe>
