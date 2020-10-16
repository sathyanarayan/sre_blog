---
layout: post
title:  "Developing SLO and SLI"
date:   2020-10-15 03:12:00 +0530
categories: SRE
--- 

Make Sure you have read [previous post] [previous post] before reading this one

The process of developing SLIs and SLOs for a user trace can be broken into couple of  steps.

* what types of SLI you want to measure for your journey, and set down high level specifications for these SLIs
* Describe in detail what the events you' want to measure, any validity restrictions to the scope of the SLI to a subset
of events, and what makes an event good and what are the valid events.
* Weigh the pros and cons of the various measurement strategies, and figure out where and how the SLI will be measured
* SLI implementations need to be detailed enough that someone could build or configure monitoring infrastructure to gather 
the data without needing to consult 
* Document on the failure modes that your SLIs won't capture, hopefully there won't be many.
* choose your measurement window and set some SLO targets, either based on historical data or can estimate based on the 
business requirement

[previous post]:/sre/2020/10/14/choose-good-sli.html