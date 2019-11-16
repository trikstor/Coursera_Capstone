

# The best places for placing a supermarket.

**Applied Data Science Capstone by IBM/Coursera**

## Introduction 

Let's assume that our stakeholder is a supermarket chain. The company operates in a very competitive and volatile environment, so this company has to find new places to expand and leave unpromising locations. This company wants to expand its network in Yekaterinburg city in Russia. But the market in this city is very competitive and we need to do an analysis to find the best places to place supermarkets. Ideally, we need to find crowded places in residential areas without supermarket competitors in the areas.



## Data 

We need to find places without supermarkets in the area, with public places nearby like a shopping mall or a business center. Residential areas can be distinguished by the presence of schools, kindergartens, and hospitals (social venues) in a neighborhood. All the venues we need can be found using the Foursquare API. Yekaterinburg city is divided not into neighborhoods, but huge districts and it's useless to find places for supermarkets by districts. I will divide the city by post offices because post offices are evenly distributed throughout the city. Also, almost every post office has its postal code.

**Official districts of Yekaterinburg**![img](https://upload.wikimedia.org/wikipedia/commons/3/34/Ekb_all_districts.svg)

### Getting post offices

We need to get post office locations and their postal codes. We can find it with the Foursquare API. After that, we can see, that Foursquare API returns some locations without postal code. There are also duplicate postal codes. We have to delete such venues.

![PostOffices](https://github.com/trikstor/Coursera_Capstone/blob/master/Assets/PostOffices.png?raw=true)



### Getting venues nearby post offices

The idea is to find places from different categories that can influence the success of the supermarket in a particular place. We will find and visualize the number of venues in around post offices in every category:

- Social venues
  - Schools
  - Hospitals
- Competitor venues
  - Big Box Stores
  - Supermarkets
  - Food & Drink Shops
- Crowded venues
  - Business Centers
  - Offices
  - Entertainment Centers

As we rely on people coming to a supermarket it must be accessible by walking no more than 15-20 minutes from any place in the area. Assuming the average speed of a person is 6 km/h it will result in a radius of 800 meters.

Social venues:

![SocVenues](https://github.com/trikstor/Coursera_Capstone/blob/master/Assets/SocVenues.png?raw=true)

The largest number of venues:

* 620000 – 23 venues

* 620100 – 16 venues

* 620142 – 14 venues

Competitor venues:

![CompVenues](https://github.com/trikstor/Coursera_Capstone/blob/master/Assets/CompVenues.png?raw=true)

The largest number of venues:

* 620131 – 22 venues

* 620130 – 18 venues

* 620142 – 18 venues

Crowded venues:

![CrowdVenues](https://github.com/trikstor/Coursera_Capstone/blob/master/Assets/CrowdVenues.png?raw=true)

The largest number of venues:

* 620000 – 33 venues
* 620102 – 24 venues
* 620027 – 22 venues

## Methodology 

We collected post office coordinates and their postal codes for dividing the city on neighborhoods because it is impossible to analyze with administrative districts because of their huge sizes. After that, we collected data about venues nearby every post office. We used venue types of three relevant for us categories: social venues, competitor venues(other supermarket chains) and crowded venues. Now we need to analyze the data. As I think, the best approach is ranking areas by computing a grade for every area. The formula for ranking will be:

𝑟=𝑠𝑜𝑐+𝑐𝑟𝑤+2∗𝑐𝑚𝑝
*𝑟* - rank
*crw* - number of crowded places
*cmp* - number of competitor venues

The presence of social and crowded venues is a positive factor, the presence of competitor venues is a huge negative factor, so the multiplier is valid in this case.
Also, we will try to separate the data using the DBSCAN clustering model, because this model is robust to outliers and can find arbitrarily shaped clusters. I want to try to prove ranking results by clustering results.



## Analysis 

Let's find out the best places for placing a supermarket. So we need to find the grade for every postal office. Also, we will try to find the difference between areas with the clusterization model.

The DBSCAN model as an alternative approach to prove the result. Use search grid to find the best params. The best clusters number is 18.

Ranking result:

![RankingResult](https://github.com/trikstor/Coursera_Capstone/blob/master/Assets/RankingResult.png?raw=true)

The best graded neighborhoods

* 620000 – 24
* 620027 – 14

* 620102 - 10

Clusterization result:

![ClusterResult](https://github.com/trikstor/Coursera_Capstone/blob/master/Assets/ClusterResult.png?raw=true)

Outliers:

* 620000
* 620027

## Results and Discussion 

As we can see, the best results by the ranking are highlighted by DBSCAN as outliers, It means that these results are different from other neighborhoods. So the DBSCAN approach almost completely proved the ranking result.

Let's look the best postal codes:

- Ranking:
  - 620000
  - 620027
  - 620102
- Clustering:
  - 620000
  - 620027
  - 620012

620102 and 620012 areas have not such a good grade as 620000 and 620000, so let's exclude these areas. So the best places for placing a supermarket are around these post offices:

| Address |         Postal code |    Lat |       Lng | Social venues | Competitor venues | Crowded venues |      |
| ------: | ------------------: | -----: | --------: | ------------: | ----------------: | -------------: | ---- |
|       0 | ул. Мельковская, 2Б | 620027 | 56.850024 |     60.602210 |                10 |              9 | 22   |
|      28 |   просп. Ленина, 39 | 620000 | 56.839337 |     60.608463 |                13 |             14 | 30   |

The result is like the truth. These places are residential areas with crowded places in the proximity. We considered the vicinity of 800 meters around every coordinate, so stakeholders should find places in a radius 800 meters from the post offices. However, the result isn't ideal, because we considered only venues nearby post offices and we could skip some suitable places for supermarkets. Also, we relied on the most popular venue types and didn't use rare types of venues.



## Conclusion 

The purpose of this project is to find the best places to place supermarkets in the city with a competitive market. We divided the city into neighborhoods by postal offices and found relevant venues with Foursquare API. After the analysis, we have found two of the best places for placing a supermarket. The final decision on optimal supermarket location will be made by stakeholders based on specific characteristics of neighborhoods and locations in every recommended zone, taking into consideration additional factors like availability of retail space or district welfare level.
