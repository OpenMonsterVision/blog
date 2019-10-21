---
title: "Automating your DM workflow with Dndshell"
date: 2019-10-21T17:45:48-06:00
tags : ["dnd", "dm", "shell", "automation", "dndshell", "workflow", "game", "games", "json", "5e"]
draft: false
---

<link rel="stylesheet" href="https://openmonstervision.github.io/css/prism.css" />
<script src="https://openmonstervision.github.io/js/prism.js" type="text/javascript"></script>

# The What
[dndshell](https://github.com/bsdpunk/dndshell/) Is a shell I made so you can quickly, access Monsters and other rules(spells/races/classes/dice roles), in text/json format. Most of the leg work for the rules was done by this project [5e-database](https://github.com/adrpadua/5e-database). You can expand the rules by simply editing the JSON files that come with the shell. To install it's:
``` bash
go get github.com/bsdpunk/dndshell
```

The shell has tab completion and history for the session you are in. It can also be used with arguements so it can be scripted.
```
> ~/go/bin/dndshell                                                                                                                     master!
I mean, I guess you can throw the Wererat at him.  -  Unknown Frustrated GM
> ListRaces
0 Dwarf
1 Elf
2 Halfling
3 Human
4 Dragonborn
5 Gnome
6 Half-Elf
7 Half-Orc
8 Tiefling
> 
```
or
```
~/go/bin/dndshell ListRaces
```
![tab](https://github.com/bsdpunk/dndshell/blob/master/one.gif?raw=true)
![tab](https://github.com/bsdpunk/dndshell/blob/master/two.gif?raw=true)

# The Why
I spend a lot of my time on the command line, and it's the most intuitive way for me to solve problems. Automating with bash or zsh, is second nature to me. If I want to build an encounter I could do so quickly. For Example:
<br />
### Get Monster List: 
<br />

![get monster list](https://openmonstervision.github.io/blog/images/monsterList.gif)
<br />
### Get C4 Monsters
![get c4 monsters](https://openmonstervision.github.io/blog/images/cr4.gif)
<br />
### Get Json For Those Monsters
<br />
![get monster json](https://openmonstervision.github.io/blog/images/monsterToJson.gif)
<br />

Eventually there will be a character creation mechanism once the shell is finished. Currently working on merging an encounter generator I had made before into the shell.

## Currently Unemployed

I am currently unemployed and ask that you take a look at this amazon affiliate link for [Mastering Go](https://www.amazon.com/gp/product/1788626540/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=1788626540&linkCode=as2&tag=bsdpblog-20&linkId=7a540f0054c36f4025c387f6c26c86fe), I have read it, and of the Go books I have, I keep coming back to it.
