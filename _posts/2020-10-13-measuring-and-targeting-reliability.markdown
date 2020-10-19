---
layout: post
title:  "Measuring and Operating for Reliability"
date:   2020-10-13 10:12:00 +0530
categories: SRE
--- 
Make Sure you have read [previous post] [previous post] before reading this one 

In this post we’re going to discuss how to measure the desired reliability of a service. We will see what to consider 
when setting SLOs for services and also look at the three principles we could use to measure the desired reliability of 
a service. We will also see:
* how to figure out what we want to promise
* how to figure out the metrics to measure the service reliability and mark as “good”.
* how much reliability is good enough.

### Happiness Test 
How do we tell if the service is working correctly? Let's talk about Amazon, we say amazon app is working fine, when we type
a search query for a product and we get a page of relevant results, or another example is, perhaps when you order in swiggy, 
the route it takes to get your home is relatively efficient. There are many characteristics of reliable services. The common 
theme is that users perceive a service to be unreliable when it fails to meet their expectations, whatever those may be. 
Users whose expectations have not been met tend to get unhappy.

A good rule of thumb to figure out to set SLO targets is through happiness test. The test states that, services need target 
SLOs that capture the performance and availability levels that if barely met would keep a typical customer happy. If your 
service is performing exactly at its target SLOs, your average user would be happy with that performance. If it were any 
less reliable, system would no longer be meeting their expectations and they would become unhappy. If your service meets target SLO, 
that means you have happy customers. If it misses the target SLO, that means you have sad customers.

The challenge is in quantifying and measuring the happiness of your customers since you can't do this directly. It's bad 
if customers are unhappy despite the fact that system appear to be meeting all of the SLOs, in that case the SLO's set are not correct. 

### How to Measure Reliability ? 
Let's explain by considering ours service to be video streaming platform like Netflix, Prime Video. What are the basic 
characteristics a user would check if to consider the service is working or good enough. 
* Once you select a video, it should load quickly or should start playing quickly.
* If user was already watching a movie, it should be uninterrupted and have no issues with playback 
* On hitting netflix.com, landing page should load seamlessly

So which metric do you think we could use?, The time it takes load the landing page, the time to start playing, we want 
those features to be quick. For this, we can measure the request latency, which is measuring how long it takes for a 
request to play back or return response. We refer to this metric and other metrics that measure the level of service 
provided as Service Level Indicators, or SLIs. SLIs, like request latency, are a quantitative measurement or metric of a 
user experience. An another SLI could be the error rate, were we consider the ratio of errors or successes over the total 
number of requests. Regarding SLI’s they are best expressed as a proportion of all valid events that were good. For example, 
the proportion of requests served successfully or the proportion of requests served within z milliseconds.

Once we have the SLI’s defined, we could choose our SLO’s. It’s catchy that new services wouldn’t have historical data 
to set the SLO’s, so it’s totally acceptable not to set SLO’S at first, or we could set SLO’s with reference to 
business criteria and then later come back and modify. SLO is just a target that service owner choose, so once you’ve 
decided on that target, you measure the performance of the SLIs against it over a period of time. Depending 
on what our SLO is, our SLI should instantly tell us,at certain point in time service was good or bad.

The Reliability of the service need not be always same, there are different edge cases per organizations, let's say for 
example, the e-commerce websites they would see huge traffic during Black Friday, Cyber monday days. The teams would put 
extra effort to make sure service is more reliable or say would set the availability to 99.99 from 99.9 and post the event 
they would set it back to normal.

### How Relaible a service should be ? 

If you ever think you need to set reliability target as 100% for your service, it's wrong. There are many trade-offs we 
have to consider when setting a reliability target. If you’re trying to run your service much more reliably than it needs 
to be, you’re slowing down development velocity. It becomes much more expensive to make reliable systems even more reliable. 
So there’s some point at which the incremental cost of reliability exceeds its value. To choose the reliability for the 
service, we need to follow the below steps :
* First, we need to have ambitious but achievable targets. It's derived from understanding what your users need from 
the service and how it currently performing. 
* The stakeholders across products, development, and operations must all agree on those targets. 
* Then, we want to measure how the SLI is performing against target and try to be slightly over-it.

There isn’t actually a straight relationship between service availability and user happiness. Once you’ve passed the 
happiness test, increasing availability will not have an effective ROI. In addition, higher availability costs you more 
, reducing your ability to make changes and release new features. If your service is significantly more reliable 
than an individual customer’s ISP, they’re going to struggle to attribute any failures to you.

Running a service with an SLO is an adaptive and iterative process. The initial SLO might not be as good a fit in 12 months.
New features might not be properly covered by it, and user expectations may not be reflected in the SLO. After all, it’s 
bad if you’re meeting all of your SLO’s and your customers are still unhappy. So, it's better to plan to review the SLO's 
and SLI's definitions after a few months and identify what needs to change. Always be sure to think about all groups of 
your customers, like mobile, desktop, or different geographies, and modify your SLO's accordingly. You should expect to 
regularly, re-evaluate the details of your chosen SLIs and the SLO targets.

### Error Budgets 

Error Budgets is a mechanism for quantifying unreliability of the service. 
Error budgets help Service owners decide when to focus on making a service more reliable. We’ll also look into some of the engineering and 
operational improvements that can help to do that. An error budget is basically the inverse of availability, and it tells us how unreliable your service is allowed to be. If your SLO says that 99.9 percent of requests should be successful in a given time frame(say every 30 days), your error budget allows 0.1 percent of requests to fail. This unavailability can be generated :
* As a result of bad pushes by the dev teams
* Planned maintenance, 
* hardware failures etc.
 
If am considering the error budget in terms of down-time, ie 0.1 percent 
unavailability in 30 days, it would be of ```43.2 mins (0.1/100 x (30 x 24 x 60))``` down time allowed every month. Even though this is just about enough time for the monitoring systems to detect and report an issue, and for a human to investigate and fix it, but just one incident would burn out 100% 
of your error budget, providing you with no more room for any other incident that month.

While confirming the error budgets its always better to get consent from all the parties involved and the leadership. In simple words you could consider error budgets as your salary It’s there to be spent on things you want, such as:
* rolling out new software versions(which might break) 
* releasing new features
* plan downtime
* inevitable failure in hardware, networks, power, etc. 

We shouldn’t overspend it, but it does not mean you shouldn’t spend it at all. If you look at your error budget for your
SLOs over standard time period like 30 days, and if see that we’ve used 90 percent of the error budget, we would have 
used,38.88 minutes of unavailability in that month. In general, we shouldn’t care whether we’ve used 10 percent, 50 percent,
or 70 percent of your error budget in the past 30 days. We should be concerned, if we’ve used more than 100 percent. 
If we have exceeded the budget, we need to take action to make the service is more reliable, and protect users from 
additional unavailability.

SLOs give us this framework to make these trade-offs. Essentially, you want to align incentives for both your dev and SRE 
teams to not go out of error budget. When the service is in SLO, the developers can take risks and push new code and 
features quickly, or the SRE Team can perform the planned maintenance. When it’s not in SLO, they need to be more conservative. 
Basically, we are talking about a trade-off, one is we can increase reliability but it will cost lot more, at the same time, 
we can push faster but we will need better integration tests and automated canary analysis and rollback to keep the error 
budget burn within SLO. SLOs are used to make business decisions about reliability and development velocity. This is why 
they are a common incentive for both product and SRE teams, and why they require not only buy-in from both teams, but 
also executive buy-in.

It's basically an effective process, that would help in proper usage of the error budget, the operations team must have 
the ability to effect change in the dev team’s practices. This means they must have and use authority to halt feature 
launches when there’s no remaining error budget. SREs need to feel that they have a stake and a role in getting new software 
deployed as quickly and easily as feasible, which is what the dev teams want. The developers need to feel that they have 
a stake and a role in making the service reliable which is what the SREs want. A strong partnership and effective communication 
using error budgets and engineering collaboration is required to maintain service reliability.

### Watch this video : 

<iframe width="560"
        height="315"
        src="https://www.youtube.com/embed/y2ILKr8kCJU"
        frameborder="0"
        allow="autoplay; encrypted-media"
        allowfullscreen>
</iframe>


### How do we make the service more reliable ? 
There are different methods to improve reliability, some of them are:
* Rolling out changes gradually, there by reducing the blast radius.
* Deploying you services in multiple availability zones.
* Make sure your pipeline has integration/QA tests to catch the bugs 
* Having Endurance and Failure Mode testing approval workflows in you pipelines 
* Enhance the process involved in mitigation and resolution of the issue , in result reducing the TTR (Time to resolve) or 
try reducing TTD (time to detect)
* Try to automate the manual steps involved in Standard Operating Procedure (SOP)
* Periodically report on worst customers or worst region or another category to find cases where the error budget is not evenly distributed. 
Then focus on extra reliability efforts in those regions.
* Standardizing infrastructure. Software and hardware don't always go well together. Product teams could be developing on 
certain hardware specifications and architectures, but when pushed to production, they may find that some services don't work 
on different infrastructure.
* SRE Team getting involved during the design phase of the system itself 
* Always make sure postmortem is done and action items are tracked to closure 



[previous post]:/sre_blog/sre/2020/10/13/sre-and-devops.html
