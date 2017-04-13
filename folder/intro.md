###Introduction
-----

The topic for visualization spring 2017 at IIT Institute of Design is spatial 'real-time' data. We’re looking for interesting data about a place, where the characteristics of that location are recorded and updated at regular intervals over time. An example would be data gathered through Chicago’s Array of Things that tells you about the nature of a location—e.g. sound, air quality, light at an intersection that’s been outfitted with a sensor node.

Before we begin visualizing data, it is important that we find published sources for our realtime data, which are called **datasets**. A dataset is a collection of data that contains observed elements (called keys or members) and information about those elements (called values, data points, or parameters). You are probably most familiar with Excel's presentation of datasets which represent keys as rows, and values as columns. Datasets, however, come in many other forms -- comma-separated-value files, HTML tables, JSON arrays, Javascript API's, network matrices, RSS feeds, and many other exotic formats.

The requirements for the datasets are:

**Location-based data**: data that describes a particular geographic place. The data must be plottable on a globe or map of some kind.

**Multiple datapoints per location**: the dataset includes multiple datapoints that describes each location. Though we will not necessarily use them all, at least 3 datapoints (in Excel language, at least 3 columns per row of data) are required of datasets we work with. You can freely combine datasets to produce the necessary 3 data points -- for instance, we can merge a routinely updated CSV dataset that has live population counts for the 100 biggest cities, an API that has temperature data for those same cities, and a third website that tracks the cities' air quality.

**Updated/refreshed at intervals**: the information is updated periodically to show change over time in that location. The refresh frequency would depend on the context in which the information is gathered, but at minimum, we request at least daily updates be available.
