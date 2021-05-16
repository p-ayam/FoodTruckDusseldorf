# "Where Shall I Park My Food Truck in Düsseldorf, Germany?", **asked the client!**

<img src="https://image-service.web.oebb.at/www.nightjet.com/.imaging/default/dam/nightjet/hero-header/header-overlay-1422x800/laender-und-staedte-1422x800/duesseldorf-hafen.jpg/jcr:content.jpg?t=1618495548479&scale=0.5" alt="alt text" width="900" height="whatever">

Project source code can be viewed [here](https://nbviewer.jupyter.org/github/p-ayam/FoodTruckDusseldorf/blob/main/Food%20Truck%20in%20Dusseldorf.ipynb).

## 1. Introducing the Business Case

The client is a gastronomy company, serving **high-quality cuisines on food trucks**. Their business plan is uniquely designed to attract customers that are willing to pay for a relatively pricy meal but are unwilling or too busy to sit in a restaurant or bistro. They mainly operate in neighborhoods that are preferably far from other restaurants or gastronomical competitors. They pay particular attention to the uniqueness of their brand and advertise their company, basically by not advertising for it!

Since the start of the Covid-19 pandemic, the demand for their takeaway meals is soaring, particularly due to the home-office work style of their customers. This has convinced them to expand their business to other cities. Due to logistical reasons, their first choice is the city of Düsseldorf in Germany. The client's wish is to find out the neighborhoods in the city that would be more attractive for their business. More specifically, they would like to find out 2-3 neighborhoods that could fulfill the following criteria:

i) Neighborhoods that offer **the highest number of potential customers**.

ii) The customers are willing to **purchase pricy meals** from the food truck.

iii) The neighborhoods are preferably **far from other restaurants** or gastronomical businesses as potential competitors.

<img src="https://media.architecturaldigest.com/photos/598c98125f33bd7d13f70a46/16:9/w_1600%2Cc_limit/DelPopoloMatthewMillmanPhotoCredit.jpg" alt="alt text" width="900" height="whatever"> 

Photo:Matthew Millman/Courtesy of Del Popolo from [this page](https://media.architecturaldigest.com/photos/598c98125f33bd7d13f70a46/16:9)

## 2. Data Wrangling <a name="data"></a>

Considering the criteria requested by the client, it would be necessary to collect granular data for different districts of the city of Düsseldorf with representative features like demographic aspects and purchasing power. In order to be able to provide a targeted insight for the client, the focus of data collection was geared toward zip codes in the city. That is because more data was available for the zip codes than for the boroughs. 

Specifically, the following data was collected for each zip code:

* The land area of the zip code
* The Population of the zip code
* Average rental price per square meter in the zip code
* Spatial coordinates of the restaurants

Land area and population of the zip code will allow for a quantification of the number density of potential customers in the vicinity of the food truck. The data for these features were collected from this source:
www.suche-postleitzahl.org

The average rental price in each zip code is a representative factor for the purchasing power or willingness of the potential customers to pay for a rather expensive meal from the food truck. This was collected from:
https://www.miet-check.de/mietpreis-plz/

Spatial coordinates of the restaurants will allow for spotting the denser areas in terms of the potential competitors. The client is more interested in regions that are not highly populated with other restaurants. This data was collected using Foursquare API. It was noted that the zip code data provided by this API for each venue was not always correct. In order to access the true zip code for each restaurant, reverse geo-coding of the coordinates was performed using ArcGIS API.

## 3. Results and Dicussion

To address the request of the client, two approaches were employed to select the business-attractive zip codes for their food truck. Here, a brief summary of the methods is provided, with reference to the resulting insights provided in section 4:

**A. Neighborhood score:** In this step, the relevant information from each zip code including 

-the cumulative number of restaurants, 

-land area,

-population, 

-population density and 

-the average rental price for each zip codes 

was analyzed. First, it was be checked whether the total number of restaurants in the neighborhood is correlated to the other factors or not (i.e. the cumulative number of restaurants, land area, population, population density and, the average rental price for each zip codes). The following graph shows (in the first row) a weak correlation with the presented factors, even though the correlation with rent price and population density is stronger than other factors.

<img src="https://github.com/p-ayam/images/blob/main/foodtruck_corr.png" alt="alt text" width="500" height="whatever">

Next, the zip code information will then used to rank the neighborhoods in contrast to each other. For example, the zip code with the highest number of restaurants will get the first rank in "restaurant numbers" while the zip code with the highest rental price will get the first rank in "rent per square meters". A neighborhood with the smallest number of inhabitants will then get the 32nd rank in "population" (there are 32 zip codes in Düsseldorf). 

The ranking information was then be used to calculate a score for the business potential of that zip code for the client. This score is nothing but the following relation: 

###### score = ranking of the unattractive features + 32 - ranking of the attractive features

with 32 presenting the total number of zip codes in the city. Unattractive features for the client's business include higher restaurant numbers and larger land areas while attractive features refer to higher population, higher rental price and a higher population density. Land area, population and population density of the zip codes are all included in the calculation of the scores, despite the latter being a product of the first two. The reason is the higher importance of these factors for the customer in terms of securing a net number of customers. 

The score values calculated for each zip code, indicate that the top three neighborhoods for the client with the luxury-food-truck business model are the zip codes 40219, 40239 and 40215, respectively.

<img src="https://github.com/p-ayam/images/blob/main/foodtruck_zipcode%20scores.png" alt="alt text" width="280" height="whatever">

**B. Clustering the Restaurants:** Here, the geospatial, demographic and rental features relevant for each restaurant will be used to cluster them according to nearby competitors, rent and population density. **Hierarchical Density-Based Spatial Clustering of Applications with Noise (HDBSCAN)** was employed for clustering. It was expected that the restaurants in the business-attractive regions will most likely be labeled as outliers of this clustering algorithm. 13 clusters were identified for the restaurants that are presented here. The dark labels present the outliers of the clustering algorithm:


<img src="https://github.com/p-ayam/images/blob/main/foodtruck_city.jpg" alt="alt text" width="900" height="whatever">

Zip code 40219 (with the highest score) will be discussed here in more detail. This zip code has the highest average rental price in the city, indicating a strong purchasing power of the neighborhood. On the other hand, this could also refer to a larger portion of the population having jobs that require home-office working style, promising a larger portion of active and loyal customers for the food truck, especially for lunch meals. Besides, with more than 10400 inhabitants, this zip code has the 3rd highest population density in the city and a land area not much larger than 0.8 square kilometers as the 5th smallest in the city. This means that majority of the customers are within the walking distance of the food truck, maximizing the likelihood of their daily purchase. More importantly, there are only two restaurants detected in this zip code, providing a market without a major competition. Clustering of the restaurants with the relevant features using HDBSCAN algorithm, labelled the only two restaurants in the most attractive region as outliers, indicating the likelihood of business attractivity and less competition for the proposed zip code.

<img src="https://github.com/p-ayam/images/blob/main/foodtruck_402.jpg" alt="alt text" width="900" height="whatever">

## Conclusion:

The client with the luxury food truck business model needed recommendations on choosing the most business-attractive zip codes in the city of Düssedorf. The important criteria for the selection of the zip codes were:

i) Zip codes that offer the highest number of potential customers.

ii) The customers are willing to purchase pricy meals from the food truck.

iii) Zip codes that are preferably far from other restaurants or gastronomical businesses as potential competitors.


By breaking down the customer's criteria into quantifiable parameters a scoring scheme was employed alongside a clustering algorithm to detect the zip codes that can maximize the revenue of the client and simultaneously minimizing the competitive challenge.

The most attractive zip code area provides three attractive features simultaneously:

##### 1. Strong purchasing power of the customers.

##### 2. High likelihood of having a large population of active and loyal customers particularly for the lunch meals in the working days.

##### 3. Customers that are in the walking distance of the food truck.
