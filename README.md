# Final-Project-Statistical-Modelling-with-Python

## Project/Goals
- Get data from the CityBikes API, the Foursquare API, and the Yelp API
- Combine the data from the different API results
- Build a regression model based on the data and interpret the results

## Process

### Step 1 : Get Helsinki Data From CityBikes API
1. Make use of [the available documentation](https://api.citybik.es/v2/) to understand the structure of CityBikes API
2. Using the [CityBikes map](https://citybik.es/) and looking up Helsinki we get 'City bikes, Helsinki' as the search term. That doesn't tell us the exact syntax of the endpoint, so we can make a request of the API and get some basic info including the endpoint and city. We can filter this data and look only at results with Helsinki as the city.
3. With the exact endpoint we can request all information from the Helsinki endpoint.
4. Review the results. Parse them and put them into a DataFrame.
5. We can save the DataFrame as a csv in order to have easy access to the data from other locations.

The more detailed process of getting Helsinki Data from the CityBikes API can be found in the [city_bikes.ipynb document](/notebooks/city_bikes.ipynb) saved in the [notebooks folder](/notebooks/).

### Step 2: Get Point of Interest Data from Foursquare and Yelp.
1. Review the [documentation page for Foursquare](https://docs.foursquare.com/developer/reference/place-search) and the [documentation page for Yelp](https://docs.developer.yelp.com/reference/v3_business_search). Note that both APIs have a code box that you can test code in. The pages are visually quite similar to each other in terms of the display, so it's important to be aware of which documentation is being reviewed. I chose to review the documentation in tandem to determine if it's possible to format a function that will work with both of these APIs. Due to the similar structures and requirements of the APIs, we can make a function that works well no matter if it's calling from Foursquare or Yelp.
2. Ensure that the coordinates that will be iterated through are ready to access in the notebook.
3. Set up a function to sort through the results and get the data we are looking for.
4. Ensure the variables are set up for Foursquare, using methods to transfer the relatively large amount of category numbers.
5. Ensure the variables are set up for Yelp.
6. Call the respective APIs using the function made in step 1.
7. Format the respective API results using the function made in step 3.
8. Put the parsed results into their respective DataFrame.
9. Clean up both DataFrames.
10. Use the cleaned DataFrames to compare the completeness of the different API results.
11. Get the top 10 restaurants according to rating.

The more detailed process of getting Point of Interest data from the Foursquare API and the Yelp API and cleaning and comparing the results can be found in the [yelp_foursquare_EDA.ipynb document](/notebooks/yelp_foursquare_EDA.ipynb) saved in the [notebooks folder](/notebooks/).

### Step 3: Join Data from Step 1 and Step 2
1. Load in both API data DataFrames.
2. Add prefixes to columns to match the respective DataFrame; Foursquare or Yelp.
3. In each API DataFrame, Round the latitude and longitude to 3 places and create a column 'Rounded [coordinate type]' in each DataFrame. 
4. Make an ID that combines these rounded coordinates for each DataFrame.
5. Find close matches for names.
6. Merge to create a new DataFrame on close matches using the ID.
7. Get a search value for each row. This step is useful since categories will have to be adjusted either way and this removes the chance of ambiguity that comes with having similar but different categories in the two DataFrames. Having a search column means that the Foursquare and Yelp values will be distinct in that Foursquare's search keys are numeric in comparison to Yelp's string.
8. Get a category for each row.
9. Clean up the DataFrame by dropping unneeded columns and combining values that appear in both the Foursquare and Yelp data so the excess can be removed.
10. Add a label column for the combined API DataFrame as well as the Helsinki bikes DataFrame.
11. In the Helsinki Bikes DataFrame created from the CityBikes API data, Round the latitude and longitude to 3 places and create a column 'Rounded [coordinate type]' in each DataFrame. 
12. Concatenate the Helsinki bikes data into the combined API DataFrame.
13. Change the bike spot column in data points with no bike spots (the POI data points) into a 0 value.
14. Use the average rating to impute nulls in the rating column.
15. Look at a visualization of the data and describe the pattern or relationship that it depicts.
16. Put all the results into a database.

The more detailed process of joining the cleaned data from the Foursquare API, the Yelp API, and the CityBikes API can be found in the [joining_data.ipynb document](/notebooks/joining_data.ipynb) saved in the [notebooks folder](/notebooks/).

### Step 4: Build a Model based on the Data
1. Remember the target. For this project the target is available bike spots.
2. Choose the features for the model. I chose Rounded Latitude, Longitude, and Rating.
3. Check for correlation between features. High correlation can skew the model.
4. Add a constant.
5. Make the model.
6. Review the model and interpret the results.
7. Remove features if their p value is above the 0.05 threshold.

The more detailed process of building a model based on the cleaned data from the Foursquare API, the Yelp API, and the CityBikes API can be found in the [model_building.ipynb document](/notebooks/model_building.ipynb) saved in the [notebooks folder](/notebooks/).

## Results
Yelp's data points all have ratings unlike Foursquare's. Yelp didn't have category data on every data point where Foursquare did. Ratings are harder to extrapolate than categories, so Yelp's data is more complete.

The model explains 0% of the data. The relationship between each feature (Rounded Latitude, Rounded Longitude, and Rating) and the number of free bikes is most likely from natural variation as opposed to a cause and effect relationship.


The more detailed comparison of the Foursquare API and Yelp API coverage can be found in the 'Comparing Results' section of the [yelp_foursquare_EDA.ipynb document](/notebooks/yelp_foursquare_EDA.ipynb) saved in the [notebooks folder](/notebooks/).

The more detailed interpretation of the model output can be found in the 'Provide model output and an interpretation of the results.' section of the [model_building.ipynb document](/notebooks/model_building.ipynb) saved in the [notebooks folder](/notebooks/).

## Challenges 
When setting up the API calls there were lots of variables to consider. It was important to make sure each call would give the desired output. I was unsure of how to approach the calls and made the decision to build a function in order to standardize my process and reduce error. This did mean I had to be very specific in the variables I set up.

I also made some calls where my results were undesired. I changed my call function to display the results of each call in real time. This allowed me to identify issues earlier. 

When running tests I had mis-copied my Yelp API key and had to step back and restart my process from the beginning to even realize I was missing a chunk of the key.

When deciding on what values to keep from the API calls I decided on some values that would be used for a classification model. I treated the cleaning and maintaining of these values as part of the project in order to not lose some data when dropping duplicates. I did however allocate a lot of attention to extra category cleaning when it was outside of the scope of the project.

Cleaning data always brings the challenge of deciding how to handle null values. I decided to fill in null ratings with a 0. Null prices became a new category. 
 
## Future Goals

I would get the 'Category' and 'Price' columns ready by making the values in them into separate columns and having 1 mean that data point has that value and 0 mean that row does not have that value. Using price as an example, there are 5 possible outcomes ['Not Listed', 'Most Affordable', 'Affordable', 'Expensive', 'Most Expensive']. A data point that has a Price value of 'Not Listed' would have 1 in 'Not Listed' column and 0 for the other 4 columns made from the Price values.

When imputing nulls for the model used the overall average. If I had more time I would look into using methods like models to better fill in null ratings.

I wanted to set up my variables so things are more repeatable, but due to the outputs on APIs it's not as simple to make a one size fits all for call results. I looked into Object Oriented Programming and if I had more time I would try solutions using classes to perform some sort of templating. But given how API outputs can change, I would likely also end up spending time on mapping complete logic to account for changes, which may not be worth it.

I would use the distance feature to make some sort of comparison of density in relation to a point based on its relative location (eg. further north having more POI of high rating close to it). I removed distance for this project in order to be able to remove duplicate points to not skew data. If I had more time I would decide on a way to work with the distances. It would likely be working with the same base data in a different DataFrame and grouping the data by name, coordinates, and distance.