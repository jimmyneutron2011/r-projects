library(ggplot2)

library(ggthemes)

library(plotly)

library(dplyr)



**DATA**

batting <- read.csv('Batting.csv')

print((head(batting)))

![data-viz-1](https://user-images.githubusercontent.com/97744709/210138968-c84ae4a1-d221-4e48-a7c0-4b02d7ca245b.jpg)



#Using str() to check the structure

print(str(batting))

![data-viz-2](https://user-images.githubusercontent.com/97744709/210139032-88be140b-fb0e-47c3-a27f-0cca049459fb.jpg)



#Calling the head() of the AB (At Bats) column

print(head(batting$AB))

![data-viz-3](https://user-images.githubusercontent.com/97744709/210139149-614a03a1-1c40-4545-8d5a-b06913777754.jpg)



#Calling the head of the doubles (X2B) column

print(head(batting$X2B))

![data-viz-4](https://user-images.githubusercontent.com/97744709/210139284-505b61ad-6123-42ee-b987-7ca11471a430.jpg)



**FEATURE ENGINEERING**

#Creating a new column called BA and adding it to the batting data frame

batting$BA <- batting$H / batting$AB

print(tail(batting$BA,5))

![data-viz-5](https://user-images.githubusercontent.com/97744709/210139410-26bc8637-23d4-45fb-87db-5f565b1c2b5b.jpg)



#Creating a new column called OBP (On Base Percentage) and adding it to the batting data frame

batting$OBP <- (batting$H + batting$BB + batting$HBP) / (batting$AB + batting$BB + batting$HBP + batting$SF)

print(tail(batting$OBP, 5))

![data-viz-6](https://user-images.githubusercontent.com/97744709/210139575-9d1a7526-b79b-46e5-a6d2-84d335accf3f.jpg)



#Creating a new column called SLG (Slugging Percentage) and adding it to the batting data frame

batting$X1B <- batting$H - batting$X2B - batting$X3B - batting$HR

batting$SLG <-
  (batting$X1B + (2 * batting$X2B) + (3 * batting$X3B) + (4 * batting$HR)) / (batting$AB)

print(tail(batting$SLG, 5))

![data-viz-7](https://user-images.githubusercontent.com/97744709/210139657-c0e95635-95e7-43d7-b1bb-fc3cee53468e.jpg)


**MERGING SALARY WITH BATTING DATA**

sal <- read.csv('Salaries.csv')

#Using summary to get a summary of the batting data frame

print(summary(batting))

![data-viz-8](https://user-images.githubusercontent.com/97744709/210139819-ad4a7e54-bde1-469b-95b8-610f6a4a916a.jpg)



#Notice the minimum year in the yearID column. Our batting data goes back to 1871 and our salary data starts at 1985

#We need to remove the batting data that occured before 1985.

batting <- subset(batting, yearID >= 1985)



#Using summary again to make sure the subset reassignment worked

print(summary(batting))

![data-viz-9](https://user-images.githubusercontent.com/97744709/210139961-b933236b-5eaa-4740-a8e6-4441e94d8b62.jpg)



#Using the merge() function to merge the batting and sal data frames by c('playerID','yearID') and calling the new data frame combo

combo <- merge(batting, sal, by = c('playerID', 'yearID'))

#Using summary to check the data

print(summary(combo))

![data-viz-10](https://user-images.githubusercontent.com/97744709/210140098-0b9a9eee-031b-45d1-a8fc-4fb6ffc0b745.jpg)

**ANALYZING THE LOST PLAYERS**

#Using the subset() function to get a data frame called lost_players from the combo data frame consisting of those 3 lost players

lost_players <- subset(combo, playerID %in% c('giambja01', 'damonjo01', 'saenzol01'))

print(lost_players)

![data-viz-11](https://user-images.githubusercontent.com/97744709/210140547-495eba6b-cea9-445c-be77-db79651d7559.jpg)



#Since all these players were lost in after 2001 in the offseason, let's only concern ourselves with the data from 2001

#Using subset again to only grab the rows where the yearID was 2001

lost_players <- subset(lost_players, yearID == 2001)

#Reducing the lost_players data frame to the following columns:playerID, H, X2B, X3B, HR, OBP, SLG, BA, AB

lost_players <- subset(lost_players, select = c(playerID, H, X2B, X3B, HR, OBP, SLG, BA, AB))

print(head(lost_players))

![data-viz-12](https://user-images.githubusercontent.com/97744709/210140700-21239680-e3ad-4ca2-96bd-fff9628c65f9.jpg)



**REPLACEMENT PLAYERS**

#We have three constraints:

#The total combined salary of the three players can not exceed 15 million dollars

#Their combined number of At Bats (AB) needs to be equal to or greater than the lost players -> Sum(AB) >= 1469

#Their mean OBP had to equal to or greater than the mean OBP of the lost players -> Mean(OBP) >= 0.3638687

#Using subset() function to find players based on the above criteria

combo <- subset(combo, yearID==2001 & salary <= (1500000/3) & AB >= (1469/3) & OBP >= 0.3638687)

print(combo)

![data-viz-13](https://user-images.githubusercontent.com/97744709/210141011-2d821445-b5c1-4d55-bcef-151b84379bff.jpg)



#Visualizing the data frame based on the above criteria

pl <- ggplot(combo, aes(x=AB,y=OBP,color=factor(salary), label=playerID)) + geom_point(shape=1, size=5)

print(ggplotly(pl))

![data-viz-14](https://user-images.githubusercontent.com/97744709/210141135-3c0fa34d-d4c8-427d-ba4a-e8064a000e3a.jpg)
