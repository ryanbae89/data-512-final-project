# Final Project Plan - Flight Delays and Twitter Sentiment

Author: Ryan Bae

University of Washington DATA 512 Fall 2018

Due Date: December 5th, 2018

## Introduction

Every day, more than a million people take to the skies to travel in the United States. Far removed from the golden age of flying in the 1960s, air travel today mainly conjures up memories of flight delays, rude customers, and long lies at the airport.

Delays are an unavoidable part of flying. Many delays occur due to weather and other events outside anyone's control, while others occur because of mismanagement by airlines/airports or the flight crew. Quantifying this would be an interesting exercise; which airlines are most commonly delayed? Which airports? Are certain routes most susceptible to delays? What about cancellations?   

Regardless of the reasons for delays, passengers often vent their frustration of air travel on various social media mediums. One popular such medium is Twitter. While this is a popular pastime for many people, one can wonder, how realistic are such complaints? Are the negative (or positive) sentiments about an airline inline with reality?  

The goal of this project is to characterize causes of flight delays in the United States and identify most commonly delayed airlines, flights, airports, and routes in 2015. Also part of the analysis is to also identify if other characteristics of a flight is correlated with delays, such as aircraft type or number of runways in an airport.

Second part of this project is an analysis of the Twitter sentiment of various airlines in the month of February 2015. Positive and negative sentiments are to be characterized for each airline and see if they match with observed delays in the same period of time.

## Data

The data comes from several different sources. First is the flight delays data from US Department of Transportation. This dataset contains all the domestic flights in the United States with major carriers. It also contains tables for US airports and airlines. Second is the airline Twitter US airlines sentiment dataset, which contains tweets about airlines and the tweet's sentiment in February 2015. This dataset originally comes from data collection website called figure eight, and is listed on Kaggle as well.

### 2015 Flight Delays and Cancellations Data

This dataset comes from [United States Department of Transportation's Bureau of Transportation Statistics](https://www.bts.gov/) and is listed as a dataset on [Kaggle](https://www.kaggle.com/usdot/flight-delays). The dataset contains flight information of all the domestic flights from 2015 of large airlines. It also contains information about each airline and the airports in the United States. The 3 tables have the following schema:

1. **flights.csv**

This table contains 31 columns describing all domestic flights in the US by large carriers in 2015.

*Note that this table is too large to be uploaded on Git, so a 5% sampled table called `flights_sampled.csv` is included in the repo instead.*

| Column | Datatype | Description |
|-----|------------|----|
| YEAR | int | Year of the Flight Trip |
| MONTH | int | Month of the Flight Trip |
| DAY | int | Day of the Flight Trip |
| DAY_OF_WEEK | int | Day of week of the Flight Trip |
| AIRLINE | string | Airline Identifier |
| FLIGHT_NUMBER | int | Flight Identifier |
| TAIL_NUMBER | string | Aircraft Identifier |
| ORIGIN_AIRPORT | string | Starting Airport |
| DESTINATION_AIRPORT | string | Destination Airport |
| SCHEDULED_DEPARTURE | int | Planned Departure Time |
| DEPARTURE_TIME | int | WHEEL_OFF TAXI_OUT |
| DEPARTURE_DELAY | int | Total Delay on Departure |
| TAXI_OUT | int | The time duration elapsed between departure from the origin airport gate and wheels off |
| WHEELS_OFF | int | The time point that the aircraft's wheels leave the ground |
| SCHEDULED_TIME | int | Planned time amount needed for the flight trip |
| ELAPSED_TIME | int | AIR_TIME+TAXI_IN+TAXI_OUT |
| AIR_TIME | int | The time duration between wheels_off and wheels_on time |
| DISTANCE | int | Distance between two airports |
| WHEELS_ON | int | The time point that the aircraft's wheels touch on the ground |
| TAXI_IN | int | The time duration elapsed between wheels-on and gate arrival at the destination airport |
| SCHEDULED_ARRIVAL | int | Planned arrival time |
| ARRIVAL_TIME | int | WHEELS_ON+TAXI_IN |
| ARRIVAL_DELAY | int | ARRIVAL_TIME-SCHEDULED_ARRIVAL |
| DIVERTED | int | Aircraft landed on airport that out of schedule |
| CANCELLED | int | Flight Cancelled (1 = cancelled) |
| CANCELLATION_REASON | int | Reason for Cancellation of flight: A - Airline/Carrier; B - Weather; C - National Air System; D - Security |
| AIR_SYSTEM_DELAY | int | Delay caused by air system |
| SECURITY_DELAY | int | Delay caused by security |
| AIRLINE_DELAY | int | Delay caused by the airline |
| LATE_AIRCRAFT_DELAY | int | Delay caused by aircraft |
| WEATHER_DELAY | int | Delay caused by weather |

2. **airports.csv**

This table contains 7 columns describing location of all the airports in the United States.

| Column | Datatype | Description |
|-----|------------|----|
|IATA_CODE  | string | Location Identifier |
|AIRPORT    | string | Airport's Name |
|CITY       | string | |
|STATE      | string | |
|COUNTRY    | string | Country Name of the Airport |
|LATITUDE   | float | Latitude of the Airport |
|LONGITUDE  | float | Longitude of the Airport |

3. **airlines.csv**

This table contains name of the airline and the airline identifier code for all large US carriers.

| Column | Datatype | Description |
|-----|------------|----|
| IATA_CODE   | string | Airline Identifier |
| AIRLINE     | string | Airline's Name |

This dataset is released under [CC0: Public Domain](https://creativecommons.org/publicdomain/zero/1.0/).

### Twitter US Airlines Sentiment Data

This dataset originally comes from website called [figure eight](https://www.figure-eight.com/data-for-everyone/) under Airline Twitter Sentiment, and is listed on [Kaggle](https://www.kaggle.com/crowdflower/twitter-airline-sentiment) as well.

1. **tweets.csv**

This table contains tweets from February 2015 that are about airlines and analyzed sentiment, as well as the reason for the negative sentiment.

| Column | Datatype | Description |
|-----|------------|----|
| tweet_id | int | unique ID of the tweet |
| airline_sentiment | string | sentiment of the tweet about the airline |
| airline_sentiment_confidence | float | tweet sentiment confidence |
| negativereason | string | reason for negative review |
| negativereason_confidence | float | tweet negative review reason confidence |
| airline | string | name of airline |
| airline_sentiment_gold |  |
| name | string | username of the tweeter |
| negativereason_gold |  |
| retweet_count | int | number of retweets |
| text | string | text of the tweet |
| tweet_coord | float | lat long location of the tweet |
| tweet_created | timestamp | timestamp of the tweet |
| tweet_location | string | city or state of the tweet's location |
| user_timezone | string | timezone of the tweet |

This dataset is released under [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/) license.

### Data Limitations

There are several limitations with the dataset. First, the tweets are only over a 1 month span, while the flights dataset is over an entire year. There are likely seasonal aspect to both the flights and tweets datasets that are not going to be captured with this analysis. There may also be a lagging effect of people's complaints about an airline on Twitter and their experiences with the airline, which may not be fully captured by this analysis either.

In addition, some important characteristics about the airport are also missing. They can include things like number of flights and human traffic for an given airport, number of runways, etc that may all contribute to number of delayed flights. Finally, weather information at the time of each flight are also missing, which plays a significant role in determining the number of delays.

## Analytical Methods

The first portion of the analysis will contain both simple statistical visualization and classical machine learning analysis, trying to predict the most important features of a delayed flight. First is to perform simple aggregations to find average minutes of delays for each airline and airport and visualizing the results. As for the machine learning aspect of the project, there are several approaches including a simple binary or multi-class classification of a flight being delayed or not, and a regression of number of minutes delayed. Then the regression coefficients are to be analyzed to determine most important factors in flight delays.

The second portion of the analysis will be simple statistical analysis to find the most negative and positive airlines from Twitter sentiments and comparing with most and least delayed airlines from 2015 flights for the month of February in 2015. This analysis may be further categorized to the reasoning of the negative sentiment to provide additional granularity filtering to the results.  

## Research Questions

While there are many analysis of which airlines or airports are likely to have delays, they are not at the level of granularity that this project aims to provide. For example, the `flights.csv` table contains data about minutes of delays that is broken down into specific delay types. And comparison of Twitter sentiment for an airline with its delay performance at such granular level has not been done before.

### Hypothesis

This project is more of an open exploratory project, trying to find interesting patterns in both the delayed flights dataset and the Twitter dataset. There ism however, one hypothesis to be tested:

> There is statistically significant correlation between an airline's proportion of delayed flights and it's Twitter sentiment.

### Flight Delays Analysis

1. Which airlines/airports have the most number of delays when adjusted for number of flights?
2. Which cities and regions have the most number of delays when adjusted for number of flights?
3. Are there any seasonal or temporal patters to the likeliness of delays?
4. What is the breakdown of the delayed flights for different types of delays, and how do they differ for each airline and airport?

### Twitter Sentiment Analysis

1. What airlines have the most negative/positive sentiments? What about in relations to specific reasons for negative sentiments?
2. Do the above airlines match with most and least delayed airlines from the first analysis? What about when looking at specific subset of delays?

## Related Work

The US Bureau of Transportation Statistics publishes the flight delays and cancellations data annually. In addition with data publication, they perform some basic analysis at the aggregate level for the air transportation industry as a whole, which is posted on their website [here](https://www.bts.gov/topics/airlines-and-airports-0). They also have a dashboard which users can query for specific airlines and airports for their on-time statistics in the same website. However, none of these analyses contain any kind of comparison with sentiment on social media.

## Methods

The analysis falls into 2 board verticals. First is the characterization of flight delays, and second is the comparison between airline on-time performance and Twitter sentiment.

The end user for this analysis is meant to be an airline passenger, airline worker/pilot, or a general aviation enthusiast with little technicial backgroud. Therefore, one important human-centered consideration that went into communicating the results from this analysis is to keep things simple and clear. Therefore, the analysis focuses more on visualization and simple statistical modeling rather than complex machine learning models. Various plots and aggregated tables are used to answer the posed research questions and communicate findings from the analysis to a non-technical audience.  

### 1. Characterization of flight delays

The goal of this analysis is to characterize the flight delays in the dataset. Three main research questions are asked:

* What causes flight delays? What proportion is due to airlines, air traffic control, weather, security, etc?


* Are there any seasonal/temporal patterns to flight delays?


* Which airlines/airports/routes are most delayed? Least delayed?


When looking at on-time metrics, **average minutes of delay per flight** is used. To calculate this, the dataset is filtered to desired flights and grouped into airlines/airports/routes/timeframe. It is important to make sure the correct delay time is used for each comparison. The following are used in the analysis:

| Group by metric | Delay type used                 |
|-----------------|---------------------------------|
| Airline         | Arrival Delay, Airline Delay    |
| Airport/Route   | Arrival Delay, Air System Delay |
| Timeframe       | All types of delays             |

Arrival delay is the overall delay of the flight (arrival time - scheduled arrival time), and is a measure of what the passenger experiences. It is used in all characterizations of flight delays in this analysis. Airline delay is delay specifically due to the airline, and is used only when comparing airline specific performances. Finally, air system delay, which is delay due to air traffic control and airport operations, is used only when comparing airport/route comparisons. For these analysis, only routes with at least 100 flights per year are considered.  

For studying time aspect nature of delays, a 7-day running average of various types of delays are plotted for interpretation and visualization. 7-day running average is a good balance between reducing noise using a meaningful and interpretable window (there's variations in flights during each day of a week) and using a timeframe that is too large. Also a weekly average is more interpretable and simpler to understand for the end user of this analysis.

### 2. Airline on-time performance and passenger Tweet sentiment comparison

For this portion of the analysis, the following research questions are asked:

* Which airlines have most negative/positive sentiment on Twitter? How do these sentiments compare with their on-time performance?


* What does the above analysis look like when just looking at Tweets that complain about late flights?

Sentiment for each Tweet are already classified in the `Tweets.csv` dataset. Simple groupby aggregations are used to calculate sentiment for each airline using an industry popular metric called **Net Promoter Score**, or NPS. NPS is calculated using the following formula:

$$NPS = \% \space positive Tweets - \% \space negative Tweets$$

NPS is not a perfect way of aggregating sentiments (it usually relies on a 5 point scale that is not linear). But because of the human-centered considerations of the non-technical audience, it was chosen for it's simplicity and ability to quantify sentiment in a single number.  

For the second research question, the Twitter data is further filtered to those classified as `late flight` for `negativereason` column. This reduces the Tweets down to just those that complained negatively about an airline being late. And only flights that are late due to the fault of the airline are considered. These flights are defined as being more than an hour late with over half of the delay time due to the fault of the airline (`AIRLINE_DELAY` > `ARRIVAL_DELAY` / 2).

## Findings

The findings from this analysis gave some interesting insights into what causes flight delays and how airlines and airports performed in 2015.

### 1. Characterization of flight delays

* **What causes flight delays? What proportion is due to airlines, air traffic control, weather, security, etc?**

![alt text](https://github.com/ryanbae89/data-512-final-project/blob/master/images/delay_type.PNG?raw=true "Delays types")

By far the most common type of delay is what's called **late aircraft delay**, which is delay due to the previous aircraft being late. But this can be for any of the other reasons, and doesn't tell much about the real causes. Discarding these delays, the rest of the delays are dominated by airline delays and air system delays. Weather delays only accounted for less than 5% of all delayed minutes in 2015, and security delays accounting for mere 0.13%.

* **Are there any seasonal/temporal patterns to flight delays?**

The 7-day time averages of all types of delays are shown in plot below.

![alt text](https://github.com/ryanbae89/data-512-final-project/blob/master/images/delays_overtime.png?raw=true "Delays over time")

Most types of delays actually remain fairly constant throughout the year. The overall delay, `ARRIVAL_DELAY` column does show, however, seasonal peaks. They correspond with seasonal peaks in the late aircraft delay. What about day of the week? Do average flight delays vary between days of the week?

![alt text](https://github.com/ryanbae89/data-512-final-project/blob/master/images/day_of_week.png?raw=true)

As it turns out, weekends, especially Saturdays, are the worst days to fly in terms of overall delay.

* **Which airlines/airports/routes are most delayed? Least delayed?**

Here the most/least delayed routes are shown. Air traffic delay, or the `AIR_SYSTEM_DELAY` column, is aggregated here and averaged per flight. The table below shows the top 10 most delayed routes in 2015 using this metric.

![alt text](https://github.com/ryanbae89/data-512-final-project/blob/master/images/most_delayed_routes.PNG?raw=true)

It's quite remarkable to see Newark airport in so many of the destinations. San Francisco airport tops the list as well. These are all routes that fly into very busy airports, with average air system delay of over 45 minutes! Now what about the least delayed routes in 2015?

![alt text](https://github.com/ryanbae89/data-512-final-project/blob/master/images/least_delayed_routes.PNG?raw=true)

Again, there is remarkable similarities between the top 10 least delayed routes, with all the destinations being in Hawaii. In fact, most flights are short hops between Hawaiian Islands.

Now let's look at airlines:

![alt text](https://github.com/ryanbae89/data-512-final-project/blob/master/images/airline_clustering.png?raw=true)

This shows the airline delay per flight on the y-axis and arrival delay per flight on the x-axis. This shows that airlines vary in their overall delays and delays due to their fault. For example, Spirit airlines are late often, but usually not due to their fault. Delta airlines, on the other hand, are on time more often, but when they are late it is more likely to be their fault than other airlines. The plot also shows that Alaska and Virgin airlines are the least delayed both overall and due to their own fault.

### 2. Airline on-time performance and passenger Tweet sentiment comparison

First, what do people on Twitter complain about the most when it comes to negative comments about airlines?

![alt text](https://github.com/ryanbae89/data-512-final-project/blob/master/images/negative_reasons.png?raw=true)

The most common complaint is customer service. However, late flights comes in at second place, followed by ambiguous complaints and cancelled flights, lost luggage, etc. So late flights are a common source of complaints for people on Twitter.

* **Which airlines have most negative/positive sentiment on Twitter? How do these sentiments compare with their on-time performance?**

![alt text](https://github.com/ryanbae89/data-512-final-project/blob/master/images/twitter_sentiment_arrival_delay.png?raw=true)

The above plot shows little correlation between average minutes of delay per flight (overall) and airline sentiment on Twitter for February 2015. While the correlation coefficient is negative, there is no statistical significance. Part of this is due to the small sample size (6) of airlines represented.

![alt text](https://github.com/ryanbae89/data-512-final-project/blob/master/images/twitter_sentiment_airline_delay.png?raw=true)

The above plot shows little correlation between average minutes of delay per flight (airline) and airline sentiment on Twitter for February 2015. Again, while the correlation coefficient is negative, there is no statistical significance, partly due to the small sample size.

* **What does the above analysis look like when just looking at Tweets that complain about late flights?**

![alt text](https://github.com/ryanbae89/data-512-final-project/blob/master/images/twitter_sentiment2.png?raw=true)

To answer this question, the number of late flights that are the fault of the airline are plotted against the number of Tweets for that airline complaining about late flights. The above plot shows no clear correlation between number of late flights and number of Tweets about late flights for the 6 airlines shown.

## Discussions/Implications

So what does all this mean? Let's start with flight delays characterization. First is that while there is seasonal variation in average overall delay time per flight, it is mainly due to the aircraft arriving late. The other delay factors such as weather, airline, and air traffic are fairly consistent throughout the year. This means the increase in delays during holidays, for example, is largely exacerbated by the increase in number of flights. For what it's worth, airlines and air systems operate with similar efficiencies with increased loads, an interesting finding.

Regarding airports and routes, it is good to avoid large, busy airports for good on-time performance. In fact, of the three major airports in New York City, two of them made the top 10 list and Newark airport made the list 8 times! It is suggested to avoid these airports when possible. And when picking days to fly, avoid the weekends, especially Saturdays, as it is the most delayed day of the week.

As for the airlines, the plot shows some interesting clusters of airlines. Most airlines, such as Southwest, US Airways, American Eagle, JetBlue, etc have average overall on-time performance with average airline delay performance. Two airlines that most people have good opinions of, Virgin and Alaska, were the most on-time and least delayed due to their own fault. And the ultra low-cost airlines such as Frontier and Spirit airlines had worst overall on-time performance, but per flight airline delays for these airlines were quite good. Some airlines, such as Delta and Hawaiian, had good overall on-time performance that could be improved by doing better on their airline delays. These self-clustering of airlines on the airline comparison visualization was an interesting finding that reflects the different routes, operations, and customers that they serve in the US.

Now for the Twitter analysis, all findings came negative. There were no statistically significant correlations between airline Twitter sentiment and their on-time performance. Even when only the Tweets that complained about late flights are plotted alongside actual number of late flights, the correlation was poor. However, this comes with few caveats; the number of airlines (the sample size) is very small, at only 6. Also, only a single month of the year was considered, and a more clear pattern may emerge when a wider timeframe is considered.

The final plot does have, however, some interesting insights about passenger's sentiments about specific airlines. Virgin American airlines, an airline with very few late flights and delays, does have fewest number of complaints about late flights on Twitter. US Airways and United, however, has proportionally larger number of complaints about late flights than airlines like Delta and American. And surprisingly, Southwest airlines, while having the most late flights that are their fault, received second least number of late flight complaints on Twitter. These data points suggest that people's complaints and sentiments about airlines are not very reflective of reality, at least when it comes to their on-time performance except in exceptional cases, such as Virgin airlines.

## Conclusions

Overall, this analysis was an interesting exercise into the world of commercial aviation and Twitter. There were some interesting insights about causes of flight delays and best/worst performing airlines and airports. While most of the findings confirmed suspicions, such as large airports like Newark being the worst at air system delays, others were more surprising. They include variations between airlines in their overall on-time performance and airline delay performance. The Twitter sentiment analysis, while ultimately a negative result, comes with a caveat of small sample size of airlines and timeframe. This means further work in this area can provide more conclusive insights. Finally, the over complaints of airlines like United and under complaints of airlines like Southwest was an interesting insight, although not too unexpected based on my own anecdotal experience. My hope is that both those who work in commercial aviation and passengers can derive useful insights and knowledge from this analysis.

## References

[1] [Flight delays and cancellation dataset on Kaggle](https://www.kaggle.com/usdot/flight-delays)

[2] [Bureau of Transportation Statistics](https://www.bts.gov/)

[3] [Bureau of Transportation Statistics Airlines and Airports](https://www.bts.gov/topics/airlines-and-airports-0)

[4] [Twitter airline sentiment dataset on Kaggle](https://www.kaggle.com/crowdflower/twitter-airline-sentiment)

[5] [Figure Eight website](https://www.figure-eight.com/data-for-everyone/)

[6] University of Washington DATA 512 Fall 2018 Course Website, https://wiki.communitydata.cc/Human_Centered_Data_Science_(Fall_2018)
