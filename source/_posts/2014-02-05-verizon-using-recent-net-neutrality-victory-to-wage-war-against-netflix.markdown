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

PS> a number of folks have questioned the expertise of the support individual.  I completely understand.  I'm not a networking expert, but I did want to share 2 more pieces of data that I think are significant:

Traceroute from Residential Side:

```
Tracing route to iscanonline.com [23.21.158.115]
over a maximum of 30 hops:

  1    <1 ms    <1 ms    <1 ms  192.168.1.1
  2     7 ms     7 ms     8 ms  L100.DLLSTX-VFTTP-65.verizon-gni.net [173.74.57.1]
  3    10 ms     6 ms     9 ms  G0-5-2-0.DLLSTX-LCR-21.verizon-gni.net [130.81.190.204]
  4    16 ms     9 ms    10 ms  so-5-0-0-0.DFW9-BB-RTR1.verizon-gni.net [130.81.199.34]
  5    10 ms     9 ms     9 ms  0.xe-3-3-0.BR2.DFW13.ALTER.NET [152.63.100.5]
  6     9 ms    10 ms     9 ms  204.255.168.158
  7    10 ms     9 ms    10 ms  ae-1.r08.dllstx09.us.bb.gin.ntt.net [129.250.3.27]
```

Traceroute from Business line (1 mile away)
```
 traceroute to iscanonline.com (23.21.158.115), 64 hops max, 52 byte packets
 1  192.168.1.1 (192.168.1.1)  18.036 ms  1.326 ms  2.318 ms
 2  l100.dllstx-vfttp-93.verizon-gni.net (71.244.30.1)  5.870 ms  5.211 ms  5.193 ms
 3  g0-5-0-2.dllstx-lcr-21.verizon-gni.net (130.81.138.12)  7.400 ms  67.679 ms  10.605 ms
 4  so-5-0-0-0.dfw9-bb-rtr1.verizon-gni.net (130.81.199.34)  12.062 ms  6.652 ms  17.799 ms
 5  0.xe-3-3-0.br2.dfw13.alter.net (152.63.100.5)  7.207 ms  7.858 ms  9.616 ms
 6  204.255.168.158 (204.255.168.158)  7.435 ms  7.256 ms  10.366 ms
 7  ae-1.r08.dllstx09.us.bb.gin.ntt.net (129.250.3.27)  7.365 ms  10.160 ms  9.083 ms

```
