---
title: "Turning the D and D Monster Manual into JSON"
date: 2017-10-10T04:53:45-05:00
tags : [ "json","dnd","csv","grep","awk","linux","bash","sed","pdf","regex"]
draft: false
---

# The What

I found a PDF copy of the monster manual (Of course I also own a copy, but I needed digital for this).  I was able to grep out all the tables for monster stats...however, I couldn't find a way to programatically get their names so I assigned them ID numbers. [Git Repo](https://github.com/bsdpunk/monman)

## GNU vs BSD Tools

Though generally I script on my mac, I use the gnu tools vs the bsd tools. Why is this? I grew up on the GNU tools and am more familiar, there are slight differences in the BSD tools which can generally hang me up for hours. So I install them on my mac via brew. For this particular script I used GNU grep, and GNU sed, or ggrep and gsed respectively.

## The How

For Mac:
``` bash
#!/bin/bash - 
#===============================================================================
#
#          FILE: buildMonDB.sh
# 
#         USAGE: ./buildMonDB.sh 
# 
#   DESCRIPTION: Turn the Monster Manual PDF into JSON
# 
#       OPTIONS: ---
#  REQUIREMENTS: ---
#          BUGS: ---
#         NOTES: ---
#        AUTHOR: Dusty Carver (), 
#  ORGANIZATION: 
#       CREATED: 04/13/2018 16:00
#      REVISION:  ---
#===============================================================================

set -o nounset                              # Treat unset variables as an error
ggrep "Initiative +[0-9]" monman.txt > Monsters
#Had to delete this last line, it was output from grep about the file
sed '$d' Monsters
IFS=$'\n'
n=0
echo "["
for i in $(cat Monsters); do 
echo "{"
    #echo $i;
    echo "\"id\" : \"$n\","
    #Names are not included in the Table in the PDF
    echo '"name": "",'
    #Add Description Keyword, Then Grab Description, Put in quotes
    printf "\"Description\": \""; echo $i | ggrep -o '.* Initiative +[0-9]\+' | gsed 's/Initiative.*//' | gsed 's/$/",/'
    #Grab And Change Inititive to quoted JSON Format
    echo $i | ggrep -o 'Initiative +[0-9]\+' | gsed 's/Initiative /"Initiative": "/' | gsed 's/$/",/'
    #Grab and Change Level
    echo $i | ggrep -o 'Level +[0-9]\+' | gsed 's/Level /"Level": "/' | gsed 's/$/",/'
    #Grab and Change Senses
    echo $i | ggrep -o 'Senses.*HP' | gsed 's/HP$//' | gsed 's/Senses /"Senses":"/' | gsed 's/$/",/'
    #Grab and Change hp
    echo $i | ggrep -o 'HP [0-9]\+' | gsed 's/HP /"HP": "/' | gsed 's/$/",/'
    #Other health is a big category and requires some additional work, grab all the information, remove any quotes
    #Printf so it's on the same line
    printf '"Other Health": "';
    #Printf again, Command substitue the echo, greps and seds, that get it to where you want it, then transpose all the quotes to deleted
    printf "$(echo $i | ggrep -o 'HP [0-9]\+.*AC [0-9]\+' | gsed 's/HP [0-9]\+ //' | gsed 's/AC [0-9]\+$//' | gsed 's/^; //' | tr -d '"' )\""
    echo ","
    #Grab and Change AC
    echo $i | ggrep -o 'AC [0-9]\+' | gsed 's/AC /"AC": "/' | gsed 's/$/",/'
    #Grab and Change Fortitude
    echo $i | ggrep -o 'Fortitude [0-9]\+'| gsed 's/Fortitude /"Fortitude": "/' | gsed 's/$/",/'
    #Grab and Change Reflex
    echo $i | ggrep -o 'Reflex [0-9]\+'| gsed 's/Reflex /"Reflex": "/' | gsed 's/$/",/'
    #Grab and Change Will
    echo $i | ggrep -o 'Will [0-9]\+'| gsed 's/Will /"Will": "/' | gsed 's/$/",/'
    #Grab and Change Speed
    echo $i | ggrep -o 'Speed [0-9]\+'| gsed 's/Speed /"Speed": "/' | gsed 's/$/",/'
    #Other Abilities is a lot of information, again grab it all and remove any quotes
    printf '"Other Abilities": "'
    #Printf again, Command substitue the echo, greps and seds, that get it to where you want it, then transpose all the quotes to deleted
    printf "$(echo $i | ggrep -o 'Speed [0-9]\+.*' | gsed 's/Speed [0-9]\+//' | gsed 's/ m.//' | tr -d '"' )\""
    echo "},"

    #iterate our id number
    n=$(($n + 1))
done
echo "]"
```

For Linux:
```
#!/bin/bash - 
#===============================================================================
#
#          FILE: buildMonDB.sh
# 
#         USAGE: ./buildMonDB.sh 
# 
#   DESCRIPTION: 
# 
#       OPTIONS: ---
#  REQUIREMENTS: ---
#          BUGS: ---
#         NOTES: ---
#        AUTHOR: YOUR NAME (), 
#  ORGANIZATION: 
#       CREATED: 04/13/2018 16:00
#      REVISION:  ---
#===============================================================================

set -o nounset                              # Treat unset variables as an error
grep "Initiative +[0-9]" monman.txt > Monsters
#Had to delete this last line, it was output from grep about the file
sed '$d' Monsters
IFS=$'\n'
n=0
echo "["
for i in $(cat Monsters); do 
echo "{"
    #echo $i;
    echo "\"id\" : \"$n\","
    #Names are not included in the Table in the PDF
    echo '"name": "",'
    #Add Description Keyword, Then Grab Description, Put in quotes
    printf "\"Description\": \""; echo $i | grep -o '.* Initiative +[0-9]\+' | sed 's/Initiative.*//' | gsed 's/$/",/'
    #Grab And Change Inititive to quoted JSON Format
    echo $i | grep -o 'Initiative +[0-9]\+' | sed 's/Initiative /"Initiative": "/' | gsed 's/$/",/'
    #Grab and Change Level
    echo $i | grep -o 'Level +[0-9]\+' | sed 's/Level /"Level": "/' | gsed 's/$/",/'
    #Grab and Change Senses
    echo $i | grep -o 'Senses.*HP' | sed 's/HP$//' | gsed 's/Senses /"Senses":"/' | gsed 's/$/",/'
    #Grab and Change hp
    echo $i | grep -o 'HP [0-9]\+' | sed 's/HP /"HP": "/' | gsed 's/$/",/'
    #Other health is a big category and requires some additional work, grab all the information, remove any quotes
    #Printf so it's on the same line
    printf '"Other Health": "';
    #Printf again, Command substitue the echo, greps and seds, that get it to where you want it, then transpose all the quotes to deleted
    printf "$(echo $i | grep -o 'HP [0-9]\+.*AC [0-9]\+' | sed 's/HP [0-9]\+ //' | gsed 's/AC [0-9]\+$//' | gsed 's/^; //' | tr -d '"' )\""
    echo ","
    #Grab and Change AC
    echo $i | grep -o 'AC [0-9]\+' | sed 's/AC /"AC": "/' | gsed 's/$/",/'
    #Grab and Change Fortitude
    echo $i | grep -o 'Fortitude [0-9]\+'| sed 's/Fortitude /"Fortitude": "/' | gsed 's/$/",/'
    #Grab and Change Reflex
    echo $i | grep -o 'Reflex [0-9]\+'| sed 's/Reflex /"Reflex": "/' | gsed 's/$/",/'
    #Grab and Change Will
    echo $i | grep -o 'Will [0-9]\+'| sed 's/Will /"Will": "/' | gsed 's/$/",/'
    #Grab and Change Speed
    echo $i | grep -o 'Speed [0-9]\+'| sed 's/Speed /"Speed": "/' | gsed 's/$/",/'
    #Other Abilities is a lot of information, again grab it all and remove any quotes
    printf '"Other Abilities": "'
    #Printf again, Command substitue the echo, greps and seds, that get it to where you want it, then transpose all the quotes to deleted
    printf "$(echo $i | grep -o 'Speed [0-9]\+.*' | sed 's/Speed [0-9]\+//' | gsed 's/ m.//' | tr -d '"' )\""
    echo "},"

    #iterate our id number
    n=$(($n + 1))
done
echo "]"
```
