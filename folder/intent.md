###Intent Statement
---
The topic for visualization spring 2017 at IIT Institute of Design is spatial 'real-time' data. We’re looking for interesting data about a place, where the characteristics of that location are recorded and updated at regular intervals over time. An example would be data gathered through Chicago’s Array of Things that tells you about the nature of a location—e.g. sound, air quality, light at an intersection that’s been outfitted with a sensor node.

Before we begin visualizing data, it is important that we find published sources for our realtime data, which are called **datasets**. A dataset is a collection of data that contains observed elements (called keys or members) and information about those elements (called values, data points, or parameters). You are probably most familiar with Excel's presentation of datasets which represent keys as rows, and values as columns. Datasets, however, come in many other forms -- comma-separated-value files, HTML tables, JSON arrays, Javascript API's, network matrices, RSS feeds, and many other exotic formats.

The requirements for the datasets are:

**Location-based data**: data that describes a particular geographic place. The data must be plottable on a globe or map of some kind.

**Multiple datapoints per location**: the dataset includes multiple datapoints that describes each location. Though we will not necessarily use them all, at least 3 datapoints (in Excel language, at least 3 columns per row of data) are required of datasets we work with. You can freely combine datasets to produce the necessary 3 data points -- for instance, we can merge a routinely updated CSV dataset that has live population counts for the 100 biggest cities, an API that has temperature data for those same cities, and a third website that tracks the cities' air quality.

**Updated/refreshed at intervals**: the information is updated periodically to show change over time in that location. The refresh frequency would depend on the context in which the information is gathered, but at minimum, we request at least daily updates be available.

Here are some places to start looking. From our experiences, though, the more personal and unique your choice of dataset, the more persuasive your visualizations will be. Explore what's out there, we live in the era of Big Data, there's so much available!

Data Search Engines and General Directories

https://www.data.gov
http://publicdata.eu
http://www.data.go.jp/?lang=english
http://www.zanran.com/q/
https://www.reddit.com/r/datasets/   (and check out the linked subreddits too!)
https://www.exversion.com/search/
http://www.k12science.org/materials/resources/realtimedata/
https://github.com/toddmotto/public-apis
https://webhose.io

Municipal

https://data.cityofchicago.org
https://nycopendata.socrata.com/
https://opendata.paris.fr/page/home/

Atmospheric and Geologic

http://www.ndbc.noaa.gov
https://www.ncdc.noaa.gov/cdo-web/
http://datacasting.jpl.nasa.gov/feed_directory/
http://weather.rap.ucar.edu
http://earthquake.usgs.gov/earthquakes/feed/v1.0/
http://www.ssec.wisc.edu/data/
https://cfpub.epa.gov/surf/locate/index.cfm
http://volcano.ssec.wisc.edu
https://www.opensensors.io

Space

https://data.nasa.gov
http://chandra.harvard.edu
http://sid.stanford.edu/database-browser/

Transportation

http://opentraffic.io
http://www.marinetraffic.com
https://www.radarbox24.com

Social

https://wikitech.wikimedia.org/wiki/RCStream
https://developers.facebook.com/docs/graph-api
https://www.google.com/trends/explore
https://dev.twitter.com/streaming/overview

Financial

https://www.quandl.com
http://data.worldbank.org
https://www.google.com/finance

Animals and Plants

https://beeinformed.org
https://www.movebank.org
http://whale.wheelock.edu/whalenet-stuff/StopBm2016/
https://www.liverpool.ac.uk/savsnet/real-time-data/1


**The topic I want to focus is ocean fishery and ocean health.**

The existing dataset online is messy and I want to sort the vessel information by country, so viewers can focus on countries they are interested in instead of struggling with making sense of the whole dataset. Also I want audiences to explore the correlation between fishery and ocean health, such as endanger species or overfishing. I want to encourage audiences to pay more attention to the topic and supervise illegal fishing activities.

To accomplish this goal, I will first sort the vessel activity information online by countries. Then I will add another layer of data (endangered species, main edible fish distribution, restricted use areas) to the vessel activity data, so that audiences can find some trends and hopefully causality to make them more informed.
