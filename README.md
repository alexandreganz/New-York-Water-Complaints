# **New-York-Water-Complaints**
This repository is a New York City public policy prioritization study where we analyzed over 50 million complaint lines involving water, quality and market value of real estate within the city.

# Use Case

- Use Case Summary:
- Objective Statement:
  * Gain insight into how the city government acts on water issues with its residents.
  * Identify patterns in water quality and complaints to make transparent the impacts generated by poor sanitation quality.
  * Identify the most valued regions from a market value point of view in the city and thus understand the relationships with public complaints involving sanitation.
  
- Challenges:
  * Extremely large data size, making it a real challenge to process it using Python.
  * Joining of many databases to form a complete detail about the public policy scene within the city.
  * Non-standard spatial transformations in the databases make the analysis difficult, requiring a lot of time to think of a standard conversion format.
 
- Methodology / Analytic Technique:
  * Descriptive analysis
  * Spatial analysis
  * Random Forest
  * Reverse geocode
  
 - Expected Outcome:
  * Present a clear pattern of response in the resolution of public complaints related to water, where the city government focuses more on the appreciated regions instead of the distant and less valued ones.
  * Determine what are the key features that determine the creation of a complaint and thus provide insights on how to address the root problem.
  * Identify the areas with the worst water levels in the city, thus making it clear to the city of New York where they must act to correct their problems
  
 # Data Understanding
- Source Data: This project used 5 different data sets
  * [311 Service Requests from 2010 to Present](https://data.cityofnewyork.us/Social-Services/311-Service-Requests-from-2010-to-Present/erm2-nwe9): Daily updated public database on public maintenance requests within the city.
    - The dataset has 41 columns with more than **25 million of rows**, with a daily update.
    - Data Dictionary can be found [here](https://data.cityofnewyork.us/Social-Services/311-Service-Requests-from-2010-to-Present/erm2-nwe9)
   * [Drinking Water Quality Distribution Monitoring Data](https://data.cityofnewyork.us/Environment/Drinking-Water-Quality-Distribution-Monitoring-Dat/bkwf-xfky/data): Hourly measurements of water quality at different locations within the city, in order to detect levels of turbidity, colifrom. fluride and chlorine.
     - The dataset contains 10 columns with more than 100 thousand rows, with a daily update.
     - Data Dictionary can be found [here](https://data.cityofnewyork.us/Environment/Drinking-Water-Quality-Distribution-Monitoring-Dat/bkwf-xfky)
   * [Home Rolling Sales NYC](https://www1.nyc.gov/site/finance/taxes/property-annualized-sales-update.page)
     - The dataset contains 21 columns with more than 50 thousand rows, with a yearly update divided by NYC four boroughts (Manhattan, Bronx, Brooklyn, Queens, Staten Island)
     - Data Dictionary can be found [here](https://www1.nyc.gov/assets/finance/downloads/pdf/07pdf/glossary_rsf071607.pdf)
   * [NYC ZIP Tabulation](https://jsspina.carto.com/tables/nyc_zip_code_tabulation_areas_polygons/public/map)
     - This is a dataset to make the spatial analysis possible, it is a GeoJSON tabulation of each ZIP code in New York City, it will be merged with all the above data in order to extract insights.
     
# Data Preparation
 - Code Used:
 - Python Version 3.9.7 
 - Alteryx Designer Version 2022.1.1.25127
 - Packages: Numpy, pandas, seaborn, matplotlib, sklearn, json, math, plotly, geopandas, area, bokeh, RateLimiter, geopy
 
# Data Cleansing
- The initial phase of the cleansing was made in Alteryx, due the substantial volume of the file size (16 GB) and amounf of rows (25 million) I had to transform the data to extract only service requests regarding water releated problems.

![alteryx](https://i.imgur.com/ZSw8I8n.png)

- The second phase was merging, service requests, home sales, water quality data and the ZIP tabulation in a monthly basis in order to have the whole dataset using the same level of detail (LoD)

- The third phase was using a API to do something called reverse geocode, which based on the Latitude and Longitude measured by each quality site I could extract the ZIP code and then compare with the sales price and service requests.

- List of Service requests that we are tracking:
  - Plumbing
  - Sewer
  - Water System
  - Water Quality
  - Root/Sewer/Sidewalk Condition
  - Indoor Sewage
  - Drinking Water

# Exploratory Data Analysis

 - **Average housing prices in New York City by Zip code:**
 
 ![sales](https://i.imgur.com/OGJONWK.png)
 
 ![map](https://i.imgur.com/P4vM4u5.png)
 
 As we can see, the beginning of our investigation shows that the most expensive real estate in the city is found on Manhattan Island, this is not a surprise to anyone, but as the purpose of this project is to understand where the government is working to solve water-related problems, it is important to understand which areas are the most targeted in terms of capital.
 
 - **How many service requests each ZIP code has?**
 ![complaints](https://i.imgur.com/2fk4tlb.png)
 
 ![map_complaints](https://i.imgur.com/5rEmqNB.png)
 
 As we can see here there is a clear inversion of the data in comparison to the market price of real estate vs. the number of requests for water services, apparently the most distant regions of Mannhathann Island are the ones that request the most repair services
 
 - **The Relation of home prices vs amount of complaints**
 
 ![relation](https://i.imgur.com/fJKHCPi.png)
 
 Another finding is that, over the years the value of properties has increased when the number of complaints has decreased, some of the hypotheses could be an extensive renovation work in the region or a greater focus of the government on dealing with such complaints/repairs. However, note that there are areas where this relationship was opposite, especially in the Bronx area (Bronx Park and Fordham, High Bridge and central Bronx) the number of complaints remained the same, or decreased less than other areas.
 
 - **Water Quality in Bronx**
 
 ![coliform](https://i.imgur.com/wmuC2KC.png)
  
  Something worrying that has been identified is that in the same regions there have been peaks of [coliform](https://doh.wa.gov/community-and-environment/drinking-water/contaminants/coliform#:~:text=Coliform%20bacteria%20are%20organisms%20that,be%20in%20the%20water%20system.), which are fatal to human health.
  
  
  
  ![bronx_time](https://i.imgur.com/5iDtYaL.png)
  
  To confirm this theory, notice that in the bronx region the coliform level is extremely high in 2015 and 2016, but it seems that the city has solved this problem in 2017 and 2018, and the peaks are high again in 2020 and 2021. In contrast, manhatann, having a similar problem, was solved from 2018 with a slight peak in 2021 (nothing compared to bronx).
  
  
 # Recommendation
 
 - With the data shown for New York City it is clearly possible to see a greater focus on areas that are not as well appreciated as the island of Manhattan, especially the borough of the Bronx where there are worrying indications of coliforms and drinking water quality.
 
 - The details of the study, with more information on how we do data collection, processing and additional insights (Forecasting, random forest) are available in the [notebook](https://github.com/alexandreganz/New-York-Water-Complaints/blob/main/FinalProject.ipynb).
