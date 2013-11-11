---
layout: post
title: Synchronizing other calendars with Google
categories:
- blog
---

Scheduling things can be surprisingly difficult for a graduate student - you need to keep track of classes, seminars and various other meetings.
Over the years, I've come to rely on [Google Calendar](http://calendar.google.com) to help me organize myself from day to day.
I keep all my appointments there - having them all in one place makes them more difficult to miss, and makes scheduling future appointmets a lot easier.
Unfortunately, not everyone uses Google Calendar: for example, [my lab](http://lmd.ist.hokudai.ac.jp) uses an antique version of [xoops](http://xoops.org/); my [gym](http://reebokcrossfitssc.com/) uses [supersaas.com](http://www.supersaas.com/).
This means I need to keep track of three calendards when making a new appointment:

1. Check my Google calendar for personal appointments
2. Check my lab's calendar for relevant meetings
3. Check my gym's calendar for sessions that I've booked myself into

I have enough trouble keeping up with one calendar, let alone three.
Fortunately, it's fairly easy to import calendar entries into Google calendar, even if the source of the import doesn't cooperate explicitly.

For xoops, I wrote a [Python](http://python.org) script that:

1. Crawls our lab's calendar for recent entries
2. Filters out entries that are relevant to me
3. Adds them to my Google calendar
4. Deletes Google calendar entries that were removed on the lab side 

Python makes it all really easy. 
The [requests](http://python-requests.org) makes fetching remote content a one-liner (I used [urllib](http://docs.python.org/2/library/urllib.html), but that was before I discovered requests).
[Regular expressions](http://docs.python.org/2/library/re.html) can make parsing structured text very simple (although given that it's HTML, using something like [lxml](http://lxml.de) would have been even better).
Working with dates and times is also easy thanks to the [datetime](http://docs.python.org/2/library/datetime.html) module.
Finally, Google supports a [Python API](https://developers.google.com/google-apps/calendar/v2/developers_guide_python) that allows you to programmatically update your calendar (among many other things). The 2.0 API is somewhat dated; Google released [Python client libraries for the newer 3.0 API](https://developers.google.com/api-client-library/python/) after I wrote the script.
So that's all the hard work out of the way - all you need to do is to glue the components together.
Here's the [source](https://github.com/mpenkov/cal2google).
I run it as part of a [cron](http://en.wikipedia.org/wiki/Cron) job on my [Linode](http://linode.com) periodically.

For supersaas, the deal is pretty similar, except I got a little lazy and didn't investigate crawling the calendar remotely.
Their calendar sends notifications of new appointments via email, so I wrote a quick-and-dirty script that hits my inbox once every couple of hours and processes new notifications using some regular expressions.
The component that pushes the entries to Google calendar is virtually identical.
Here's the [source](https://github.com/mpenkov/supersaas2google) - like I said, it's pretty quick-and-dirty, but works well enough.
