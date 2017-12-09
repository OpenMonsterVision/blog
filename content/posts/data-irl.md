---
title: "How to Create Data IRL Maps"
date: 2017-12-08T17:45:48-06:00
tags : ["R","Maps","Me","bsdpunk","Adult","dataset","datasets","Rlang","Code","Graphs"]
draft: false
---
<script src="https://openmonstervision.github.io/prism.css" type="text/javascript"></script>
<script src="https://openmonstervision.github.io/prism.js" type="text/javascript"></script>
# I used to live in Philadelphia

<a href="https://i.imgur.com/dRefCUE.png"> ![Philly Crime](https://i.imgur.com/dRefCUE.png "Philly Crime")</a>

I assure I commited neither. I got the dataset [here](https://www.kaggle.com/mchirico/philadelphiacrimedata "Kaggle Dataset of Philly Crime"). 

## The Code

```r
library("ggplot2")
library("ggmap")



#Get Philadelphia Crime Data, this takes a while
philCrimeData <- read.csv("~/Downloads/crime.csv",stringsAsFactors = FALSE)
# Get Philly Map, Zoom in slightly
philCrimeMap <- get_map(location = "philadelphia",maptype = "roadmap", zoom = 12)
#Create Map
philCrimeMapGG <- ggmap(philCrimeMap) 
#Create My Address
me <- data.frame(
  Lon = c(-75.112021),
  Lat = c(39.991058),
  Crime = "Me",
  stringsAsFactors = FALSE
)

#Data Needs to be arranged so that the legend prints properly
#Create rapes table
rapes <- philCrimeData[which( philCrimeData$Text_General_Code == 'Rape' ),]
rapes <- rapes[,c("Lat","Lon")]
#Create Crime Col
rapes$Crime <- "Rapes"
#Create Murders Table
murders <- philCrimeData[grep(pattern = "Homicide",philCrimeData$Text_General_Code),]
murders <- murders[,c("Lat","Lon")]
#Create Crime Col
murders$Crime <- "Homicides"
#Bind Tables
Crimes <- rbind(rapes, murders)
#Add Dots
pngf <- philCrimeMapGG +
  #Make Crime dots, There's a lot So make them Transparent
  geom_point(show.legend = TRUE,data = Crimes, aes(x = Lon, y = Lat, color = Crime),size = 0.5, alpha = 0.2) +
  #Make An Outline for the Me dot, 1 unit bigger than the me dot, Don't want this to show up in the legend, Not Transparent
  geom_point(show.legend = NA,data = me, aes(x = Lon, y = Lat), color = "black", size = 3) +
  #Make the Me dot, size 2 compared to our .5 dot Crime dots, Not Transparent
  geom_point(show.legend = TRUE,data = me, aes(x = Lon, y = Lat, color = Crime), size = 2)
#The Legend is created Automagically, By utilizing the Crimes and Me Tables data Structure
#The colors for Murders and Rapes are automagically assigned, Except the Me dot which is specified
#Add Labels
final <- pngf +
  xlab(label = "Longitude") +
  ylab(label = "Latitude") +
  ggtitle("Philly",subtitle = "Criminal Activity") +
  theme(plot.title = element_text(hjust = 0.5), plot.subtitle = element_text( hjust = 0.5))

#Save as PNG
png(filename = 'Philly.png', width=1000, height=625)
print(final)
dev.off()
```
The code is probably better highlighted on [GitHub](https://github.com/OpenMonsterVision/maps/blob/master/createMap.R "GitHub Link to createMap.R").



# I currently live in Nashville

<a href="https://i.imgur.com/6k27QDB.png">![Adult Business Map](https://i.imgur.com/6k27QDB.png "Nashville Registered Adult Business")</a>

I've never been to any of these. The dataset can be found [here](https://data.nashville.gov/Licenses-Permits/Davidson-County-Locations-Sexually-Oriented-Permit/g97f-x87i).

## Getting My Lat and Long

First I got a google maps api key, and I saved it to "~/.gmaps". Then I wrote this Script:

```bash
#!/bin/bash - 
#===============================================================================
#
#          FILE: getGPS.sh
# 
#         USAGE: ./getGPS.sh 1600 Pennsylvania Ave NW, Washington, DC 20006
# 
#   DESCRIPTION: 
# 
#       OPTIONS: ---
#  REQUIREMENTS: gsed for Mac, if your on linux change gsed to sed
#          BUGS: ---
#         NOTES: One Shot, Get JSON for Lat and Lon for an address
#        AUTHOR: Bsdpunk, Dusty Carver
#  ORGANIZATION: 
#       CREATED: 12/08/2017 19:25
#      REVISION:  2
#===============================================================================

set -o nounset                              # Treat unset variables as an error
#Put Your API Key in ~/.gmaps
#Just Copy it, and Paste it into vim as the above file
KEY=$(cat ~/.gmaps)
ARGARR="$@"

APPEND=''
for i in $ARGARR;
do
    APPEND="$APPEND$i+"
done
#Remove Commas
APPEND=$(printf $APPEND | gsed 's/,//g')
#Get JSON, and reduce it
curl -sKL "https://maps.googleapis.com/maps/api/geocode/json?address=$APPEND&key$KEY" | egrep 'lat|lng' | head -n2

```
## Creating The Map

Now It's just like the other one. Just as simple, and I tried to comment the hell out of my code.

```r
library("ggplot2")
library("ggmap")

#Get Nashville Adult Data
nashAdultData <- read.csv("~/data/adult2.csv",stringsAsFactors = FALSE)
# Get Nashville Map, zoom in 2 additional levels
nashAdultMap <- get_map(location = "nashville",maptype = "roadmap", zoom = 12)
#Create Map
nashAdultMapGG <- ggmap(nashAdultMap) 
#Create My Address
me <- data.frame(
  Long = c(-86.74),
  Lat = c(36.22),
  AdultBusiness = "Me",
  stringsAsFactors = FALSE
)
#Create Adult Frame
adultGPS <- nashAdultData[,c("Lat","Long")]
#Add type
adultGPS$AdultBusiness <- "Adult Business"
#Combine tables
AdultCommerce <- rbind(me, adultGPS)
#Save Map, with additions to variables
final <- nashAdultMapGG +
  #Add black dots for erryone, do not show in legend
  geom_point(show.legend = FALSE,data = AdultCommerce, aes(x = Long, y = Lat),size = 3, color = "black") +
  #Create different Color dots based on type(AdultBusiness or Not), do show in legend
  geom_point(show.legend = TRUE,data = AdultCommerce, aes(x = Long, y = Lat, color = AdultBusiness),size = 2) +
  #Label X axis Longitude
  xlab(label = "Longitude") +
  #Label Y axis Longitude
  ylab(label = "Latitude") +
  #Add Title and Subtitle
  ggtitle("Registered Adult Businesses",subtitle = "Nashville") +
  #Center Title and Subtitle
  theme(plot.title = element_text(hjust = 0.5), plot.subtitle = element_text( hjust = 0.5))

#Save as PNG
png(filename = 'Nashville.png', width=1000, height=800)
print(final)
dev.off()

```
# Why Would I waste my time doing this ?

Cause [/r/data_irl](https://www.reddit.com/r/data_irl/ "Data IRL") is awesome. 

# Why Would You Waste Your Time Doing This ?

Cause maybe someone on [/r/data_irl](https://www.reddit.com/r/data_irl/ "Data IRL") will see this, and realize making shit in R is ridic easy. And we get more data, IRL.

# Mind Blown

Guess what you didn't make a map, you just made a graph. That thing is just decimal numbers mapped out.

# Who am I

I am [@bsdpunk](http://twitter.com/bsdpunk)[@bsdpunk], and I reccomend buying this R book cause [machine learning](https://www.amazon.com/gp/product/1784393908/ref=as_li_qf_sp_asin_il_tl?ie=UTF8&tag=bsdpblog-20&camp=1789&creative=9325&linkCode=as2&creativeASIN=1784393908&linkId=8927b5137c562d2dd27d43f76c0fba3c "A fucking ad") is awesome.
