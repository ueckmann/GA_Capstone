# Predicting Basketball Games Using Statistics

### Uriel Eckmann

![Lebron Dunk](./assets/lebron_dunk.png)

## Problem Statement

Basketball is an incredible game. It changes from year to year as teams develop new offensive schemes, and players develop incredible skills to get an edge over their opponents. As such, data analytics and data science can help players and coaches develop ways to adjust their gameplan to be unique to every single opponent, as well as help players determine what skills they should be focusing on. Simultaneously, predicting the outcomes of NBA games is a huge business, and the data from individual games can be used to predict the outcomes of upcoming matchups between two teams. So, we're looking to answer 2 questions here.
1. Can we use data to determine what parts of the game are most important?
2. Can we build a useful classification model that can accurately determine the outcomes of games?

In a certain sense, every game is a toss up. Each team has a 50/50 shot of winning the game. That's not necessarily the reality, but that does tell us what we're searching for when it comes to the success of our models - if our models can predict game winners with more the 50% accuracy. 

## Executive Summary

Our goal here is to create an accurate model that will predict game winners using data from previous games as well the game itself after the fact. 
### _Workflow_
#### _Data Collection_
The data for this project was collected from [Basketball-reference.com](https://www.basketball-reference.com/). This is a database of every professional game going back more than 50 years worth of data. Because I wanted to focus on how the modern game is approached, I utilized data from the 2017 - 2018 and 2018 - 2019 seasons. In order to collect this data, [I built three custom webscrapers]("./capstone_data_collection_webscraper.ipynb") designed to be able to collect entire seasons' worth of data at time. This was broken down into 4 parts:
1. The webscraper that collected [regular season standings](https://www.basketball-reference.com/leagues/NBA_2019.html). This webpage has a table of all 30 teams and their standings over the course of the season. This contained valuable information, which helped to build the next webscraper, 
2. The webscraper that collected [individual team schedules](https://www.basketball-reference.com/teams/MIL/2019_games.html). This webpage has a table with all 82 games for a team in any given season. This table provided more valuable information, such as game results, opponents, and the date of each game. This information then helped with the final webscraper,
3. The webscraper that collected [individual player and team statistics](https://www.basketball-reference.com/boxscores/201810170CHO.html) from every game. This included statistics from both teams, and had over 30 different relevant statistics for every individual player who played in that game.
4. The final piece was a function that used all 3 webscrapers to collect data from all 1,230 games of the regular season. These webscrapers can work on any season, all they need is for the user to specify what season they want data from. It is important to note that this function has a built in timer that slows down the webscrape, since basketball reference is providing a free service for us. 

#### _Data Exploration_
After collecting our data, we were able to investigate all of the different statistics and determine which of these will be most useful for our investigations. Our models are going to be using _team_ data, so it's important to recognize how individual player statistics are aggregated for the team, and which statistics will be most important. It should be noted that basketball is a game of points - it doesn't matter if you win by 1 or by 50. As such, that data can interfere with our models, since if one team's point totals are greater than  their opponents, our models will be able to infer who will win with that. We want to avoid all data leakage, so our models predicting games that are already over will highlight the other aspects of the game besides scoring points. This also includes team aggregates that are based on points, such as offensive and defensive rating. Additionally, there was the Box Plus/Minus stat, which only considers individual players, and is not applied to team play. Exploring how best to apply that stat to team overall performance was quite the challenge.

#### _Observations_
After building several models from our data, one thing has become clear - defense may be more important than offense. The phrase "The best defense is a good offense" is very true, because it doesn't matter how many points you score if your opponent outscores you. Our models highlighted how bad defense can be more impactful than a good offense. Our models that utilized data from earlier games also picked up on which teams are the best teams in recent years, giving them more weight as being winning teams. 

#### _Issues_
Building a custom webscrape was extremely arduous, and created a whole number of challenges. Working with the blend of Python and HTML was really interesting, and I had never built a webscraper with such intricate detail like that before. Additionally, the nature of our data made organizing it for modeling quite the challenge. Each row had to be matched up with the proper opponent, and each row had to be repeated- once with the home team first, then again with the away team first. 