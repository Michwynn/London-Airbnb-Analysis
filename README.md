# London-Airbnb-Analysis

Airbnb is an online marketplace which provides a platform for home-owners (the host) to rent out their properties to people looking for an accommodation for short-term use. In 2021, there were over 7 million properties (also called listings) on Airbnb and the type of property varies from a shared room to a private room to the entire apartment with property rentals varying from around $60/night to $12,000/night. How are these listing prices decided? Though Airbnb does help the host in setting up a pricing strategy, the price of the property is eventually decided by the host. In a highly competitive market how does the host ensure that they price their properties optimally so as to maximize their earnings? What are the factors which show a strong correlation with the property prices? Is the size of the property the biggest influence on the price? Does the crime rate of a locality influence the property prices? Does proximity to public transport matter?
We look at the Airbnb data for approximately 68,000 property listings in London and analyse in conjunction with other publicly available data on crime rates, public transport, population density and proximity to tourist attractions to identify the key factors influencing the property prices.    

**Primary Datasets used:**

Listings dataset


Detailed listing data for all London airbnb properties that provide names, text descriptions of properties, neighbourhood locations, latitudes, longitudes, price and other relevant attributes pertinent to the sizes of properties and review scores. Each listing is uniquely tagged to its ID, along with its unique host ID. 
67,904 rows of recorded listings 

Reviews dataset


Detailed review data for all London listed airbnb properties. Data columns include review IDs, Listing IDs, date of review, the reviewer’s name and the review itself in text format. 
1,048,405 rows of reviews 

 Calendar dataset


Detailed calendar data for all London listed airbnb properties, which includes listing IDs, date of listing, whether listing was available at that point in time (Binary variable), price, adjusted price, the maximum and minimum night requirements of listing.
1,070,626,000 rows of listings by date

For all three datasets:


Location: http://insideairbnb.com/get-the-data.html
Format: CSV
Access Method: Download

**Secondary Datasets used:**

Crime dataset


Metropolitan Police service data in London which includes column that are a high level description of crime, a more granular level description of crime, location of crime, and the number committed to each crime description each month from Nov 2019 to Oct 2021 (each month as a column)
1,556 rows of data
Location: https://data.london.gov.uk/dataset/recorded_crime_summary
Format: CSV
Access Method: Download

Population Density dataset


Population data for London that includes the area name, inland area, total area, population per hectare, 2011 Census population figures for each area. 
34 relevant rows in ⅓ tabs
Location:https://data.london.gov.uk/dataset/land-area-and-population-density-ward-and-borough
Format: CSV
Access Method: Download

Public Transport dataset


London underground data including station IDs, latitudes, longitudes, name, zone 
Approximately 310 rows
Location: https://commons.wikimedia.org/wiki/London_Underground_geographic_maps/CSV
Format: HTML
Access Method: Web scraping

Leading Visitor Attraction dataset


2020 visitor attraction dataset which includes ranking of visited site, name of attraction, total visitors, Area (Filtered for == “London”)
41 relevant rows of record
Location: https://www.alva.org.uk/details.cfm?p=423 
Format: HTML
Access Method: Copy/Paste into excel and tagged each area of interest manually for longitude, latitude and borough 


**Cleaning and Manipulation**


The primary Airbnb dataset is broken out into 3 datasets: Listings, Calendar, and Reviews. Each airbnb is considered a listing, and has a one to many relationship with Calendar, and a one to many relationship with Reviews. Listings joins to Calendar on [Listings].[id] = [Calendar].[listings_id], and Listing joins to Reviews on [Listings].[id]= [Reviews].[listing_id]. 

The above Airbnb dataset can then be joined to all other secondary datasets by joining on borough names (there are 32 boroughs that make up the greater London area). We will join these datasets to the borough names that already exist, and to boroughs whose names we will automate through their lat/long coordinates. ex) [Listings].[neighbourhood_cleansed] = [Crime].[LookUp_BoroughNames] 

Some cleaning challenges of this data will be with the borough names. There could be different naming conventions through the dataset that make joining incompatible before we check for and correct these.


**Analysis**


Step 1: We will use scatter plots, bar graphs, SPLOM and Pearson’s correlation coefficient to visualize the strength of the relationship between price of the listings (the target variable) and the various variables (independent variables) available in the primary data obtained from Airbnb. The independent variables include total number of listings by the host, minimum & maximum number of nights requirement, availability for the next 365 days, number of reviews etc. We also study the correlation amongst the independent variable
Step 2: We will explore additional publicly available data on crime rates by locality, proximity to London underground stations, proximity to tourist attractions etc. and try to measure the correlation to the rent of Airbnb properties.
Step 3: Finally, we use multivariate regression (with price of the listing as our target variable) to compute partial slope and partial correlation coefficient. This helps measure the degree of association between the price of a listing (Y) and each of the independent variables(X) while controlling for all the other confounders. This will allow us to better estimate if the relationship between X and Y is direct, spurious, or intervening


**Visusalisations**


Scatterplot Matrix / Heatmaps - Finding the correlation between different quantitative variables that affect prices of listings - considering the number of bedrooms, review scores, host ratings, etc. One of the questions we can answer is how much weight customers, who stayed at these Airbnb properties in London place, place on service?  We can do this on borough level and then on a consolidated level. 

Bar charts - Trying to investigate if crime rate affects the listing’s price and availability by borough. Do boroughs with more crimes committed per square area affect the listing prices and demand? Are there certain crime classifications that correlate with significantly lower prices?

Geopandas and Basemap - We can visualize geographical data using longitude and latitude values of all listings and see which areas in London have the most listings, most expensive, have the highest number of Airbnb visitations, etc. This can also apply to visually inspecting areas with more crimes committed, more densely populated, etc. 


Word Cloud - Identifying the most common words used to describe the Airbnb properties in London, that are unique to the highest rated and lowest rated listings
