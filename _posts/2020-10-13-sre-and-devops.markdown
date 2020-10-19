---
layout: post
title:  "Site Reliability Engineering (SRE) Concepts"
date:   2020-10-13 07:12:00 +0530
categories: SRE
--- 

### What is SRE, how it differs from DevOps ?
DevOps is a set of philosophies, practices, and tools that help teams/organizations quicken the development cycles, 
but keeping in mind about the service availability. Lets Look into DevOps principles: 

* Reduce operational silos(breaking down barriers across teams and collaborate)
* Accept Failure as Normal
* Implement gradual changes 
* Leverage tooling and automation 
* Measure Everything 

Whereas SRE (Site Reliability Engineering) is the way of implementing the above philosophy or principles. In other words 
SRE is a class that extends the DevOps interface and implements the above concepts. Let’s see how SRE teams implement 
the above principles:

`Reduce operational silo`: SRE shares the ownership of the production system with the developers. SRE team use same tools 
so that everyone working on the production system will have same view and understanding. The responsibility of the production 
system to function efficiently is shared by both the SRE and Dev team.

`Accept Failure as Normal`: If there has been an issue that affected the production system, post the mitigation, SRE Team 
does postmortems, so that issue doesn’t occur again. SRE team calculates the error budgets and make sure service never 
exceeds the calculated budget

`Implement gradual changes`: It's never suggested to deploy the changes directly into production, usually the change to 
production follow the below cycle : 
* Raise a code review with the peers
* Deploy to pre-prod stage (In this stage we could use synthetic clients to replay the traffic and see if fix is performing as expected)
* The change should be validated by integration/QA tests in the pipeline (performing canary tests)
* Deploying to pre-production and supplying with small percentage of  real time traffic and monitor.
* Deploy to production finally

`Leverage tooling and automation`: Automate and move out should be the motto, should strive to automate and make sure 
service is self resilient. An ex: would be to implement canaries to automatically check the functionalities, or implementing
 auto-rollback monitors (it would rollback to previous stable version of your service in case of any issue observed). 

`Measure Everything` :  Implementing the monitoring systems,metrics to monitor the service health.

In this and subsequent posts ill try to cover couple of important SRE concepts, after reading these series of posts 
you should be able to implement these concepts and make your service more reliable. 

### Watch this video : 

<iframe width="560"
        height="315"
        src="https://www.youtube.com/embed/uTEL8Ff1Zvk"
        frameborder="0"
        allow="autoplay; encrypted-media"
        allowfullscreen>
</iframe>


### Topics being covered :
* What do you mean by SLI SLO and SLA 
* [Measuring and Operating for Reliability] [Measuring and Operating for Reliability]
* [How to choose a Good SLI] [How to choose a Good SLI]
* [Developing SLO's and SLI's for a new Service] [Developing SLO's and SLI's for a new Service]
* [Quantifying Risks and Consequences of not having SLO's] [Quantifying Risks and Consequences of not having SLO's]

### What is Service Reliability ? 
I have often seen engineers getting confused with the `Reliability` and `Availability` of a service, so lets try defining them: 

* `Reliability`: Reliability refers to the probability that the service will meet certain performance standards and provide 
an expected response within the desired time duration. Reliability helps to  understand how well the service will be available
in different situations.

* `Availability`: Availability refers to the percentage of time that the service remains operational under normal circumstances.

The first major principle is that the most important feature of any system is its reliability. If your customers cannot 
use the services, they can’t take advantage of the features built. This doesn’t mean that the system should never fail 
and should be 100% reliable, setting or trying to make service 100% reliable is also not right. This is costly and often 
unreachable target to set. A more realistic goal is that the system should meet the expectations of its users, a small 
amount of isolated failure will not cause any issue. If there are multiple failures, you can easily lose the trust and 
respect of the user base and in-return, this can have disastrous consequences on your business. If your system must show 
it is meeting the user's expectations of reliability, then we have to measure their experience of its reliability.

If your users perceive your service to be unreliable, then it is not meeting their expectations, no matter what your logs 
and metrics say. Imagine your support team trying to tell an unhappy user that their problem didn’t actually exist because 
the monitoring systems had detected nothing wrong. The next principle states this: our monitoring does not decide our 
reliability, our users do. 

The final principle is regarding the pursuit of ever-increasing reliability. Architecting a system carefully and following best practices for reliability, 
like running in multiple distributed regions, means that the system has the potential to achieve three nines over a long time horizon. 
To reach four nines, or five nines, it’s not only enough to have talented developers and well-engineered software, we also 
need an operations team whose primary goal is to sustain system reliability through both reactive, like well-trained 
incident response and proactive engineering. They would be involved in the tasks like removing bottlenecks, automating 
the manual processes, and isolating failure domains etc. Each additional nine makes your system 10 times more reliable 
than before, but as a rough rule of thumb, it also costs your business 10 times more. 


### What do you mean by SLI SLO and SLA ? 
Even though the SLI, SLO and SLA are related to each other, each of them have different concepts: 
* SLA or Service Level Agreement is a contract that the service provider promises to the customers on service performance, availability, etc.
* SLO or Service Level Objective is a goal/threshold set by the service provider, and service owner's strives to maintain it. 
SLO’s are set in such a way to catch an issue before it breaches the SLA. SLO's are supposed to have tighter thresholds than SLA.   
* SLI or Service Level Indicator is a metric-measurement the service provider uses for the meeting the goal/SLO.

Therefore, it is to the best interest of the service owners to catch an issue before it breaches the SLA, so that the 
technical team has time to fix it. These thresholds are the SLOs, service level objectives. They should always be tighter 
than the SLAs because customers are usually impacted before the SLA is actually breached. Violating SLAs come at significant cost. 
So summing up, an SLA is an external promise that comes with consequences, often monetary. While an SLO is effectively an 
internal promise to meet customer expectations, and SLI is the metric you considered for the SLO.

Let's consider and example, if the service SLA is defined as response latency will not be more that 400ms, on analyzing 
the service we came to know that most of the response are usually served within 250 ms, so we could set our slo to be at 
250ms, and in-case of any issue, we could catch it earlier. So in this above case SLA is of 400ms, SLO is set at 250ms 
and SLI: is Latency metric.

[Measuring and Operating for Reliability]:/sre_blog/sre/2020/10/13/measuring-and-targeting-reliability.html
[How to choose a Good SLI]:/sre_blog/sre/2020/10/14/choose-good-sli.html
[Developing SLO's and SLI's for a new Service]:/sre_blog/sre/2020/10/14/developing-slo-sli.html
[Quantifying Risks and Consequences of not having SLO's]:/sre_blog/sre/2020/10/18/2020-10-18-quantifying-cons-slo.markdown
