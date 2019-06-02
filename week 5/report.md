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

* ```python
  def group_related_shops(venue):
      # group the venues ["Restaurant", "Coffee Shop", "Café"] as related
      if venue in ["Restaurant", "Coffee Shop", "Café"]:
          return "Related Venue"
      else:
          return venue
  
  df["Venue Category"] = df["Venue Category"].apply(lambda x: group_related_shops(x))
  df["Venue Category"].value_counts()
  ```

We expect the restaurants, coffee shops and cafes to generate foot traffic, so we'd want to open our bakery close to these venues. On the other hand we want competing bakeries to be kept to a minimum. Within the notebook we'll take two steps:

* We'll use the kmeans classifier to look for shop / related venue clusters.

* For each of the clusters we'll search for the lowest number of bakeries relative to other related shopping venues.

## Results

We collected the foursquare data for Toronto, cleaned and filtered it as described in the methodology section, then split the venues into bakeries and related shopping venues. This leaves 45 bakeries and 642 shops / related venues.

![counts](https://github.com/simongarisch/Coursera_Capstone/blob/master/week%205/venue_counts.png)

We then looked for shop clusters using kmeans. Each cluster was assigned a different colour and the bakeries were overlaid in red. 

![counts](https://github.com/simongarisch/Coursera_Capstone/blob/master/week%205/related%20venue%20clusters.png?raw=true)

Now that we have the related shopping clusters the next step was to look at the ratio of shops that were bakeries.

![counts](https://github.com/simongarisch/Coursera_Capstone/blob/master/week%205/num_bakeries.png?raw=true)

The purple shopping cluster to the North has a reasonable number of related shops, but no bakery.  Second in line is the orange cluster to the North East.



## Discussion

It looks like of all the clusters the one to the North in Eglinton is short on bakeries. A quick look at [google maps](https://www.google.com.au/maps/search/bakery/@43.7062441,-79.400403,17z/data=!4m2!2m1!6e5) shows that one bakery has popped up in the area, but largely this could be a reasonable bet in terms of low competition and other shopping venues nearby to generate foot traffic.

Wellesley also looks like a good bet as from the foursquare data there are a variety or shops in the area, but no bakery. However, as mentioned earlier, a [google search](https://www.google.com/maps/search/bakery/@43.667978,-79.3928821,15z) of the area showed a number of bakeries nearby. I suspect that the foursquare data is not quite as complete or up to date as what is available on google search.



## Conclusion

It looks like there are a lower number of bakeries relative to other shopping venues in the North of Toronto (Eglinton area). For a client looking to open a bakery this would be a good area for further investigation as there should be customers available and lower competition.
