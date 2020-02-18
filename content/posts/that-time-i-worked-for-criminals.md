---
title: "That Time I Worked for A Criminal Organization"
date: 2020-02-18T14:15:48-05:00
tags : ["interviews","devops","linux","ptr","The Law","Micfo"]
draft: true
---


# That time I was employed by a criminal organization

I was employed by [Micfo](https://www.wsj.com/articles/fraud-case-in-charleston-s-c-shines-light-on-webs-dark-corners-11581944400) for several months, I quickly determined that they were shady and invovled in illegal practices. I did not quit because of that, I quit because I felt unwanted, by HR and Management. Later they would ask me to come back. This is the full story. There will be amazon affiliate links at the end of the page in the epiloge(I am currently unemployed).


## Should you trust me?

### Why you shouldn't?
There are a lot of reasons you shouldn't trust my word, here are a few:

    * I ran a Hacker Culture Blog for many years.
    * Kind of just generally the worst.
*[Hacker Culture Blog](http://bsdpunk.blogspot.com)
### Why you might

I am being as candid in this post as I can. If I make an error, or something doesn't seem right, I'll check with what I know, and correct it, if necessary.

### The Interview

So I've interviewed with google, facebook, linkedin, and et cetera. I have a prolific interviewing career, if nothing else. In this interview I was told I would be working with edge networks, and IoT devices. This was not the case, on a day to day basis I was automating processes involving provisioning and DNS, I didn't mind that though, in fact I liked it. But it was not the job presented during the interview. During the personality portion of the interview I revealed I liked Canadian HipHop, I always reveal something quirky about myself during this part of the interview. Try to read the room and find something they're going to remember but not be offput for this one, like I said I went with you know, liking WordBurglar. Now I've gotten some [weird interview questions](http://bsdpunk.blogspot.com/2016/01/what-to-study-before-linux-admin.html) over the course of my career, but this is the most severe. Last question of the interview.  
  
Tell me something that would make me not want to hire you?  

I have bipolar.  
  
### Arriving at Charleston
After quite the drive to Charleston, I had a couple days to lounge until my first day, I was moved by the beauty of the architecture and the niceness of the weather. Because of my mentioning composing some hip hop music and talking about Canadaian hip hop, they decided to induct me onto the team with the weirdest hip hop surrounding me and singing a song from 8 mile I believe. Deep down I'm still just a nerd, so this was terrifying. 

### Amazing people, Amazing place
Charleston is a beautiful city, and the weather is always nice. You wake up, you think what should I wear? Then you think well what's the weather going to be? Fucking Nice. So whatever is fine. I think I wore my "Magneto was Right" shirt a little to habitually on off hours. My job began and though the work was not quite what was promised, I was happy to be working on fun things. I would start carpooling with a woman who would become my favorite Charleston friend. We clicked very quickly and one coworker was even awed by our ability to communicate thoroughly non verbally. For a while everything was the best.

## The VPN Crimes

### Mishandling of warrants
We recieved a lot....A LOT...of subpeonas and search warrants. I had worked as part of an abuse and trust division of a company before, so being handed some of these responsibilities didn't seem that odd to me. At one point we recieved a search warrant, addressed to Micfo, but sent to one of our Channel Partners. I told management I would just call the LEO agent in the warrant, and sort it out. This caused more than an outcry with upper managment. They insisted I never talk to any LEO, and that I only respond to the Warrants and Subpeonas through the portal of the Channel Partner, and as the Channel Partner. This was the first time I was discouraged to be working there. More than that, I was driven to near tears, with a mix of emotions, of why do these people I care so much about, asking me to do something that is wrong. At that point, everything I touched became open to an eye of scruitiny. Within a week I was convinced Micfo encompassed more than just Micfo, but a handful of shell companies:

    - Cloudiac
    - Contina
    - Hostbang
    - Hostaware
    - Oppobox
    - Roya
    - Telentia
    - Virtuzo

This was later proved to be true when the CEO(Amir), admitted in the ARIN(American Registry for Internet Numbers) trial, he was really behind these entities.
  
  
We had a portal for each channel partner, level 1 tickets would get taken care of by a team in India all other were escalated to us. Once I had a reason to distrust what was happening, it was easy enough.

### Child Endangerment 

As mentioned before I noticed that the amount of Subpeonas and Search Warrants, seemed exceptional for a company our size. I resigned it to the fact that we hosted a lot of VPNs, and people on VPNs tend to misbehave. I would lean on this excuse a lot, after the realization of Child Endangerment search warrants, would come in a lot. Much more than I was comfortable with. I was eventually removed from dealing with these, I think because how uncomfortable I was with all of this was visible...and audible.

## Crimes involving Fraudlant use of (mostly)IPv4 and IPv6

### PTR records, Enabling Spam

After editing my millionth PTR record, and then suggesting we automate these PTR efforts. I started to wonder, why the fuck I was not only adding PTRs by the boat load, but why was I changing them so quickly as well. Well what's a PTR record for? At it's most basic it's a reverse lookup, so you lookup an IP you get a name. But that's really nothing, right? Well what uses PTR records....email, to prove that something isn't a spammer. Oh no.jpg. Where we spamming? Of course not, that's illegal. Were our customers using us to spam. Fuck yes they were. I'm not changing a million PTRs a day, and building a system they can automatically submit to, to change them, cause they're not doing that. We were without a doubt, enabling spamming. Our largest customer would submit PTR deletions, additions, and changes to our channel partners, I would log into the channel partner portal, and recieve them then act on them.


### Violation of Terms of Service

Often both the CEO and Vice President would talk openly about how one of our clients was violating the TOS of several companies. At the time, the scale of what was happening was lost to me, I remember another client at a different hosting company I once worked for doing the same thing, by getting large blocks of IPv6 addresses, and it was dismissed as fine, he was just a clever guy abusing the system a little. It didn't occur to me that this company was doing this on a massive scale, not rotating through a few IPv6 blocks with a couple of servers, but more like through hundreds of thousands of addresses with servers numbering in the 100s or 1000s. It wouldn't occur to much later, in fact more than a year the company doing this was most likely trying to clone google search results in order to stay ahead of/on target with the google page rank system. Having an accuarte view of how the ever evolving page rank algorithm actually worked.

## Leaving, The Law, And How things Stand

It was a small company, and I talked to those who would listen. However, when the troll-like IT guy says one thing, and the CEO who has a reality distortion field says another it doesn't matter. An 18 Charisma will alway beat a 15 Intelligence. Every fucking time. HighSchoolAllOverAgain.mp4

### Quiting

I don't want to portray myself as noble, or a hero. I didn't quit because of a moral obligation. I didn't quit out of righteousness. I quit for the only reason I ever quit a job. I felt like there were people there who didn't want me there. I loved the majority of my coworkers. I loved the work. But I don't stay in places I'm not wanted. I didn't go to any law enforcement authority. I didn't care what they were doing, I just felt unwanted. I felt like I moved across the country to a great place with some great people, but those who lied to me about what I was to be doing, and lied to me, to my face about what I was actually doing, were saying don't be here anymore, so I wasn't. Why would I go to law enforcement, the people who still worked there were my friends.

### The First Reach out to the FBI
 So what changed? Some months passed. One day I was talking to my friend, and mentor still there and he had quit. Not only had he quit, he left a scathing review on glassdoor. They threatened to sue him over it. You can fuck with me, all day long. You can not fuck with my friends. That weekend was the weekend of Phreaknic, the local hacker con. I knew there would be an FBI agent there, there always is. I told him about a possible whistle blower situation, he asked if anyone is in immediate danger I said no. He gave me his card, and told me to call him. I called him after the weekend was over, I got spooked on the phone, because....I'm me...and I never really got through. I sent two emails to him about everything that happened at Micfo I remember being possibly illegal or shady. Nothing became of it.

### A SECOND CHANCE

No not really, I recieved an email from Amir asking me if I wanted to work for him again, as if.

### Moving on, I thought

More months passed. And then I heard from my best Charleston friend. She had left Micfo and they were suing her. "Would you talk to my Lawyer? Would you talk to ARINs Lawyer? Would you talk to the Charleston FBI Agent?". FUCK YES, MY BEST GOOD FRIEND, I WILL DO ALL OF THE ABOVE. Anybody who listened I talked.

### So where does everything stand

ARIN sued and won back the IPv4 numbers. The court awarded [ARIN](https://teamarin.net/2019/05/13/taking-a-hard-line-on-fraud/) $350,000 , and invoked the IPs back. At this time some of the IPs still have micfo information related to them, which may just be slow processing. My understanding is that the criminal investigation is still underway, with over 20 counts of Fraud.


### I'm unemployed, buy a book from amazon

Think about clicking an amazon link and buying a book:
 [Fire in the Valley is my all time favorite historical book about computers.](https://www.amazon.com/gp/product/0071358927/ref=as_li_tl?ie=UTF8&tag=bsdpblog-20&camp=1789&creative=9325&linkCode=as2&creativeASIN=0071358927&linkId=7df43e63a8f74a77f798ff6b9f148f5f)
 <br />
 <br />
 [Silence on the wire is a very fun, and very technical security book, it is my favorite security book.](https://www.amazon.com/gp/product/1593270461/ref=as_li_tl?ie=UTF8&tag=bsdpblog-20&camp=1789&creative=9325&linkCode=as2&creativeASIN=1593270461&linkId=90ddf44080eaf9caf7953b46d3844769)
 <br />
 <br />
 [Coders at work, is one of the best books for aspiring and current coders, I mean JWZ how can you go wrong.](https://www.amazon.com/gp/product/1430219483/ref=as_li_tl?ie=UTF8&tag=bsdpblog-20&camp=1789&creative=9325&linkCode=as2&creativeASIN=1430219483&linkId=35efdfab63ae6ba0c4a0855483364ec4)
 <br />
 <br />
 [And if you're looking for something a bit more dangerous.](https://www.amazon.com/gp/product/1597499579/ref=as_li_tl?ie=UTF8&tag=bsdpblog-20&camp=1789&creative=9325&linkCode=as2&creativeASIN=1597499579&linkId=a58981ba296c874276f14be594c9bddd)
 <br />
 <br />
