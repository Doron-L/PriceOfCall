# PriceOfCall


## Introduction
During my fellowship at Insight Data Science I took on a consulting project for Upcall,
a B2B telemarketing and sales company. Upcall wanted to get insights from their data. It made sense to 
firs try to optimize the time when a specific number is being called in order to increase pick up rate (probability 
that someone answers the phone).


## The Data 
Upcall sent to me about 320k calls with the following features: the call id, duration, and time and date, the number out of which the call was made, the call country destination, area code, company name, the industry of the business, campaign type, and the title of the person getting the call. 

For our objective, we need a binary target: 0 and 1, for a non-picked-up and a picked-up call, respectively. However, in the target, there were 17 different categories:

**Completed**                  **Not interested**  
0 = completed  &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;   5 = hungup  
11 = completed_fu_call  &emsp;&emsp;&emsp;&nbsp;       6 = not_interested  
12 = completed_fu_email &emsp;&emsp;&nbsp;       7 = dnc (do not call)  
13 = completed_fu_inperson &emsp;    9 = wrong_number

**Try again**                  **Failed**  
1 = call_back_later  &emsp;&emsp;&emsp;&emsp;&emsp;          3 = no_answer  
2 = busy  &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&ensp;    8 = no_service  
4 = voicemail   &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;          14 = technical_issues  
16 = alternative_phone &emsp;&emsp;&emsp;        10 =  untried 

**Speaking**                   **Interested**  
15 = speaking   &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;             17 = maybe_interested 

I threw classes 2, 8, 9, 10, 14, as they didn't seem relevant. Classes 3 and 4 were grouped as the non-picked up calls. the rest of the classes were grouped as the picked-up calls. The ratio between the calls that were not picked-up to the ones that were was about 2/1. 

## Cleaning the data

The main cleaning challenge was to deal with many NaNs. One of the main reasons for having NaNs is the fact that Upcall started to collect information about the different features at different times. Thus, features that were started to be collected at later times have many NaNs. My strategy was to remove calls that have NaN's in any of the features that are relevant for the specific analysis.

## Exploratory Data Analysis 

I first looked at the country feature. There were calls to many different countries, but only the US and GB had more than 100 calls each. More specifically, there were ~300k and 400 calls to the US and GB, respectively. Since these calls are further divided to the many other features, I removed GB as well.  

Then I made an exploratory data analysis to see if there are dependencies between the pick-up ratio and the different features. I looked at the dependencies on time (e.g., year, month, day of week, and hour), as this feature is most relevant to our first objective. At a first glace, the human eye could identifies some "trends". For example, by looking at the dependency of the pick-up ratio over the hour, we see that the pick-up ratio is higher at the middle of the working day (except for some increase at 19:00, please see the black curve in the following figure), which makes perfect sence as we call to a business, and most of them are more active in the middle of the day (camparing to early in the morning or late in the evening). However, when we look at this dependency in the three area codes with the highest number of calls, we see that the dependency of the pick-up ratio over time is really different between the different area codes and between them and the general trend. Note that bins with less than 100 calls were removed. 
![](https://github.com/Doron-L/PriceOfCall/blob/master/pickup_ratio_vs_hour_diff_area_codes_png)

A similar insights are found when the pick-up ratio vs. hour (or any other time measure) is ploted for the different categories of another feature. Thus, the conclusion here is that, from these data, it is risky to infer anything from the time tends we see, and that the dependency of the pick-up ratio may depend on more than one or two features and maybe also depend in a more complex way.

## Feature engineering

All the features were categorical, and so it was strighforward to engenir them. For the logistic regression, I 1 hot-code them, while for the random forest, I only replaced the names of the different categories by numbers. The biggest challenge was to engineer the company name featyure. I feature engineered this feature in two different ways. In one way, I took the companies with the highest number of calls calls, made exploratory analysis, and saw if I understand the hour trends. There was no clear different hour trend for these different companies (???show figure???). In the second way, I grouped the different names by domain kowledge. For example, companies with the word restuarant were grouped together, as company names with the word hotel or inn. Then I removed groups with less than 100 calls and used it as one of the ML techniques features. 

## Machine Learning
Even though the dependencies between the pick-up ratio and the various features where not clear in the exploratory data analysis, I applied machine learning classification techniques to perhaps identify more complex dependencies that were missed using the exploratory data analysis, as the dependcy is on a combination of many of the features. 

I started with a logistic regression and then tried also random forest. None of them gave an accuracy (which is the releant metric in this case, as we care about...) that is significantly higher than the one obtained by predicting all calls not to be picked-up, which is the larger class.


??? number the ones below and give a few explanations.

In order to fight the large number of feature, I applied a dimension reduction methon, principal component analysis. Since I wanted to still be able to interpret the features, I applied the PCA in blocks, where I reduced the dimension of features of the same kind together. For example, I took all the area codes categories and reduced their dimentions. In this way I reduced the number of features and kept their basic meaning.  

In order to fight to low amount of data stayed after the cleaning procedures, I classified the data by taking a lower number of features, where I cleaned the data only by the features I took. Since I cleaned the data by less features, more data was left. However, the accuracy was still not significantly higher than the one obtained by predicting that all the labels will be 0.


## ???

| Feature        | \# of categories| 
| ---------------|:--------------:| 
| from_numb      | 258            | 
| campaign type  | 5              | 
| area codes     | 294            |  
| titles         | 15993          |  
| industries     | 28             |  


The amount of data  
Data size: 320k calls  
Cleaned data:  <30-40k calls depending on the exact cleaning procedure  
Picked-up: ~ 1/3 of the cleaned calls  

One of the reasons we are left with a vey low number of calls after the cleaning is that Upcall started to collect different features at different times, and so only the most recent calls have information in all their features. 
    
In addition to the lack of enough data, the features didn't seem to have a lot of predictive power, as non helped to predict with accuracy above the one obtained by...??? 

I tried to speculate why two features that initially I thought will have information didn't have.
The area code may have two problems. First, it may not represent the real physical location of the business,
as phone numbers can be kept even if a person or a business move. Second, the pick-up ratio in 
an area code is an average over the pick-up ratio over various buisinesses. Each business may
operate at different parts of the day. Thus, te dependency on the business ... may be more relevant than 
the area code.

The phone number out of which the call was made was initially also a feature that I suspected that should
have some predicting power. I speculate that this is not very true since people first care if the 
number is identified. Then they care if the number os flagged by some app as spam, etc. If the number is
neither identified nor flagged, they care less what is the exact number.

It turns out that it is difficult...

## ???Pivoting??? the first objective - going back to exploratory data analysis

In order to look for valuable objective that can be found from this data set, I left the time features and looked at other ones. I looked at the industry and campaign type, as they have the fewest number of categories.  Thus, I chose the two features with the fewest number of categories, and cleaning the data only if there were NaNs in their elements. Leaving more data, and less features. Thus, leaving the emphasize over time and making exploratory data analysis of the pick-up ratio over these two features.

We can see that there is a significant difference in the pick-up ratio between the different industries, as well as between different campaign types.
![](https://github.com/Doron-L/PriceOfCall/blob/master/pickup_ratio_vs_industry_png)
![](https://github.com/Doron-L/PriceOfCall/blob/master/pickup_ratio_vs_campaign_type_png)
The ... black lines over each bar are the 1 sigma uncertainty... obtained by...

## The deliverable
The relative pick-up ratio can be interpreted as the relative effort necessitated to get to some pick-up ratio. The two dependencies (on the industry and campaign type) can be combined and give the relative effort needed to be invested in each project dependign on these two features. The company can use this price/effort scheme in two different ways. They can either reject high-effort project, or to have a more project-specific price scheme that takes into consideration the effort needed to be invested. In the following figure, I plotted the relative price/effort level needed for each project depanding on the different industry and campaign type. Due to the sparse nature of the data, I linearly interpolated and smoothed the relative price values.
![](https://github.com/Doron-L/PriceOfCall/blob/master/price_smoothed_vs_industry_n_campaign_type_png)

## Further work that can be done

For having a price scheme that is more complex (depends on more features) and more accurate, there is a need for more data. In addition, more features with relevant information should be added. These features can be engineered from existing ones (e.g., text mining the title feature) or adding new ones (e.g., the size of the company and the sub-industry of the business).


