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

### Example :

![sre_blog_post3_1](/images/sre_blog_post3_1.png)

The above is an example of user journey of viewing the product details in the app. I have considered the above route and 
keeping it simple, in actual scenario the response would be obtained via connecting to multiple micro services. What do you
think the users expectation would be on selecting the product seen in the search page? what would make them happy and 
unhappy?. The clients browser/app will make an https request to the server, which the loadbalancer will route to one of the
web servers to serve the response. The JS and CSS assets will be pulled via the CDN by the client, and the page is compiled at
client side and full product detail page is shown to the user. From user perspective, user would be happy only if the page 
loads as quickly as possible, displaying all the details. This example is more over like a Request/Response interaction,
so we want to measure both the availability and latency metrics. This raises a question what does successful loading of page 
or when we say quickly loaded means?. When we define the SLI it shoould be clear , what, where and how are 
we measuring the SLI's. When you are pre-paring a document of the SLI's , it should be pretty clear , anyone with that 
document should just be able to start without any concerns or queries. Coming back to this scenario so we have decided 
availability and latency are the metrics we need to measure, for measuring the availability , lets say we need to check 
if the page/product page was successfully served or not for this we could depend on the status code response, which would get 
recorded in the loadbalancer as it returns the response. To be more precise we could tell lets say, consider all requests 
with request path /api/product<id>,where response is not 500 and 429 should be considered as successful. Now considering the 
next one latency : all response within y milliseconds should be considered as good response. For this we could again use the LB
for GET of https request to product page, which all where less than y milliseconds.

There could be lot of gaps, in the settings we have done we always need to walk through the user journey and consider 
if the chosen measurement strategy will observe in the SLI metrics, when the infrastructure fails, ie lets say if the load 
balancer is down, out SLI metrics wont be populating and we wouldnt be knowing whats happening. This clearly calls out that 
the postmortem or Root cause analysis is always important, as it would not only able to help you find the issue but also help in
placing the right metrics. One solution i could use is create a one-box stage in my pipeline to which ill be supplying 
synthetic traffic and would be monitoring the response. Capturing all the falls at first go for the complex systems are always 
unrealistic, it would always be an incremental enhancement.

[previous post]:/sre_blog/sre/2020/10/14/choose-good-sli.html
