---
layout: page
title: Chicago Crime Data
description: UNC STOR 320
img: assets/img/chicago_background.jpg
importance: 2
category: my work
related_publications: true
toc:
  sidebar: left
---

## Introduction
For most Americans, Chicago is often what people think of when they imagine a “dangerous city”. Chicago is located near the Great Lakes, and its population grew rapidly over the mid-19th century. Although Chicago is a leading city for finance, technology, and trade, it’s also notorious for having one of the highest crime rates in the American Midwest. Chicago’s high violence rate stems from many drug and gang-related operations concentrated in the inner city. However, gangsters and drug dealers are not the sole reason for Chicago’s high crime rate, it is also due to the poor resource distribution that negatively affects the city overall as well.

Obviously, crimes are caused by violent individuals, but the biggest perpetrator causing violent crime is the poor environmental conditions given to Chicago citizens. These environments include poverty, poor education, and a lack of mental health institutions. Another problematic factor to consider for Chicago is the low arrest rate in the city, leading to higher stress within police departments. It would be important to know the factors that lead to the highest probability of an arrest. Our dataset is limited to all crimes committed in 2016. Therefore, it is important to note that all analyses conducted this year cannot be extrapolated to other years, since we are not analyzing multiple years.

Given that, the first question that we decided to focus on was, “What variables are most significant when predicting if an arrest has occurred in 2016?” Meaning, if we were given a random police report, could we determine if the police were able to arrest the individual, or were they able to escape? While there were many variables within the dataset that could have impacted whether or not an arrest was to be made, we decided to investigate and determine which variables would have had the most impact. Knowing the most impactful variables could be vital in increasing the arrest rate for Chicago police, leading to a safer city for innocent people. This question could not only be vital for Chicago’s police department, but for other law enforcement agencies across America as a whole.

Our second question relates to the idea of what other social problems conflate with the high crime in Chicago. The social issues we will delve deeper into are poverty and mental health hospitalizations. This leads to our question, which is “Is there a connection between areas of high mental health hospitalizations and/or poverty when dealing with areas of high violent crime in Chicago?” Finding connections between Chicago’s crime rate and its social problems could lead to solutions that could substantially decrease the overall crime. We will use the longitude and latitude coordinates within our Chicago crime dataset in order to create a scatterplot of crimes across Chicago, and then compare it to two other heatmaps from other sources. Conclusions from this question could lead to better knowledge about how high crime rates are created, and which communities politicians in Chicago should pour more resources into.

## DATA
The dataset came from The City of Chicago Data Portal, an organization that updates the crimes reported at least once a day. While the dataset is made to represent crimes from 2001 to present day, the project focuses primarily on crimes reported in 2016. The dataset has 269,786 rows in total, and each row represents a case that was reported. Each row has a unique ID which can be used as a primary key in the dataset. The cases are either classified as reckless homicide or first degree murder and all the cases fall under the Primary Description variable as homicide. The Arrest variable classifies whether an arrest was made based on a true or false. The project’s first follow up question focuses on whether an arrest can be determined in 2016 based on 5 variables. First, Location Description indicates the description of the location where the incident occurred. The project is limited to the top 10 location descriptions with streets having 440 crimes then auto having 87 crimes with the tenth place being gas stations having 9 crimes. Second, District indicates the police district where the incident occurred. Third, Community Area indicates the 77 community areas in Chicago. Fourth and fifth, Day indicates the 7 days the crime could have occurred and Month indicates the 12 months the crime could have occurred. The project’s second follow up question mainly looks at the Location variable which is the combination of the Latitude and Longitude with the longitude and latitude of percent household poverty in Chicago.

The table groups the ID by community area and location description, displaying the counts of number of cases, true for arrest, false for arrest, and the arrest rate. The arrest rate variable is true divided by the number of cases. Looking at the first table, while community area 25 has the highest number of cases, the top 10 community areas are all similar in number of cases, meaning that not one community area will be looked at more highly for the analysis. Community area 29 and 24 has a similar number of cases but community area 29 has a 3 times larger arrest rate value, indicating some significant external variables are involved. Then looking at the second table, the location description apartment and restaurant have similar arrest rate but the apartment is 7 times larger in number of cases. Additionally, while streets and sidewalks seem similar in location, sidewalk’s arrest rate is 3 times the street’s arrest rate.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/p1.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/p2.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

The plot displays the arrest variable based on the top 10 community areas due to most community areas only having one recorded crime. Looking at the first plot, community area 25 is an outlier with over 15,000 counts of arrest which skewed the plot scale for the rest of the bars. Beside the first community area, the rest of the 9 community areas are all similar to each other in terms of arrest counts. It is important to note that in all the community areas, the false counts are significantly higher than the true counts. Then looking at the second plot, the location description at street has the most counts of arrest but it is not overly significant to consider it an outlier. Lastly, the project’s first follow up question highlights whether an arrest can be predicted. By looking purely at the counts of the arrest, one can make a conclusion that an arrest will probably be made in any community area. But with additional variables in the table, an arrest can be more accurately predicted.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/p3.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/p4.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

The original dataset had multiple categories for the Primary Type and Description variables. For a crime, the primary type could be theft, battery, or criminal damage and the description could be over $500, liquor license violation, or simple. In order to simplify the dataset, both the primary type and description variables were taken into consideration and all the crimes that did not fall under criminal activities were dropped from the dataset. The original dataset had 269,786 rows. The dataset that was used for analysis removed all “non-criminal” offenses and changed all the primary types of “homicide.”

## RESULTS
### Question 1: What variables are most significant when predicting if an arrest has occurred in 2016?

We constructed 15 models to see which variables were the most important for predicting the event of an arrest happening in Chicago. We have a wide variety of models, where some models cover one variable, and some other models cover multiple variables. We included all data points within our main dataset without null values for our model creation.

-Model 0: Arrest ~ 1
-Model 1: Arrest ~ primarytype
-Model 2: Arrest ~ description
-Model 3: Arrest ~ locationdescription
-Model 4: Arrest ~ domestic
-Model 5: Arrest ~ communityarea
-Model 6: Arrest ~ day
-Model 7: Arrest ~ month
-Model 8: Arrest ~ latitude
-Model 9: Arrest ~ longitude
-Model 10: Arrest ~ locationdescription + communityarea
-Model 11: Arrest ~ latitude + longitude + communityarea
-Model 12a: Arrest ~ day + month
-Model 12b: Arrest ~ day + month + longitude + latitude
-Model 12c: Arrest ~ day + month + longitude + latitude + communityarea
-Model 12d: Arrest ~ day + month + longitude + latitude + communityarea + domestic
-Model 13: Arrest ~ day + domestic + latitude

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/p5.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

In the first question, we want a visualization in order to find the variables that are the most significant in predicting an arrest in 2016. We made a bar plot in order to figure out the various relationships. The model constructed looked at the mean squared error of variables by arrest. In the second bar plot, model 1 and 2 were taken out because they were outliers and skewed the plot scale for the rest of the plot. This makes sense because the primary type and description only had one or two values. Model 0 to 10 looked at individual variables and no significance was seen. So in the next four models, we decided to cross the various variables. Even after taking multiple variables into consideration in determining the arrest, the error values were still similar to the individual function. We were surprised to see that, especially model 13c, did not have significantly lower error value. Model 10 had the highest error, which was not very surprising because it was a function of latitude. But compared to model 9, which was a function of latitude, model 10 was almost two times higher.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/p7.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

When we take the residual by fitted plot we see an interesting plot being formed. This is because our dependent variable is binary, and many other variables in our dataset is categorical. When we make a model:arrest primarytype, we are plotting a logistic regression our dependent variable, arrest, is a TRUE or FALSE variable (binary). Therefore, when we take the residual, we should see a logistic regression form, where the difference between the predicted values of arrestand observed values of arrest or logarithmic. In a standard linear regression, a residuals by fitted plot will have seemingly random points around the 0 line. If it is a good linear regression, the dots will have an even variance across the 0 line, with the dots close to the 0 line. However, our regression is not linear, it is logarithmic. Converting the logarithmic model into a linear regression should produce a normal residuals by fitted plot.

### Question 2: Is there a connection between areas of high mental health hospitalizations and/or poverty when dealing with areas of high violent crime in Chicago?

In our EDA, we diverted our second question from looking at the pattern crimes happening over the course of months, to focusing on where crimes were committed in Chicago. We thought it would be more powerful to look at the locations, since we could compare our data to other third party maps of Chicago, as possibly draw some significant observations. The Chicago Crime dataset provides values of longitude and latitude, making it possible to locate specific coordinates of crimes. Using a package called ggmap(), we plotted two separate maps of Chicago using the longitude and latitude variable given to us in our main dataset. The first map is a sample of 1000 crimes committed across Chicago, and the second map are all the reports of homicides in Chicago. There may be possibly more cases for the map plotting homicides since we also removed observations with null location coordinates. Each black circle is an instance of a homicide reported. In instances of reported homicides happening very close to each other, the circle will darken, indicating a higher density of homicides in that area.

The first map we make is a random sample of 1000 crimes that have been committed across Chicago in 2016. Generating a map of all crimes was not possible due to the hundreds of thousands of data that had to be loaded in. A random sample would give us a good look of where most crimes occured. Looking at the map generated, we cannot really pinpoint a specific area of high crime, but instead about 3 different areas centered around west, south and mid-east Chicago.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/p8.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

Looking at the all the map of all homicides that happened in Chicago, we can see some differences from this one, to the random sample we had. First of all, there is an absence of homicide in mid-east of Chicago. Also, we see two hot spots of homicide, which is primarily focused on the Westside and Southside of Chicago. The map below this one shows which homicides led to an arrest in green, and those were not in red. One interesting observation is that homicides happening away from the hot spots looked to have a higher rate of arrests.

<div class="row justify-content-sm-center">
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/p9.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/p10.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

As stated in our introduction, Chicago’s crime problem is the result of failures within other social departments in Chicago. We chose to look at two different maps not possible to be generated from our data. These two maps are the densities of Percent Poverty Households and Behavioral Health Hospitalizations. Looking at solely these two graphs, we can instantly see a connection in the high density areas the both share in Westside and Southside Chicago.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/p11.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Image Credit: Percent Poverty Household Image: Household Poverty from Healthy Chicago, Chicago Department of Public Health, Source: American Community Survey, 2008-2012 Behavioral Health Hospitalizations Image: : IDPH, Division of Patient Safety & Quality, Discharge Data, 2017; US Census Bureau, 2010 Census.
</div>

We will choose our homicide map to compare to the two other maps since homicide is one of the most problematic crimes in Chicago. When we overlap the locations of homicides to the heatmaps of poverty and mental health hospitalizations, we can see clear connections to high areas of homicide to high areas of poverty and mental health hospitalizations. It seems that there is a relationship between the amount of homicides that happen, and the rate of poverty and mental health hospitalizations. Looking at this comparison could give us a clue that in order to decrease the amount of deadly crime in Chicago, they must start from the ground up by aiding impoverished neighborhoods.

<div class="row justify-content-sm-center">
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/FINAL_GIF_MENTAL.gif" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/FINAL_GIF_POVERTY.gif" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

## CONCLUSION
In our analysis, we attempted to address two significant questions about the Chicago Crime dataset. First, we wanted to assess which variables or combinations of variables are the most significant when predicting if an arrest occurred in the year 2016. After extensive modeling procedure, we determined that the significant regressors for predicting whether an arrest is made are as follows: community area, latitude + longitude + community area, day + domestic + latitude. We were limited largely to a logistic regression given the binary nature of the arrest variable, but alternate methods of predicting a binary dependent random variable might work better. Throughout this data, we operated with the expectation we likely would be able to model the arrest status of a crime given the other variables and the results do reflect that assumption. Predictive arrest models are essential tools for law enforcement agencies and policymakers in crime prediction and prevention. Our model aims to achieve the same, identifying that geographical variables such as latitude, longitude and community area are amongst the most effective at predicting whether an arrest should be made, can consequently help police departments allocate resources more effectively and take proactive measures rather than reactionary ones to prevent criminal activity and ultimately lead to more efficient and effective law enforcement, reduced crimes rates, and increased public safety.

After analyzing the arrest statistics, our group wanted to examine how other social departments in Chicago are influencing the long-standing crime problems within the community. Therefore, we asked the following question to further our investigation: Is there a connection between areas of high mental health hospitalizations and/or poverty when dealing with areas of high violent crime in Chicago? In the analysis that we conducted, we managed to verify our assumption and conclude that there is a relationship between the amount of homicides in Chicago, an especially troublesome crime in the city, and the rate of poverty and the rate of mental health hospitalizations that accompany homicides. For instance, we can see from the overlap of the homicide locations with the heatmaps of poverty and mental health hospitalizations that homicide occurs in substantial amounts in the Westside and Southside of Chicago, both of which are also characterized by significant rates of poverty and mental health organizations. The finding offers important new understandings into the intricate interactions between various underlying causes of Chicago’s high homicide rate. It emphasizes the significance of adopting a comprehensive strategy to combat crimes in the city as opposed to a typical strategy that focuses only on law enforcement and punishment. It also highlights the need for laws and initiatives that deal with issues of poverty and mental health as part of a new comprehensive plan hoping to lower homicide rates and make Chicagoans’ lives safer.

The prediction of crime patterns is a complex process that involves various factors, including sociology, economics, history, and geography. While the Chicago dataset provided a reasonably coherent analysis, we believe that certain modifications or additions would have made it more robust. If the dataset included more variables such as the presence of witnesses, severity of the crime, and the previous criminal history of offenders, our predictive model for arrests could have been greatly improved. Further research on the topic of arrests could expand to different timeframes and locations, with a focus on how effective the model is at predicting crimes that occurred outside of 2016. Additionally, the inclusion of certain variables or modifications could improve the model since the social context varies from year to year. Similar analyses could be conducted in other major cities with persistent crime issues, such as New York and Los Angeles. Comparisons could also be made among different states and coasts in the future.