---
layout: post
title: Parsing postal addresses
categories:
- blog
---

The rapid pace of the world around us often makes us take many small miracles for granted.
For example, I can go on [Amazon Japan](http://amazon.co.jp), order some gadget, and have it delivered to my door within days (eventually, [within minutes](http://www.youtube.com/watch?v=98BIu9dpwHU)).
We only get to see the delivery guy (or girl, if you're lucky) dropping off the package, but there are many things happening in the background:

1. Amazon packages the item and prints my address on the outside.
2. Amazon entrusts the package to some domestic delivery company (in Japan, typically [Yamato](http://www.kuronekoyamato.co.jp/en/) or [Sagawa](http://www.sagawa-exp.co.jp/english/)).
3. The delivery company interprets the address, in particular, looks for the postcode, prefecture, city and street address.
4. The delivery company, which has depos all around Japan, sends the package to a depo location that is closest to me, based on the above.
5. A driver from the depo goes out to the address and delivers the package.

In this article, I'd like to focus on the third step: interpreting an address.
This isn't something that is restricted to delivery companies: there are many applications that involve finding out where exactly something is.
This particular task is known as [geocoding](http://en.wikipedia.org/wiki/Geocoding) and it is part of a broader problem area known as [geolocation](http://en.wikipedia.org/wiki/Geolocation).

But let's get back to the specific problem and formulate it.
We have a string that contains an address - nothing but the address.
For example:

    Level 7
    3 Thomas Holt Drive
    North Ryde
    NSW 2113, Australia

We'd like to interpret that address.
More specifically, we'd like to determine:

 - the street number (3)
 - the street name (Thomas Holt Drive)
 - the city (North Ryde)
 - the state (New South Wales)
 - the postcode (2113)
 - the country (Australia)
 - any other relevant information (such as "Level 7" or PO box numbers)

If that seems too easy, then this is the same address:

    L7, 3 Thomas Holt Dr., Nth. Ryde, NSW 2113

It turns out that this is a [fairly](http://stackoverflow.com/questions/518210/where-is-a-good-address-parser) [well-discussed](http://stackoverflow.com/questions/16413/parse-usable-street-address-city-state-zip-from-a-string) [topic](http://stackoverflow.com/questions/11160192/how-to-parse-freeform-street-postal-address-out-of-text-and-into-components) on StackOverflow. Here's a brief summary of the answers:

 - [Regular expressions](http://docs.python.org/2/library/re.html) can get you far, but not all the way
 - [Some](http://pyparsing.wikispaces.com/file/view/streetAddressParser.py) [Python](https://github.com/SwoopSearch/pyaddress) packages exist, but YMVA
 - There are [several](https://developers.google.com/maps/documentation/geocoding/) [online](http://smartystreets.com/products/liveaddress-api/extract) [services]() for parsing address (some free, some not free) (read a [review](http://blog.programmableweb.com/2012/06/21/7-free-geocoding-apis-google-bing-yahoo-and-mapquest/))

The online services tend to give the best results, but it's not practical to use them if, for example, you want to parse several million addresses in a hurry. And cheap. In that case, you'd have to reinvent the wheel.

Luckily, you don't need to start from scratch.
Many postal services maintain standards documents that dictate how exactly addresses need to be formatted (for example, [USPS Publication 28](http://pe.usps.gov/cpim/ftp/pubs/pub28/pub28.pdf)). 
This document is a goldmine of information that contains goodies like:

 - Acceptable abbreviations for US street types and states
 - The expected order of address components
 - Mappings between English and Spanish address terms (for addresses in the US Territory of [Puerto Rico](http://en.wikipedia.org/wiki/Puerto_Rico)).

By treating these documents as specifications, it's possible to write a fairly robust parser for a particular country.
Furthermore, the specifications of different countries share many common points.
For example:

 - Addresses in English-speaking countries generally follow the same order (street, city, state, postcode, country)
 - Street types in English-speaking countries tend to be pretty common, such as the usual "street", "road", "avenue" and the more exotic "boulevard" and "esplanade".
 - British and Canadian postcodes follow a similar pattern
 - ... and [many more](http://www.columbia.edu/~fdc/postal/).

That's all for today, but in future articles, I'll be looking at the actual mechanics of parsing an address, as well as other essential supplementary materials.
Stay tuned!
