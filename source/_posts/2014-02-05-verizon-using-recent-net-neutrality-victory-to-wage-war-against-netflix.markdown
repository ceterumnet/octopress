---
layout: post
title: "Verizon using recent Net Neutrality victory to wage war against Netflix"
date: 2014-02-05 07:25:50 -0600
comments: true
categories:
---
I usually don't post articles about current affairs.   However, a recent series of events has inspired me to write about this.

Towards the end of January, the president of our company - <a href="https://www.iscanonline.com">iScan Online, Inc.</a>, was complaining that our service was experiencing major slowdowns.  I investigated the issue, but I couldn't find anything wrong with our production environment.  We were stumped.

One evening I also noticed a slowdown while using our service from my house.  I realized that the one thing in common between me and our president was that we both had FiOS internet service from Verizon.

Since we host all of our infrastructure on Amazon's AWS - I decided to do a little test - I grabbed a URL from AWS S3 and loaded it.

40kB/s.

WTF.

I also noticed that our Netflix streaming quality is awful compared to just a few weeks ago.

Next, I remoted into our office - about a mile away from my house.  I tested the same link -

5000kB/s.

WTF.

So I contacted Verizon support over their live chat.

Verizon had me do a speedtest.

75Mb/s.

He says "You have excellent Bandwidth - is there anything else I can help you with?"

I replied - "Yes.  Why are these files slow..."

So he proceeded to walk me through various troubleshooting:

* "reboot your router..."
* "make sure your system has latest updates...
* "change your wifi channel"

After about 30 minutes of this - I grew impatient.  I explained to him that there was something limiting the speed on their side.  He remoted into my system with a screen sharing tool, and I showed him my remote screen to the connection at the office.  He kept on saying that bandwidth is different for different locations etc...

That's when I decided to press him.  Here is a screen capture of the final part of our chat:

{% img /images/verizon_fail.png %}

Frankly, I was surprised he admitted to this.  I've since tested this almost every day for the last couple of weeks.   During the day - the bandwidth is normal to AWS.  However, after 4pm or so - things get slow.

In my personal opinion, this is Verizon waging war against Netflix.  Unfortunately, a lot of infrastructure is hosted on AWS.  That means a lot of services are going to be impacted by this.
