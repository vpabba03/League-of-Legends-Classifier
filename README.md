# League of Legends Classifier
### Vishwak Pabba and Manav Jairam


## **Introduction**
League of Legends is an online, multiplayer game where players in teams of 5 battle each other in Summoner's Rift, a massive arena that has an expansive environment and monsters. Players fight against each other to destroy the enemy by toppling turrets and non-playable minions, slowly advancing through 1 of 3 lanes (top, bottom, middle) towards their enemies' nexus crystal. When a team destroys their enemy nexus crystal located in their base, they ultimatly win the game.

The dataset we used for analysis was found on https://oracleselixir.com/tools/downloads, and contained competitive match data from 2022. The dataset included information about the team composition in each competitive match, an individuals game statistics in each respective match, and match details (date, split, game ID, etc.). 

Over the course of the project, we cleaned the dataset for the necessary columns that we would use to train various model to classify whether or not a team will win a match. Using a Decision Tree Classifier following hyperparameter testing, we arrived at a model that had an accuracy of ~98%.  
