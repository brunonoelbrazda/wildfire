# Overview
The capstone was a 5-week project completed towards the end of my data science immersive course where we were given the freedom to address a real-world issue by applying our newly learnt skills.
# Contents
### Introduction and Goals
### The Data
### Exploratory Data Analysis
### Feature Engineering
### Clustering
### Modelling
### Further Steps
### Conclusions
### Key Learnings/Limitations

# Introduction and Goals

According to the World Health Organisation, Wildfires can disrupt transportation, communications, power and gas services, and water supply. They also lead to a deterioration of the air quality, and loss of property, crops, resources, animals and people.

The goal for my capstone was to use machine learning to accurately classify wildfires in the United States based on their size, in order to determine what factors contribute the most to Wildfire size. My hypothesis was that using features such as temperature, humidity, time, and fire cause, I would be able to classify them very accurately by size. This could in turn help improve wildfire prevention in regions of the United States that are most at risk. 

# The Data
The primary Dataset used was a Geo-Referenced SQLite database containing 1.88 Million U.S Wildfires spanning 24 years. This Dataset was obtained from Kaggle, where it has been used for Kaggle Competitions. 
A snapshot of the database:
![DataBase](/visuals/database.png)

To supplement this dataset, I used an API called “Virtual Crossing” to collect historic weather data. The weather data I used was temperature and humidity, as these in my mind would influence how large a fire could grow to be.



The Classes are defined as follows:
![Classes](/visuals/fire_classes.png)


# Exploratory Data Analysis
The EDA process highlighted the distribution of fires across the U.S, the seasonality of wildfire Size, as well as low correlations between my features and target variable. This prompted me to apply DBSCAN clustering.


Mean Wildfire Size
![MeanSize](/visuals/mean_size.png)


Wildfires by Count
![Count](/visuals/count.png)


Mean Wildfire Size in Acres Sampled Weekly
![Seasonality](/visuals/seasonality.png)


Insufficient correlations among features
![Correlations](/visuals/correlations.png)

# Feature Engineering:

Feature Engineering focused on extracting duration of the fire from discovery and contained date, extracting the month of the fire as a feature, and dummifying categorical features. The final features that could be used for modelling were:

Longitude, Latitude, Cause of the Fire,
Duration(hours), County, State, Type of Land, Discovery Day of
Year, Contained Day of Year, Temperature, Humidity

Target: Fire Size Class

# Clustering:
DBSCAN Clustering was performed twice, once on more recent fires (2015), and a second time on fires from a decade earlier (2005-2006). By tuning the parameters (eps (which specifies how close points should be to each other to be considered a part of a cluster) and minimum samples), I was able to break the U.S Wildfires into a few clusters, and pinpoint the regions with the highest density of wildfires. The fires within the “purple” cluster in both instances were not taken into account, as their geographic diversity made them difficult to model accurately.

3 Clusters
![3clusters](/visuals/3clusters.png)

5 Clusters
![5clusters](/visuals/5clusters.png)


# Modelling:
Once I had obtained the different clusters, I began fitting different classifiers on the data. On a cluster that spanned the west coast of the United States, with fires in 2005-2006, a preliminary random forest classifier achieved a mean cross validation score of 49% higher than baseline on the test set.
![Model11](/visuals/model1.png)

Feature Importances of the Model. Duration and Longitude were the two most important
![FeatureImportances1](/visuals/feature_importance1.png)

The model performed best when classifying fires of sizes A and B, as illustrated by the confusion Matrix and Precision-Recall curves below.
![ConfusionMatrix1](/visuals/confusion_matrix1.png)
![Precision-RecallCurve1](/visuals/precision_recall1.png)

A similar process was applied to the second, more recent set of fires (2015). To explore different regions of the U.S, here a North-Eastern Cluster was chosen for modelling. A preliminary Random Forest Classifier was used, achieving a score of 0.8, far above baseline (0.61).
![model2](/visuals/model2.png)

Feature Importances were extracted once again, interestingly in this North-Eastern cluster, latitude and temperature were the most important features.
![feature_importances2](/visuals/feature_importances2.png)



ROC and Precision-Recall Curves
![roc2](/visuals/roc2.png)


# Further Steps:

To take his project further I would increase the size of my clusters, and supplement my data set with additional features to improve model accuracies. (income of county, time of day fire started) I would also investigate the how the trends in wildfires changed over time. (SARIMA Models).

# Conclusion:
Overall, using a combination of supervised and unsupervised learning algorithms I was able to achieve my goal of accurately classifying fire size classes. Following DBSCAN clustering and supplementing my data with an API, my random forest classifier performed about 50% higher than baseline. Extracting feature importances I was able to determine factors that contribute the most to the size of a wildfire. For the cluster on the western coast of the United States, these were, in order of importance: duration, longitude, latitude, and humidity. As for the north-eastern cluster, the most important features, in order, were: latitude, temperature, humidity, and longitude. These insights highlight how different geographic regions must prioritise different factors when carrying out wildfire prevention.

# Limitations/Key Learnings:
The primary limitation I encountered was the fact that my DBSCAN algorithm scaled quite poorly, therefore I could only input roughly 30,000 fires. Moreover, finding a suitable API that contained historic weather data and could be queried using longitude/latitude was an arduous process that involved a lot of trial and error. 
My key learnings from the project were time-management, and how this relates to planning ahead. I had previously thought that modelling would be the most challenging and time consuming part of the project, and had allocated more time to it than to the data collection and cleaning. Finding an appropriate API, making requests, and transforming the data took far longer than I had anticipated. Modelling, on the other hand, was in fact a matter of reusing some generic code I had written in the past, and turned out to take far less time than I had imagined. I will take this as a lesson as I move forward in my Data Science career.

