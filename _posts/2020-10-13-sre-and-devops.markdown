---
layout: post
title:  "Site Reliability Engineering (SRE) Concepts"
date:   2020-10-13 07:12:00 +0530
categories: SRE
--- 

### What is SRE, how it differs from DevOps ?
DevOps is a set of philosophies, practices, and tools that helps teams/organization’s quicken the development cycles, 
but always keeping in mind about the service availability. Following are set of principles in DevOps: 

* Reduce operational silos(breaking down barriers across teams and collaborate)
* Accept Failure as Normal
* Implement gradual changes 
* Leverage tooling and automation 
* Measure Everything 

where as SRE (Site Reliability Engineering)is the way of implementing the above philosophy/principles, in other words 
SRE is a class that extends DevOps interface and implements the above concepts. Lets see how SRE teams implements the 
above principles
 
`Reduce operational silo` : SRE Shares the ownership of the production system with the developers, SRE team
use same tools so that everyone has same view as that for working with production. That is the responsibility of the production system
to function efficiently is shared by both the SRE and Dev team.

`Accept Failure as Normal` : If there has been an issue that affected the production system, post the mitigation and 
once the issue has been resolved SRE Team does postmortems, so that issue doesn't occur again. SRE team is responsible
for calculating the error budgets and making sure service never exceeds the calculated budget.

`Implement gradual changes` : It never suggested to deploy the changes directly into production, usually the change to 
production follow the below cycle : 
* Raise a Code review with the peers (Note: each code review should be focused on addressing a single issue or fix)
* Deploy to pre-prod stage (In this stage we could use synthetic clients to replay the traffic and see if fix is performing as expected)
* The change should we validated by integration/QA tests in the pipeline (performing canary tests)
* Deploying to stage before production and supplying with small percentage of  real time traffic and monitoring
* Deploying to production finally  

`Leverage tooling and automation` :  Automate and move out should be the thought process of SRE, should strive to 
automate and make sure service is self resilient. An ex: would be implementing canaries to automatically check the functionalities,
or implementing auto-rollback monitors (it would rollback to previous stable version of your service if any issues is found). 

`Measure Everything` :  Implementing the monitoring systems,metrics to monitor the service health.

In this and subsequent posts ill try to cover couple of important SRE concepts, after reading these series of posts 
you should be able to implement these concepts and make your service more reliable. 

Watch this video : 

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
* Quantifying Risks to SLO's
* Consequences of not having SLO's 

### What is Service Reliability ? 
I have often seen engineers getting confused with the `Relaiblity` and `Availability` of a service, so lets try defining them: 

* `Relaiblity` : Reliability refers to the probability that the service will meet certain performance standards and provide 
expected response for the desired time duration. Reliability can be used to understand how well the service will 
be available in different situation.

* `Availability` : Availability refers to the percentage of time that the service remains operational under normal 
circumstances. 

Now getting bit deeper into the concepts, 

The first major principle is that the most important feature of any system is its reliability. 
if your customers are unable to use the services, they can't take advantage of any of the features built, this doesn't mean 
that the system should never fail and should be 100% reliable, setting or trying to make service 100% reliable is also not right. 
This is a costly and often unreachable target to set. A more realistic goal is that the system should meet the expectations of its users. 
, a small amount of isolated failure will not cause any issue. If there are multiple failures you can easily lose the trust and respect of 
 user base, and in turn, this can have disastrous consequences for your business. If your system must show it is meeting 
its users expectations of reliability, then you have to measure their experience of its reliability. 

If your users perceive your service to be unreliable, then it is not meeting their expectations, no matter what your logs and metrics say. 
Imagine your support team trying to tell an unhappy user that their problem didn't actually exist because your monitoring 
systems had not detected anything wrong with your system. 
The next principle states this clearly, our monitoring does not decide our reliability, our users do. 

The final principle concerns the pursuit of ever increasing reliability. Architecting a system 
carefully and following best practices for reliability, like running in multiple globally distributed regions, means that 
the system has the potential to achieve three nines over a long time horizon. To reach four nines, or five nines, it's not only enough to 
have talented developers and well-engineered software, we also need an operations team whose primary goal is to sustain 
system reliability through both reactive, like well-trained incident response and proactive engineering, things like removing 
bottlenecks, automating processes, and isolating failure domains. Each additional nine makes your system 10 times more reliable than before. 
But as a rough rule of thumb, it also costs your business 10 times more. 


### What do you mean by SLI SLO and SLA ? 
Even though the SLI, SLO and SLA are related to each other , each of them have different concepts : 
* SLA or Service Level Agreement is a contract that the service provider promises to the customers on service performance, availability, etc.
* SLO or Service Level Objective is a goal/threshold set by the service provider, and service provides strives to maintain. 
SLO's are set in such a way, to catch the an issue before it breaches the SLA. SLO have to be tight thresholds than SLA.   
* SLI or Service Level Indicator is a measurement the service provider uses for the meeting the goal.

Therefore, it is in your best interest to catch an issue before it breaches your SLA so that your technical team have time to fix it. 
These thresholds are your SLOs, service level objectives. They should always be stronger than your SLAs because customers are usually 
impacted before the SLA is actually breached. Violating SLAs come at great cost. So to sum up, an SLA is a external promise 
that comes with consequences, often monetary. While an SLO is effectively an internal promise to meet customer expectations, and SLI 
is the metric you considered for your SLO. 

Lets consider and example if your service SLA is defined as response latency will not be more that 400ms.On analyzing the 
service we know that most of the response are served withing 200 ms , so we could set our slo to be at 250ms, and in-case 
of any issue, we could catch it earlier. So in this above case SLA is of 400ms , SLO is set at 250ms and SLI : Latency. 

[Measuring and Operating for Reliability]:/sre_blog/sre/2020/10/13/measuring-and-targeting-reliability.html
[How to choose a Good SLI]:/sre_blog/sre/2020/10/14/choose-good-sli.html
[Developing SLO's and SLI's for a new Service]:/sre_blog/sre/2020/10/14/developing-slo-sli.html
