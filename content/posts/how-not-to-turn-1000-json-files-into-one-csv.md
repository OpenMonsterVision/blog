---
title: "How Not to Turn 10000 Json Files Into One Database, The Perils of Freelancing"
date: 2017-10-30T14:37:20-05:00
tags : ["BigQuery","data","databases","awk", "csv", "JSON", "sed","grep", "bigdata"]
draft: true
---
# The Wrong Way

## Worse Is Better

As a grubby unix hacker, I usually take the easy way on my projects. And as of late, I have been getting fairly [simple data conversion](https://openmonstervision.github.io/blog/posts/how-to-make-money-using-grep-sed-and-awk/) jobs from various freelancing sites and customers. So of course, [worse is better](https://www.jwz.org/doc/worse-is-better.html) is my go to philosophy. Well this dug 'ol [@bsdpunk](http://twitter.com/bsdpunk) into a little hole. I got a couple of example JSON files, from a guy who said he had a lot of data and wanted to turn them into a single CSV, to load into BigQuery. The couple of three JSON files he gave me combined so easily, mostly using someone elses [node module](https://www.npmjs.com/package/json2csv). So I just thought I could get away with a few bash loops, one for converting one for combining, along with a couple lines of sed. No problem, easy peasy lemon squeezey. When he came back and said the price was a little steep, I lowered it because this wasn't going to take very long and what's 20 bucks, I mean really.


# Humble Bash Begennings 
It looked something like this:

``` bash
for i in $(find . -name *.json); do cat $i | json2csv -F >> all.csv && echo $i; done
cat all.csv | sed '/^\"header/d' > noheader.csv
cat */noheader.csv >> header.csv
```

## Sophisticated Bash 

When I got the rest of the data I was floored. There was so much data. Over 70 gigs. Because of the amount of the data, I knew pretty much instantly this wouldn't work. It was going to be too slow. So, I started parallelizing the bash and running some of the commands individually, or a few in foor loop with some single ampersands, instead of in a script. If I knew what I was doing I would have done it more like [this](http://codehackit.blogspot.be/2013/08/divide-and-conquer-with-bash-and-friends.html). But I was in a hurry, and thought this was a good solution.

## Of Course It Failed

The final 53 gig CSV failed. I mean it existed, but it was broke. If you know anything about JSON, you know why. The Objects and how they were populated changed. Of course it did, that's how JSON works. I didn't forsee it because the two files I tested on had the same structure...yeah I know, sample size. Also dumb, I know.


## But Why, Why Didn't You Just Cancel the Order

No one else was, taking orders, from upwork, fiverr, peopleperhour, or guru.com. I even tried to cancel it, but my client suggested just going through with it. And I would have felt terrible cancelling, I hate calling it quits. Especially if someone was relying on me. 


## So how did you fix it

I built a custom parser in Node.js cause you know, the customer deserves his data, and cause I can roll it into a bigger project later. OK I lied...[@trozdol](http://twitter.com/trozdol) told me to, he told me to goddamn #adult.

Example Code from that monstrosity:

``` javascript

var username                      = json[i].user.username || '';
var user_profile_picture          = json[i].user.profile_picture || '';
var user_id                       = json[i].user.id || '';
```

You know, like a goddamn JSON parser. For some reason I decided it was time to do, [The Right Thing](https://yosefk.com/blog/what-worse-is-better-vs-the-right-thing-is-really-about.html). Of course I am aware these philosophies don't exactly mimic my problem. But if you don't know about them, you need to, and if you do, it's just a silly blog post from a silly person.

# Javascript Crashed

I knew I should I have used python...fuck. Something's funny though, the file it generated was 8 gigs, it should only be around 2. What the fuck.

## Something Really Dumb

Hey you guys, I used the same iterator in two for loops. I am dumb, I am Really Really, Dumb.

## Something Less Dumb

It still suffered from a crash. Over used the heap, and other sayings. At first I tryed forcing garbage collection, using --expose-gc flag on node, that made it incrediblly slow and still crash. Next I tried just increasing the max memory node could use...bob's your uncle. Now it works like a charm. That flag is --max_old_space_size=7000. That's 7000 Megabytes. Go big or go home.

# 20 Bucks Is A Lot

At this point on this project I'm working for about $3.00 an hour, and by the end of it, it will probably be in the two digit cents range, as I haven't quite finished. Still have some details to work out.

## Wait A Minute You Act Like A Badass, This Sounds Like You're Barely Scraping By

Yeah I eat hot dogs every day. I'm still a badass I was offered quite the lucrative gig in The Valley this week, but it meant leaving my girlfriend and cats...So suck it. I be out here every day failing, I don't give a shit. Come at me bro. Other masculine but meaningless sayings.

# On to Bigger and Better Things

I am beginning a larger all encompassing data project, that will slurp data and puke awesome. If you're familiar with me, you know I've already built something like this before, but that platform was more about using data to create predictive models, and it's time I return to the much more humbler, but also much more viable data cleaning and conversion system.

# Whose fault was it

Read some [Camus](https://www.amazon.com/gp/product/B00IJ0TWHK/ref=as_li_qf_sp_asin_il_tl?ie=UTF8&tag=bsdpblog-20&camp=1789&creative=9325&linkCode=as2&creativeASIN=B00IJ0TWHK&linkId=35eefd37871cbb47a02b0650e3d02812) sometime simpleton, it's nobody's fault. The world is absurd and the search for meaning ridiculous. Err... I mean I couldn't have seen ahead giving the data I was giving. At the beginning of the project with the amount of data, and type of data I was given I foresaw possible issues, but as the eternal optimist, I tredged along.

# The Who

My name is [@bsdpunk](http://twitter.com/bsdpunk), and I work in the data mines for a living. For apparently very cheap, so you should tottally hit me up. Also I'm a socialist who lives with a ["healthy" blind cat](https://www.instagram.com/p/Ba7bXmwjS0j/?taken-by=bsdpunk), and I feel like that joke, writes itself. Also by that [Camus](https://www.amazon.com/gp/product/0679720219/ref=as_li_qf_sp_asin_il_tl?ie=UTF8&tag=bsdpblog-20&camp=1789&creative=9325&linkCode=as2&creativeASIN=0679720219&linkId=49f9264674854c7516a7be2a0b834f7d) book, you uncultured swine. Oh and I know he wants like none of the credit for this awful monstrosity but [@trozdol](http://twitter.com/trozdol) contributed and, cleaned up my code so it didn't look like I wrote it. Not that either one us want to claim the final product.
