# PriceOfCall

Introduction
During my fellowship at Insight Data Science I took on a two weeks consulting project for Upcall,
a B2B telemarketing and sales company. Upcall wanted to get insights from their data. It made sense to 
optimize the time when a specific number is being called in order to increase pick up rate (probability 
that someone answers the phone).


The Problem



Data Acquisition
A first data set was givven by Upcall. After I examined the data... predictive power for the purpose of...
I asked for features that may help. There where two more rounds where they sent me two more sets of features.


The Data (features)

1st data batch     2nd data batch
id                 Company name
duration 
called_at          3rd data batch
from_numb          Industry
area_code          Campaign type                     
country            Title

# 2nd data batch
# Company name

# 3rd data batch
# Industry
# Campaign type
# Title



The Data (targets)
# completed 
0 = completed  
11 = completed_fu_call  
12 = completed_fu_email  
13 = completed_fu_inperson  

# try again  
1 = call_back_later  
2 = busy  
4 = voicemail  
16 = alternative_phone  

# speaking  
15 = speaking  

# not interested  
5 = hungup  
6 = not_interested  
7 = dnc (do not call)  
9 = wrong_number

# failed  
3 = no_answer  
8 = no_service  
14 = technical_issues 
10 =  untried 

# interested  
17 = maybe_interested 


I first made an exploratory data analysis to see if there are dependencies between the pick-up ratio
and the features. It looked like the pick-up rate depends on.... However, I suspected that this dependecy may
be some averaging over.... Thus, I plotted the pick-up rate vs. hour for the 2 area codes with the highest 
number of calls.

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

Feature                                 # of categories 
Number of from_numb:                           258
Number of campaign type:                         5
Number of area codes:                          294
Number of companies:                         15993
Number of titles:                             1797
Number of industries:                           28



The amount of data data:
Data size: 320k calls
Cleaned data:  <40k calls
Picked-up: < 15k calls
    
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



Objective that can met and is very valuable for the company.
Choosing the two features with the fewest number of categories, and cleaning the data 
only if there were NaNs in their elements. Leaving more data, and less features. Thus, leaving the emphsize over 
time and making exploratory data analysis of the pick-up ratio over these two features.


![](/price_smoothed_vs_industry_n_campaign_type_png.png)

Still can be done:
text mining of the titles feature, in a similar way to the one I have done for companies.
I thought that the companies feature should have more information, so I text mind it first.
