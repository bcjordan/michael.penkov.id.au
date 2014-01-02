---
layout: post
title: Reinventing the Wheel (in a useful way)
categories:
- blog
---

I procrastinate a lot.

I try to do it in an effective way: I let other people decide things that may be worth looking at, and then take my pick from that.
That saves me the effort of wading through a lot of generally uninteresting stuff that I don't need to look at, leaving more time for fun & rewarding procrastination.
In the past, I spent a lot of time on [slashdot](http://slashdot.com) (I still do occasionally, just not as often).
More recently, I've been a huge fan of [reddit](http://reddit.com).
On top of doing a great job of finding interesting things on the net, reddit has a knowledgeable community that provides a lot of otherwise unavailable context through user commentary.
For those that haven't heard of it, here's [reddit](http://reddit.com) in a nutshell:

<iframe width="560" height="315" src="//www.youtube.com/embed/tlI022aUWQQ" frameborder="0" allowfullscreen></iframe>

There's a [videos subreddit](http://reddit.com/r/videos) that contains a live feed of videos that reddit's viewers found interesting.
You click on a video to watch it, and when it finishes playing, you click another one (or get back to work if you feel too guilty about procratinating any further).
I found the constant clicking on videos to be a hurdle in my quest for optimal procrastination, so I made what eventually became an [app on Google's app engine](http://rvytpl.appspot.com).
It makes a playlist of the interesting videos so I (and anyone else) could then watch them without having to do a lot of bothersome clicking.
This was back in early 2012.

It turns out that I pretty much invented [reddit.tv](http://reddit.tv).
Except that, you know, [reddit.tv](http://reddit.tv) has been [around since 2009](http://thenextweb.com/2009/04/29/reddit-launches-reddittv/).
Argh.
Turns out I've been (unknowingly) reinventing the wheel.

Reinventing the wheel tends to get a bad rap.
Whenever you hear it being used, it's almost always in a negative way.
Looking back on my experience this time, however, I've found that I've learnt a ton of stuff I wouldn't have otherwise touched.

The big take-away point from the whole exercise was: Web applications.
As a professional software engineer, I've only built desktop apps and code that sits deep inside some library. 
As a researcher, I only code that runs only on my machine. 
Having to write a Web app taught me:

 1. You can do a lot with HTML5 (HTML, CSS and JavaScript) and a compatible browser. It's awesome for showing people stuff, like a demo or a toy. That's just HTML5 - we're not even in proper Web application territory.
 2. Application engines: I started with [Google app engine](http://appengine.google.com). I learnt just enought to get the app going: how to install apps, how to schedule repeating tasks, how to use storage, etc. These days, I tend to use [CherryPy](http://www.cherrypy.org), since it lets me host applications on my local machine. 
 3. Templating engines: again, I started with Django, since that was the default on Google app engine, but moved on to [Mako](http://www.makotemplates.org), since it is relatively simple and part of CherryPy.
 4. [Twitter Bootstrap](http://getbootstrap.org): gives your site an instant 1000% improvement in looks. Also, gently teaches you "the proper way" to do stuff through its use of CSS and [jQuery](http://jquery.com).
 5. Did I mention JavaScript? When used [correctly](http://shop.oreilly.com/product/9780596805531.do), it can be freakin' awesome. Since getting my teeth into it two years ago, I find myself using it in ways I never expected, like making interactive graphs with [d3.js](http://d3js.org) and [ndv3](http://ndv3.org).
 6. [JSON](http://www.json.org): I grew up at a time when XML was all the rage, but I never got over the hassle of parsing XML. It wasn't particularly difficult or anything, but it felt way too involved for something that should be really simple. Reddit can serve its content via JSON (instead of the usual HTML for human consumption) - learning about simpler alternatives to XML was a relief.
 7. [tkInter](https://wiki.python.org/moin/TkInter) and [distutils](http://docs.python.org/2/library/distutils.html): the first incarnation of the app was a command-line [Python](http://www.python.org) script. You had to download it, open up a command-line and unleash some CommandLine-Fu to get anywhere. I made it slightly more accessible by writing a simple GUI in tkInter and an installer using distutils. Then a friend suggested I scrap the desktop app and switch to a Web app, which was way more accesible. The rest, as they say, is history.
 8. [git](http://git-scm.com) and [GitHub](http://github.com): while I had already started using git by the time I began working on the app, using it as part of this project helped me get more comfortable with it.

Overall, I found it an invaluable learning experience.
Perhaps one day I'll invent a wheel nobody has actually invented yet, for a change.
