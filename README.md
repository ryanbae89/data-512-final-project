# Final Project Plan - Flight Delays and Twitter Sentiment

Author: Ryan Bae

Due Date: December 5th, 2018

## Introduction

Every day, more than a million people take to the skies to travel in the United States. Far removed from the golden age of flying in the 1960s, air travel today mainly conjures up memories of flight delays, rude customers, and long lies at the airport.

Delays are an unavoidable part of flying. Many delays occur due to weather and other events outside anyone's control, while others occur because of mismanagement by airlines/airports or the flight crew. Quantifying this would be an interesting exercise; which airlines are most commonly delayed? Which airports? Are certain routes most susceptible to delays? What about cancellations?   

Regardless of the reasons for delays, passengers often vent their frustration of air travel on various social media mediums. One popular such medium is Twitter. While this is a popular pastime for many people, one can wonder, how realistic are such complaints? Are the negative (or positive) sentiments about an airline inline with reality?  

The goal of this project is to characterize causes of flight delays in the United States and identify most commonly delayed airlines, flights, airports, and routes in 2015. Also part of the analysis is to also identify if other characteristics of a flight is correlated with delays, such as aircraft type or number of runways in an airport.

Second part of this project is an analysis of the Twitter sentiment of various airlines in the month of February 2015. Positive and negative sentiments are to be characterized for each airline and see if they match with observed delays in the same period of time.

## Data

The data comes from several different sources. First is the flight delays data from US Department of Transportation. This dataset contains all the domestic flights in the United States with major carriers. It also contains tables for US airports and airlines. Second is the airline Twitter US airlines sentiment dataset, which contains tweets about airlines and the tweet's sentiment in February 2015.

### 2015 Flight Delays and Cancellations Data

This dataset comes from [United States Department of Transportation](https://www.transportation.gov/) and is listed as a dataset on [Kaggle](https://www.kaggle.com/usdot/flight-delays). The dataset contains flight information of all the domestic flights from 2015 of large airlines. It also contains information about each airline and the airports in the United States. The 3 tables have the following schema:

1. **flights.csv**

This table contains 31 columns describing all domestic flights in the US by large carriers in 2015.

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

### Twitter US Airlines Sentiment

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

## Research Questions



## References
