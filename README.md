# PriceOfCall


## Introduction
During my fellowship at Insight Data Science I took on a two weeks consulting project for Upcall,
a B2B telemarketing and sales company. Upcall wanted to get insights from their data. It made sense to 
optimize the time when a specific number is being called in order to increase pick up rate (probability 
that someone answers the phone).


## The Problem



## Data Acquisition
A first data set was givven by Upcall. After I examined the data... predictive power for the purpose of...
I asked for features that may help. There where two more rounds where they sent me two more sets of features.


### The Data (features)

**1st data batch** &emsp;&emsp;   **2nd data batch**  
id &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&ensp;&nbsp;  Company name  
duration  
called_at &emsp;&emsp;&emsp;&emsp;&ensp;&nbsp;         **3rd data batch**  
from_numb &emsp;&emsp;&emsp;&nbsp;&nbsp;         Industry  
area_code &emsp;&emsp;&emsp;&emsp;         Campaign type                     
country   &emsp;&emsp;&emsp;&emsp;&emsp;&nbsp;         Title  

??? meaning of the features

### The Data (targets)

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

??? meaning of the targets

I threw classes 2, 8, 9, 10, 14, as they didn't seem relevant. Classes 3 and 4 were grouped as the non-picked up calls. the rest of the classes were grouped as the picked-up calls.  

## Exploratory Data Analysis 
I first looked at the country feature. There were calls to many different countries, but only the US and GB had more than 100 calls each. More specifically, after cleaning ..., there were 299163 and 381 calls to the US and GB, respectively. Since these calls are further divided to the many other features, I removed GB as well. 

Then I made an exploratory data analysis to see if there are dependencies between the pick-up ratio
and the features. I looked at the dependencies on time (e.g., year, month, day of week, and hour). At a first glace, the human eye could identifies "trend". For example, by looking at the dependency of the pick-up ratio over the hour, we see that the pick-up ratio is higher at the middle of the working day (except for some increase at 19:00, please see the black curve in the following figure), which makes perfect sence ads we call to a business. However, when we look at this dependency in the three area codes with the highest number of calls, we see that the dependency of the pick-up ratio over time is really different between the different area codes and between them and the general trend. Note that bins with less than 100 calls were removed. 
![](https://github.com/Doron-L/PriceOfCall/blob/master/pickup_ratio_vs_hour_diff_area_codes_png)
A similar insights are found when the pick-up ratio vs. hour (or any other time measure) is ploted for the different categories of another feature. Thus, the conclusion here is that it is risky to infer ??? from an exploratory analysis of two or three features and that the dependency of the pick-up ratio may depend on more than one or two features and maybe also depend in a more complex way.



It looked like the pick-up rate depends on.... However, I suspected that this dependecy may
be some averaging over.... Thus, I plotted the pick-up rate vs. hour for the 2 area codes with the highest 
number of calls.


??? but does it depend on the area code?

bins with less than 100 calls were removed.

## Machine Learning
Even though things where not clear in the exploratory data analysis, I applied machine learning classification
techniques to perhaps identify more complex dependencies that were missed using the exploratory data analysis,
as the depend on a combination of many of the features.


Text mining of the companies names. where I made exploratory data analysis for companies with many calls, and looked 
at the pick-up rate vs.  hour, and I grouped companies that look similar (by some word) and inserted it as a feature
for the ML techniques I used.


Feature importance.

Applying ML classification techniques: Logistic regression, random forest (hot-coding and not hot-coding).

Dimension reduction

Testing feature by feature to have more data.

It turns out that it is difficult...

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

One of the reasons we are left with a vey low number of calls after the cleaning is that the company started to collect different features at different times, amd so only the most recent calls have information in all their features. 
    
In addition to the lack of enough data, the features didn't seem to have a lot of predicted power.
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

## Pivoting the first objective - going back to exploratory data analysis

Objective that can met and is very valuable for the company.
Choosing the two features with the fewest number of categories, and cleaning the data 
only if there were NaNs in their elements. Leaving more data, and less features. Thus, leaving the emphsize over 
time and making exploratory data analysis of the pick-up ratio over these two features.
![](https://github.com/Doron-L/PriceOfCall/blob/master/pickup_ratio_vs_industry_png)
![](https://github.com/Doron-L/PriceOfCall/blob/master/pickup_ratio_vs_campaign_type_png)



![](https://github.com/Doron-L/PriceOfCall/blob/master/price_smoothed_vs_industry_n_campaign_type_png)

Still can be done:
more features: company size, 

text mining of the titles feature, in a similar way to the one I have done for companies.
I thought that the companies feature should have more information, so I text mind it first.
