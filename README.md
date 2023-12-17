# Premier League Stats/Predictor
This project aims to look at the Premier League, the stats of each team, how they have performed this season, and to predict the rest of the season with said stats.
The Excel file features data taken from FBRef, the current table, a game predictor, and a table prediction. In addition, stats regarding attacking, goalkeeping, passing, and defending for each team are included, as well as a radar chart comparing said metrics. 

# Background
Some inspiration did come from a few areas to complete this project. The visualizations of attacking, goalkeeping, passing, and defending statistics came from Football Manager’s Data Hub feature, which creates a scatter graph of each team in a league and divides it into 4 quadrants to make it easier to see how teams are performing. A picture comparing the two is below.

![image](https://github.com/SBadovych/Premier-League-Stats-and-Predictor/assets/138629334/0816d6ee-1285-4684-9021-77c32639eb26)

![image](https://github.com/SBadovych/Premier-League-Stats-and-Predictor/assets/138629334/a3a92273-8d2b-4152-b36c-73ec412dd5af)

Another source of inspiration came from a YouTube video ([link](https://www.youtube.com/watch?v=ojUBRuBwE7s)) which described how to predict football matches using the Poisson Distribution. 

# Methodology
For to calculate the scorelines seen in the “Game Prediction” sheet, it is done as follows. For simplicity, we will call them Team A and Team B.
Firstly, we must find the attack and defense rating for both teams.
To calculate the attack rating, we must first find the correlation coefficient of a team’s xG, to see if they under or over-perform. Once that is found, the weight of a team’s xG is calculated as follows:
$$\[
\frac{1}{{1 + \text{Correlation Coefficient}}}
\]$$

Once that is found, then we can find the attack rating, which is calculated as follows:
$$\[
1 + \left(\frac{{\text{Goals Scored per Game} - \text{xG per Game}}}{{\text{Goals Scored per Game}}}\right) \times \left(\left(\text{Weight of Goals Scored per Game} \times \left(\frac{{\text{Goals Scored per Game}}}{{\left(\text{League Avg Goals Scored Per Game}\right)/2}}\right)\right) + \left(\text{Weight of xG per Game} \times \left(\frac{{\text{xG per Game}}}{{\left(\text{League Avg xG per Game}\right)/2}}\right)\right)\right)
\]$$

The defense rating follows the same idea with the correlation coefficients and weights, however it will now look at Goals Allowed and xGA (expected Goals Allowed) instead. It looks like this:
$$\[
1 + \left(\frac{{\text{Goals Allowed per Game} - \text{xGA per Game}}}{{\text{Goals Allowed per Game}}}\right) \times \left(\left(\text{Weight of Goals Allowed per Game} \times \left(\frac{\text{Goals Allowed per Game}}{\left(\text{League Avg Goals Scored Per Game}/2\right)}\right)\right) + \left(\text{Weight of xGA per Game} \times \left(\frac{\text{xGA per Game}}{\left(\text{League Avg xG per Game}/2\right)}\right)\right)\right)
\]$$

Finally, the last thing needed is a corrected value for goals scored per game. This is just an equally weighted average between goals scored per game and xG per game.
With these, you can now calculate the score for Team A:
$$\[
\frac{{(\text{Team A Attacking Rating}) \times (\text{Team B Defensive Rating})}}{{\frac{\text{Corrected Goals per Game}}{2}}}
\]$$

Likewise, Team B’s score is found as follows:
$$\[
\frac{{\text{{Team B Attacking Rating}} \times \text{{Team A Defensive Rating}}}}{{\left(\frac{{\text{{Corrected Goals per Game}}}}{2}\right)}}
\]$$

Afterwards, the Poisson Distribution looks at all possible outcomes for the game, with each team scoring from 0-10 goals in any given game using these scores/ratings. The percentage chance to win is self-explanatory. 

# Stats Overview
NOTE: All screenshots and data from this point on were from 12/17/23, or Matchday 17 of the season.

For the goalkeeping statistics:

![image](https://github.com/SBadovych/Premier-League-Stats-and-Predictor/assets/138629334/a3a92273-8d2b-4152-b36c-73ec412dd5af)

For the attacking statistics:

![image](https://github.com/SBadovych/Premier-League-Stats-and-Predictor/assets/138629334/69d57308-8c0e-416d-b80b-266617fefa69)
![image](https://github.com/SBadovych/Premier-League-Stats-and-Predictor/assets/138629334/f5e97a00-f114-4bdd-93d5-d51b2248c8bd)

For the passing stats:

![image](https://github.com/SBadovych/Premier-League-Stats-and-Predictor/assets/138629334/59896b89-8a90-4624-ae18-6c45a82f19e3)

For the defending stats:

![image](https://github.com/SBadovych/Premier-League-Stats-and-Predictor/assets/138629334/24dca703-ecd6-4508-9ded-83a12e24d736)
![image](https://github.com/SBadovych/Premier-League-Stats-and-Predictor/assets/138629334/97711f18-431f-40fb-a16c-d6a4ef42a53b)

# Table Prediction
The table prediction was calculated by taking the table and fixture data from games that have already occurred, and through 1000 simulations ran of the rest of the games. The simulations were calculated using a similar formula to the score calculation in the game predictions.
As of Matchday 17, the table looks like so:

![image](https://github.com/SBadovych/Premier-League-Stats-and-Predictor/assets/138629334/6af3f4d0-8f7c-44b5-b296-84d131fa3618)


# Prediction Results
Although the sample size is still small, the results of the predictions have been pretty good. Taking all the games from Matchday 17, the predictions counted the winner correctly about 66.6% of the time and was within 1 goal 88.89% of the time. 
More samples are needed but looks promising for now.

# Other Notes
There might be some lag when updating the connections and queries, as well as changing something within the file, as the simulations are re-running. 
The chart was set up in a way that it should translate rather seamlessly with the other top 5 leagues (i.e. Bundesliga, LaLiga, Ligue 1, Serie A). It would be worth looking into this to see how well the predictions would work in those leagues.
Some other notes to consider:
1.	The Raw Data and Calculations sheets are hidden, just Unhide them to see them.
2.	The sheet was created in a computer with a 2560x1440 monitor, so it might seem a bit large for someone on the standard 1920x1080 monitor. 
3.	The predictions cannot account for January transfers. Some transfers can shake up the table and results for the rest of the season (think of Suarez to Liverpool in 2011). 
4.	The predictions cannot account for injuries as well (e.g. Tottenham’s injury problems in the 2023-2024 season). 
5.	The predictions cannot account for a recent run of form.
6.	Point deductions have to be done manually, as in the case of Everton in the 2023-2024 season.
7.	The calculations sheet looks a bit messy. It is left as is for now, however I may come back to clean it up a bit later.
8.	The radar chart seems to break occasionally, not sure why, but will look to fix soon.
