# Food Safety

**Abstract**

This research paper investigates the restaurant food safety scores for restaurants in San Francisco. Through the use of multiple Data Cleaning methods and Exploratory Data Analysis (EDA), I have cleaned and analyzed multiple data sources, utilizing a metric system that looks at the distribution of restaurant scores over time.


**Introduction**

For this research paper, we will use data provided by the Health Department. The Health Department has developed an inspection report and scoring system based on violations observed at restaurants. Violations can fall into numerous categories: high risk categories (records specific violations that directly relate to the transmission of food borne illnesses, the aldutration of food products, and the contamination of food-contact surfaces), moderate risk category (records specific violations that are of a moderate risk to the public health and safety), and low risk category (records violations that are low risk or have no immediate risk to the public health and safety). This score card is issued by an inspector, which needs to be maintained by the food establishment.


**Data Source 1 [Businesses]**

The business csv file provides us with information about the restaurants. I checked to ensure that all business IDs were unique for each record and created two series to organize the data better. One series has the index "name" of the businesses and the corresponding value is the number of records with that name and the second series has the index "address" of the business and the corresponding value is the number of records with that address. These were sorted and descending order, and I discovered that the minimal primary key is business ID.


**Data Source 2 [SF Zipcodes]**

At the beginning of the data analysis process, I read the csv files using Pandas. When reorganizing the bus.csv file, I recognized that the ZIP code column is qualitative nominal. Furthermore, as I analyzed the provided ZIP codes, a large number were resulting in invalid values. A common invalid postal code was -9999, which could indicate a missing postal code. I constructed a series that counted the number of businesses at each address that have -9999 as their corresponding missing postal code value. Invalid zip codes tend to have address corresponding to some form of "Off the Grid." Though, we can't drop businesses with the missing postal code values because then a small class of businesses will be affected. To further organize the data, 

**Data Source 3 [Inspection]**

The inspection dataframe contained columns for the inspection ID, inspection score, time (including the date and time of day), and type. The inspection id is a primary key since it corresponds to each individual inspection for a restaurant. I split the inspection ID into 2 columns and created a separate column for just the business ID; in reference to the bus['bid'] being a primary key, I noticed that the business ID is a foreign key. I also added a timestamp column to create a more succint data frame that is organized by time. With this analysis, I learned that 2017 had a lot of businesses in newly constructed buildings. Inspection scores appear to only be reported for Routine-Unscheduled inspection types; the rest of the inspection types have missing values. THis oculd be because for other inspectino types like "New Ownership and Complaint," there aren't any corresponding inspection scores.

**What does this mean?**

I merged the Business dataframe and Inspection dataframe to find which restaurants have the lowest scores. First, I filtered out the missing scores from the Inspection dataframe, removing any rows that have a score of -1. Then, I ordered the median scores of each business in descending order and found that Chaat Corner has the lowest median score. 

I further examined the descriptions of violations for inspections that have scores between 0 and 65. To do this, I constructed a series indexed by the description of the violation with corresponding values that are the number of times that specific violation has occurred for inspections. The first few entries with the highest count in the series were "Unclean or unsanitary food contact surfaces" at 43, "High risk food holding temperature" at 42, "Unclean or degraded floors walls or ceilings" at 40, and "Unapproved or unmaintained equipment or utensils" at 39.


Figure 1

<img width="452" alt="g1" src="https://user-images.githubusercontent.com/66439654/118910796-b694df80-b8ea-11eb-8d7b-0698f598bc42.png">

This is the distribution of inspection scores. This seems to be skewed left, which makes sense since working restaurants generally recieve greater inspection scores, or else they would go out of business. Since the inspection score is a discrete variable, I created a barplot.

Figure 2

<img width="532" alt="g2" src="https://user-images.githubusercontent.com/66439654/118910806-be548400-b8ea-11eb-86e3-645d2a83e381.png">

I looked at restaurant locations that have at least three ratings, and analyzed the improvements in ratings. This is imperative in seeing if food safety is improving or getting worse. To analyze this better, I created a new indexed dataframe which has inspections indexed by ID and year, so each row corresponds to data about a given business in a year. The count column will represent the number of inspections for the business in that given year. I used this dataframe to see if there have been improvements in inspection scores from year to year and created a scatterplot to visually show First Inspection Score vs Second Inspection Score.

Figure 3

<img width="616" alt="g3" src="https://user-images.githubusercontent.com/66439654/118910814-c0b6de00-b8ea-11eb-84e1-18953e3267e2.png">

This histogram compares the differences amongst restaurants and their first inspection scores and their second inspection scores. If restaurants' scores tend to improve from the first to the second inspection, I expect that points would be above the reference line and have a positive slope. Unfortunately, it looks like the second inspection doesn't elicit better scores compared to the first. Therefore, there isn't a positive change in the score from the first inspection to the second inspection and because a majority of the points are not above the reference line.

**Summary**

Through this project, I found out that multiple restaurants have multiple inspections in a year. By merging the business dataframe and inspection dataframe, restaurant ratings with the best and worst scores could be found. Additionally, by analyzing the difference in the first and second inspection scores, I found that many restaurants aren't actualy improving their food safety.
