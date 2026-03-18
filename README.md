# League of Legends 15-Minute Statistical Analysis

Data Analysis Pipeline Project for UCSD's DSC 80
Author: Annabelle Guiditta

## Introduction

League of Legends (also known as LoL), is a popular multiplayer online battle (MOBA) game developed and published by Riot Games, the creators of Valorant and Arcane. Well known as Riot's most sucessful game, LoL is world-known and renowned for its large Esports influence. The dataset this report will cover comes from [Oracle's Elixer](https://oracleselixir.com/about), a free to access collection of Datasets from the League competitive sphere since 2015. In particular, the dataset referenced in this report draws from the 2025 competitive season.

This dataset provides information of each individual game, team, and player, recording statistics such as first blood, game length, number of kills, and results, culminating in a rich source of information for data analysis.

A typical League of Legends game lasts between 25-35 minutes and consists of players killing monsters, fighting other players, and supporting their team. Much of this is done through accumulating gold and XP, features that allow players to buy items and \_\_\_. These features can be enhanced by kills and assists, which reward the player in gold. Having a large advantage in any of these statistics is key for winning a LoL game, and will be the basis of our analysis in this report.

Our analysis focuses on the impact of early game statistics on the rest of the game, focusing on recording 15 minute features such as gold, XP, Creep Score, Kills, and more. We wish to use these metrics to see how credible early game metrics are for setting the course of the rest of the game, and help us identify what room there is for comeback in professional games.

### Data Description

After cleaning, our dataset consisted of 18472 rows and 19 columns. These include:

- `result`: The outcome of the match for each specific team. True indicates the team that won, False indicates the team that lost
- `participantid`: The individual ID for each player in a round. In our dataset, this consists of either 100 (Blue Side) or 200 (Red Side) to help us differentiate between teams
- `league_tier`: Denotes the tier of the league the match was played in. 'Major' refers to games played in LCK, LEC, LCP, LTA, LTA S, or LTA N, all Tier 1 Professional Leagues. 'International' refers to games held in international competitions, such as MSI, WLDs, EWC, IC, and Asia Master, which host teams from both Major and Minor leagues. 'Minor' refers to games played in all other leagues.
- `split`:
- `date`: The date, formatted as YYYY-MM-DD XX:XX:XX
- `firstPick`:
- `length`: The total length of the game
- `goldat15`, `xpat15`, `csat15`: The amount of gold, experience, or creep score the team had at the 15 minute mark, respectively
- `golddiffat15`, `xpdiffat15`, `csdiffat15` : The difference between the team's gold, experience, or creep score the team had at the 15 minute mark, respectively
- `gold_lead_at15` : A boolean column denoting whether a team had a gold lead (positive 'golddiffat15') at 15 minutes
- `killsat15`, `opp_killsat15` : The total amount of kills the team and their opponent had at 15 minutes, respectively
- `deathsat15`, `opp_deathsat15`: The total amount of deaths the team and their opponent had at 15 minutes, respectively
- `assistsat15`, `opp_assistsat15`: The total amount of assists the team and their opponent had at 15 minutes, respectively

## Data Cleaning and Exploratory Data Analysis

### Data Cleaning

- only keps rows with team in the position column, filtered to only needed columns
- converted results , , first pick to boolean types
- dropped columns that were marked as partial in the datacompleteness column
- converted date to datetime
- manually developed the major, minor, and league splits based on an initial 'league' column
- Developed the stat gold lead at 15, which identfied all rows that had a positive goldiffat15 for later analysis
- Missingness: identified missingness in split and firstpick. Interestingly, rows with missingness in first pick also contained missing values for all picks

Below is the head of the cleaned dataset:

## INSERT HERE

### Univariate Analysis

To begin the exploration of my dataset, I investigated the distribution of Gold at the 15 minute mark.

## INSERT HERE

This histogram of `goldat15` illustrates that the amount of gold teams accumulate by 15 minutes follows a roughly normal distribution, centered a bit under **25,000 gold**. The shape is noticeably right-skewed, with most teams falling between **22,000 and 28,000 gold** at the 15 minute mark. This distribution suggests that gold accumulation at 15 minutes is fairly consistent across professional games, with extreme values on either end being rare. In the context of our research question, this histogram helps establish what a "typical" 15 minute gold value looks like, providing a baseline for understanding how meaningful deviations from this range might be in predicting match outcomes.

### Bivariate Analysis

To begin my bivariate analysis, I attempted to visualize some of the differences between Major and Minor Leagues.

## INSERT HERE

To account for the difference in the number of Major vs Minor league games, I constructed a density histogram to fairly compare the distribution of `golddiffat15` across both tiers. Notably, the Major league distribution has a much sharper and higher peak, suggesting that gold differences in Tier 1 games are more tightly concentrated around 0. Major league games fall mostly between **-5,000 and 5,000 gold**, while Minor league games span a noticeably wider range of roughly **-7,000 to 7,000 gold**. This visual difference is supported by further calculations -- Minor league games exhibit **~73.7% more variance** in `golddiffat15` than Major league games, suggesting a more volatile early game landscape where leads are larger and less evenly matched. This sets the stage for our hypothesis test, where we formally investigate whether this difference is statistically significant.

### Interesting Aggregates

In investigating the importance of a gold lead early on in the game, I developed this grouped table to visualize its impact

## INSERT HERE

In particlar, it can be noted that teams that have a gold lead at 15 minutes tend to win more, and have higher xp, kills, assists, and creep score! In fact, 73% of teams that had a gold lead at 15 minutes ended up winning the round

Additionally, I wanted to investigate the mean values across the different league tiers

## INSERT HERE

Notably, Major league games tend to have **lower average kills and assists** at 15 minutes compared to both Minor and International leagues. This aligns with the expectation that Tier 1 games are more defensive and methodical in the early game. Interestingly, Major leagues have a slightly **higher average CS** (534 vs 514), suggesting that while Major league teams fight less, they farm more efficiently.

## Assessment of Missingness

---

Our analysis is guided by two core questions:

> **Hypothesis Test**: Do minor leagues have a significantly higher mean absolute gold difference at 15 minutes than major leagues?

This allows us to examine whether Tier 1 Major league games are more competitive and better balanced in early game than Minor league games, as measured by how close teams are in gold at the 15 minute mark.

> **Prediction**: Can we predict match outcome from 15 minute stats alone?

This asks whether the early game truly sets the course for the rest of the match, or whether there is still significant room for comeback. If 15 minute stats are strong predictors of the final outcome, it suggests that early game advantages are decisive in professional play.

Together, these questions paint a picture of how early game performance differs across leagues, and how much it ultimately matters.
