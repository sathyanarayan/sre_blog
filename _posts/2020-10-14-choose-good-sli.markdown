---
layout: post
title:  "Selecting Good SLI"
date:   2020-10-14 07:12:00 +0530
categories: SRE
--- 

Make Sure you have read [previous post] [previous post] before reading this one

In this post we will look into characteristics of monitoring metrics that can make them useful as SLIs and compare 
these against other metrics that are less useful. The choice of where to measure an SLI is a key variable, 
will also cover the main ways we can measure an SLI and compare their pros and cons. In short :
* Main ways of choosing and measuring SLI
* Understanding common SLI specifications
* Reasons for limiting the number of SLIs per user trace
* Aspirational vs achievable SLOs and why constant iteration is necessary.

### Measuring SLI's
One of the major challenges of specifying SLIs in the real world is service complexity. The service may be composed of 
tens of microservices and have hundreds of HTTP or RPC endpoints, but having hundreds of SLIs can lead to operational overload
and important signals being lost due excessive noise. If you remember we discussed about happiness tests, it says that 
an SLO should be able to divide between satisfied and un-satisfied customer. The service owners must find some way of 
approximating the happiness of the customers with metrics which can directly measure about the service. Not all metrics 
are good approximations of customer happiness.How do you make sure the metric you choose measures the users experience as
closely as possible , for that you should think about your service's reliability from customers perspective.It doesn't 
matter if it's your database being down or your load balancers sending requests to bad backends. From the user's perspective, 
these conditions all leads to website being down and customer being un-happy. Similarly, it doesn't matter which 
component of your service is overloaded. The users complaint is the website is slow. If we can find a way of 
quantifying the website does not load or this website is too slow from our monitoring data, we can use this data to 
approximate how happy or unhappy our users are in aggregate. These quantifiable metrics become our SLIs. It's always better
for the metric you are measuring be having linear relationship to customer happiness.The predictability of the relationship
is crucial because we'll be making the re-engineering efforts based on this data. 

Good SLI is to adhere to or should have following properties : 

* A good SLI is the one that has a predictable relationship with customer happiness
* The SLI should aim to answer the question, is the service working as the users expects
* SLI should be expressed as a ratio of two numbers, good events divided by valid events.
* SLI be aggregated over a reasonably long time window, to smooth out noise

Most services running in production will already have monitoring setups done, system metrics like load average, 
CPU utilization, memory usage, or bandwidth are commonly graphed and visible on monitoring dashboards. It's tempting to 
consider these metrics when looking for SLIs because sharp changes in them are often associated with outages. 
But that's not a suggested. If you think about it from the perspective of customers, they don't directly see our
CPUs pegged at 100 percent or Disk being full. They see our service responding slowly. These may seem like good SLIs, 
but just like CPU utilization example above, the data is noisy, and there are many reasons why large changes could occur.
It's pretty clear, none of these metrics have a predictable linear relationship with user happiness. 

A key engineering decision underlying any SLI is the choice of where and how to measure it. Lets see different ways of 
how we could measure the SLI's. 

* One way is deriving SLI metrics by processing the server side request logs. This method gives a way to track the reliability
of complex user journeys with many request response interactions during a long session. If we're just begining to set up 
SLO's, we can often process request logs to analyze, and get an idea of historical performance. if the SLI needs some 
built in logic to determine or differentiate events as good, this logic can be written into the processing jobs/systems,
and exported as a much simpler good events counter. This method would require engineering effort to build the processing system.
This methodology is not good for triggering emergency response, say due to various reasons , say 1) Delay , as the request 
has to be served and then fed to your processing systems to analyse so on and so forth 2) What if the request didnt reach your system
or say response didn't go out of your system, this cant be handled by this method 

* Most interactions that involve users requests that the service responds to, can be measured using the front-end load balancing infrastructure. 
This is commonly the closest point to the user that is mostly within our control, so fewer requests, will go unmeasured. Y
Usually the  Cloud provider will generally have detailed metrics and historical data readily available, so this option 
may need little to no engineering effort to get started. The downsides are that load balancers are stateless and thus 
have no way of tracking sessions, and they don't have any insight into the response data. We would have to depend on the metdata.
These can be circumvented to an extent from the feature enhancement provided by the cloud providers , say for an example 
using [AWS application load balancer] [AWS application load balancer]

* Using Synthetic clients, it can emulate a user's interactions with your service confirming that a full journey can be 
completed successfully from a point outside your immediate infrastructure. And verifying that the responses received 
are correct. But a synthetic client is only an approximation of user behavior, user behaviour could always turn/be 
unexpected. Covering all user journey via a synthetic client will be difficult task.

* Another method would be to develop a client side agent and export metrics from client application to our systems and 
then analyze at our end, even though it gives or better perspective from user perspective , it would costs heavily as 
we would be consuming more resources at the client side. This provides far and away the most accurate measure of your 
user experience and can even help you gauge the reliability of the third parties like CDNs or payment providers in 
your user journeys. Clients can incur significant measurement latency,could compromise battery life and user trust etc.


### Commonly Used SLI's
If your service is responding to a user's request, then you almost certainly want to measure how fast those responses are 
and how many of them are unsuccessful. If you have mechanisms that can degrade the quality of the response a user receives
to alleviate excess load on your service, measuring how often this occurs is also a good one. If the user is expecting 
some data to be crunched on their behalf either explicitly or implicitly, then they will probably have expectations that 
processing completes within a reasonable time frame and processes all the data it should have without errors. To quantify 
these expectations by measuring the freshness and correctness of the processed data and the coverage and throughput of 
the pipeline doing the processing. Lastly, if the user is giving you some of their data to store and they have 
expectations that they will be able to retrieve that data again, you may want to measure the durability.

Now lets consider different SLI Types, to track the reliability of a request response interaction in a user journey we 
should be measuring the `availability`, `latency`, and `quality`. For the SLI Type : Data Processing that is if user expects 
the system to process and provide data, our SLI's should be measuring  `freshness`, `correctness`, `coverage`, and `throughput`. 


Its always better to stick on to 5 to 6 SLI's, just to make it clear setting SLI's doesnt mean you should setup less metrics 
to monitor your systems operation.This can become harder and harder to stick to, and on re-iterating, it's important to 
remember that not all of your monitoring metrics make useful SLIs, even if they have reasonable correlations to outages.
The way we think about it is that a degradation in your SLIs is an indication that something is wrong. 
But once the degradation has become bad enough to provoke some form of incident response, we'll need those other systems 
to help you figure out what is actually happening. Try to always aggregate the SLI's , it would help you have lesser 
number of SLI's.

Once you have set the SLI's you could set your SLO's , always make sure you have a continous review and improvement process 
also setup.


[previous post]:/sre/2020/10/13/measuring-and-targeting-reliability.html 
[AWS application load balancer]:https://docs.aws.amazon.com/elasticloadbalancing/latest/application/introduction.html