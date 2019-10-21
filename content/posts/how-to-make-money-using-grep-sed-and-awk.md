---
title: "How to Make Money Using Grep Sed and Awk"
date: 2017-10-10T04:53:45-05:00
tags : [ "shoppify","csv","grep","awk","linux","bash","sed","pdf","regex"]
draft: false
---

<link rel="stylesheet" href="https://openmonstervision.github.io/prism.css" />
<script src="https://openmonstervision.github.io/prism.js" type="text/javascript"></script>
# The Who

I'm your friendly neighborhood unix man. And I have a problem. I love regex. Now I have two problems.

# The Why
Recently I have become destitute so I have taken to using the skills I have to provide for my poor sick family. It's mostly [disabled cats](https://www.instagram.com/bsdpunk/ "BSDPunk Instagram"). And when all you have is hammer, everything gets nailed. So I thought, you know what people probably don't automate enough of...Data Entry. I logged onto a freelance site of my choice, [upwork](https://www.upwork.com/ "Upwork"). And found exactly what I wanted. Someone who wanted to turn a PDF into a csv. Particularly, a parts catalog into a shopify CSV.

# The Good

The pdf was easy enough to convert into text, originally I thought I would convert all the pages into tiff's and use tesseract to OCR the characters. As it turned out this PDF was all selectable text, and just using pdftotext was sufficient.

For Mac:
``` bash
brew install pdftotext
brew install tesseract
```

All and all when this hideous brain child was finished, I had about an 87 percent success rate.

# The Bad

There are things I regret, and things I regret. I am ashamed to admit this but one of the earlier things I did was strip all the quotes out of the resulting pdftotext file. Which was mostly so I could clean up the commas using this function:

``` bash
function rqc () { awk -F'"' -v OFS='' '{ for (i=2; i<=NF; i+=2) gsub(",", "", $i) } 1' $@ | sed 's/"//g' ;}
```
This functions basically removes all the commas in between quotes in a csv, then removes the quotes. Yeah, for sure, there were better ways to deal with that. But it's not like I have a peer group of other regexers at beck and call.


# The Ugly

Let's look at the code:

``` bash
set -o nounset                              # Treat unset variables as an error
BROAD=$(grep '[A-Z]\.' c.txt -A2 -B9)
#List of all Skus
#LIST=$(grep -P -o '\d{5}-\d{2}\w{0,1}' c.txt | sort | uniq )
LIST=$( grep -o '^[0-9]\{7,8\}' c.txt | sort | uniq  )
#Temp for checking if adjacent price
TEMP=0
for n in $(cat <(echo $LIST) )
do
    #Get Name
    NAME=$( grep "$n" c.txt  -B9 | grep '^[A-Z]\.' | sed 's/^[A-Z]\. //' | sed 's/^[A-Z ]\+       \([A-Z][a-z]\)/\1/' | sed 's/"//g' | grep '[A-Z ]\+' | tail -n1 | sed 's/[a-z].*//' | sed 's/.$//' )
    #Get Body
    DESCRIPTION=$( grep "$n" c.txt  -B9 | grep '^[A-Z]\.' | sed 's/^[A-Z]\. //' | sed 's/^[A-Z ]\+\([A-Z][a-z]\)/\1/' | sed 's/"//g')

    if [[  -z $NAME ]]
    then
        NAME=$( grep $n c.txt  -B10 | grep '^[A-Z]\+' | head -n1 | sed 's/^[A-Z]\. //' | sed 's/[A-Z][a-z].*//g')
    fi
    #Get Handle
    HANDLE=$( echo $NAME | tr '[A-Z]' '[a-z]' | sed 's/ /-/g' | sed "s/$/-${n}/"| sed 's/[A-Z][a-z]//' )

    #Get Price
    PRICE=$(grep "$n" c.txt -A6 | grep -P '${n} $\d+\.\d+' -o | grep -P '\d+\.\d+' -o)
    if [[  -z $PRICE ]]
    then
        PRICE=$(grep "$n" c.txt -A6 | grep -P -o '\$\d+\.\d+' | sed 's/\$//g' |tr ' ' '\n' | head -n1)
    fi
    # Do Var Price Math
    FIFTEEN=$( echo "scale=2; (${PRICE}) * .15" | bc )
    VARPRICE=$( echo "scale=2; ${PRICE} - $FIFTEEN"| bc )
    # Add Sku to Name
    NAME=$(echo $NAME | sed "s/$/ - ${n}/" )
    if [[ ! -z $n ]]
    then
        FINAL=$(echo "\"$HANDLE\", \"$NAME\" , \"$DESCRIPTION\", \"Vendor\", \"\" , , \"\", \"Title\", \"Default Title\", , , , , \"$n\", ,,,,,\"$VARPRICE\",\"$PRICE\"")
    fi
    if [[ ! -z $FINAL ]]
    then
        echo $FINAL
    fi
done

```


## A note about BSD tools vs GNU tools

I started doing this on my macbook air, Mac OS X uses the bsd cousins of grep and sed. More and more, for various reasons I saw myself using ggrep and gsed, rather than grep and sed. Mostly because of how the BSD sed deals with new lines, and because on occasion I wanted to use PCRE regex with grep, ie the -P flag. Eventually I fully switched to a linux enviroment, using the GNU toolset. The final straw actually had nothing to do with the tools, the resolution\screen space on MacBook Air, didn't allow me to see the lines the way I wanted them in a vimdiff. So I switched to a debian system in the final rounds of the script.

# Hey Guys Don't Forget about my poor cats

My cats, they are [blind](https://www.instagram.com/p/BZ7QyBZjbu0/?taken-by=bsdpunk "Felicity"), they are missing [limbs](https://www.instagram.com/p/BZ7Kw9qjG5E/?taken-by=bsdpunk "Charlie"). Please purchase a book about [regex](https://www.amazon.com/gp/product/0596528124/ref=as_li_qf_sp_asin_il_tl?ie=UTF8&tag=bsdpblog-20&camp=1789&creative=9325&linkCode=as2&creativeASIN=0596528124&linkId=889140c1615f2cd3e4373dfe9ebc0be7 "Regex Book Amazon") Or a book about [successful coders](https://www.amazon.com/gp/product/1430219483/ref=as_li_qf_sp_asin_il_tl?ie=UTF8&tag=bsdpblog-20&camp=1789&creative=9325&linkCode=as2&creativeASIN=1430219483&linkId=b43558af067f9b7ba9b51c625832d542 "Coders At Work, Amazon")

# 
