# Business Problem

Suppose you have a client that is looking to open a new bakery in Toronto, Canada. Choosing a location is very impotant for a number of reasons:

- Ideally we want low number of competing bakeries.
- People are more likely to visit a bakery if it is close to other shops, so how many cafe's, restaurants, foot traffic is in the area. Ideally we want more shops to generate foot traffic, but not other bakeries selling competing produce.



## A description of the data and how it will be used to solve the problem.

Here are the steps we go through to collect the data:

- Collect postal codes from  [wikipedia](https://en.wikipedia.org/wiki/List_of_postal_codes_of_Canada:_M)  and format it in a similar way as we did for the previous week.
- Use the foursquare API to collect venue information for each of these areas.
- Export the data we have collected into csv format so that it is ready to work with without having to run foursquare API queries again.
- Look for clusters of venues that could generate foot traffic. In particular, how many restaurants, cafes and coffee shops are there in the area. If there is a cluster of shops that can generate foot traffic, then how many competing bakeries are there within this cluster.



## Methodology

The foursquare API data was collected and exported to toronto_venues.csv. There were a few data cleaning steps from here:

* There were a number of generic restaurant venue tags from the foursquare data such as ["Restaurant", "Italian Restaurant", "Japanese Restaurant", ...]. If the venue tag contained the text "restaurant" then we flagged all of these as restaurants.

* We then filtered only the venue categories including categories = ["Bakery", "Restaurant", "Coffee Shop", "Café"]. These are some of the main venue tags if you sort the venues by value counts. We then kept the bakery venues to one side and grouped all of the others as related venues.



Once the data was cleaned and filtered we 


