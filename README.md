# Notes from Scaleconf NZ 2017

## Disclaimer
These notes were fleshed out from some chicken scribbles that I took during the conference.

If I have misrepresented anyone's talks, I am happy to amend the notes.

Pull requests are welcome

Spell Checked using aspell

## Intro
I had never heard of Scaleconf before I was offered a ticket by my employer. After reading the briefs I thought it sounded well worth the very reasonable cost of a ticket.

Scaleconf is an ops focused conference, the organisers felt there was a large number of dev conferences in NZ, but not enough catering to the ops.

Scaleconf was held at Te Papa which did a great job of catering the event.

Gregory was the MC, he did a great job of both informing and entertaining the attendees. Gregory is hard to describe, but if you imagine John Clarke and Kramer from Seinfeld having a love child you are half way there.


## Cloud-scale: Lessons from the field
### Daniel Larsen

#### Senior Premier Field Engineer

#### Microsoft NZ

Daniel asked why we are not seeing the sort of scale in NZ that is seen elsewhere other than some outliers.

Some reasons he thought this might be are:

We have an over reliance on databases and tend to use them for everything, even when not appropriate.

He suggested we should be taking as much out of the database as possible, ie. using blob storage instead of plonking files in databases.

We also need to focus on performance tuning, and measure things like the number of customers we can fit on a core.

This is especially valuable in the cloud with the pay for what you use terms.

His example was SQL server on azure.

There are steep price breaks on the number of transactions, if you are making database calls for the sake of making calls then you are wasting money.

We can use this philosophy on internal hardware too.

## Automating Migrations at Scale
### Sarah Dapul-Weberman

#### Software Engineer

#### Pinterest

Pinterest had an old in house developed web framework based on backbone and python.

This framework we creaking under the pressure of a large number of devs joining the team,

They made the decision to move to React with a Node back end.

There are talks on YouTube from her colleagues on the move from backbone to react.

Her talk was focused on the python -> node migration.

Sarah wrote a transpiler to extract all of the python code into individual elements, then convert these into node.

They used a tool named Shadowtraffic to run production traffic in real-time against their transpiled node code.

The customers would be served by the old infrastructure and the outputs of the two would be compared to test the functionality of the machine written code.

They would run these tests and fix the transpiler to repair any coding issues they found, then repeat the process.

Sarah told us its ok to make throw away tools, and these tools don't have to be perfect.

That is some serious brain power to write your own transpiler, but she seemed to think it wasn't much of a big deal.

## Databases and Containers
### Ronen Baram

#### MySQL Principal Consultant

#### Oracle

Ronen's talk was mainly about a new feature in mysql **group replication**

This is a multi-master synchronous write replication technology.

Ronan put his faith in the demo gods, and it paid off.

He ran through the process of setting up a 3 node cluster running a demo webapp that displayed node status and function.

Removing a node successfully off-lined its self from the cluster and then was able to be bought back online.

The cluster uses mysql proxy which they recommend to have one of running on each database client server.

At the moment the proxy is pretty dumb, but it sounds like they have plans to make it a little smarter in the future.

There were questions from the audience about weighting different database servers for cross data centre usage. At this stage it is not a thing.

From what Ronen said, someone else was supposed to do this talk and he had it dropped on him at the last minute, so he did an awesome job, especially with the live demo.


## Planning your automation from scratch, what not to bring from your Word document!
### Vanessa Love

#### Support Guru

#### Octopus Deploy

Vanessa bought a wealth of information from her time supporting customers using octopus deploy in ways it was never meant to be used.

There were a number of things she told us **NOT** to do as part of your automated deploy
* Do not backup
  * You will have to write logic into your deploy to clean up old backups and there is a risk of accidentally deleting everything
  * If you have to back out then an automated deploy is going to be the quickest way to back out rather than manually copying backed up files around the place
    * She mentioned that there can be issues with databases schema updates so recommended reading [Alex Yates Blogs](https://www.red-gate.com/blog/author/alex-yates)
* Do not do partial deployments
  * This relies on you knowing the current state of the server you are deploying to, which is not guaranteed
  * Create a single deployment artifact
  * Version the state of the software with the software install scripts so that you have a point in time snapshot of your software
  * Never have dependencies on older releases
  * She referenced a blog about the history of deployments over the years [Dylan Beattie Blog](http://www.dylanbeattie.net/2017/07/deployment-through-ages.html)
* Always deploy to a new uniquely versioned directory
  * This means you are not overwriting existing files that may be in use by the app at the time of deploy
  * You can run tools across the newly deployed directory without impacting anything
  * sometimes these tools can take a long time to run which may cause some odd behaviour for end users

## IAM at Scale and on Hybrid
### Jeyappragash JJ

#### Software Engineer

#### ex-Twitter

JJ explained that IAM is hard

There are a huge numbers of reasons for this
* Distributed system
* Complex systems
* Heterogeneous environments

He said that most security events are the result of bad configuration rather than a bug in code.

There is a trade off between usability and performance

If you have a central hub for all IAM functions then usability is much better, but there is a performance penalty calling back to the central hub

Individual config will give you better performance, but there is a larger management overhead.


He has been working with a number of large tech companies to come up with some frameworks for IAM

There are 4 components
* Identity
* Authentication
* Authorisation
* Enforcement

 He gave us a few links to the various projects

[spiffe](http://spiffe.io)

[padme.io - Only a form asking if you want to collaborate](http://padme.io)

[istio.io](http://istio.io)


## Reduce time to market, Increase quality to market
### Vaughan Westray

#### IT Performance Manager

#### Ministry of Social Development NZ

Vaughan gave us an overview of what they have been doing at MSD.

He told us that quality needs to be developed, and tech skills need constant updating.

Poor quality output causes unplanned work which in the end has a negative impact on overall performance.

Performance and security is everyone's problem

For everyone to be able to help with these, the data needs to be made visible to them.

MSD has done extensive work with Dynatrace to measure performance of their applications.

They have recently been using the application performance monitoring tools provided by Dynatrace to see the proportion of end users that were happy/frustrate/unhappy. This was based on page load times and whether the person waited for the page to load, or gave up and quit.

Vaughan said they had some branch visits and seen the pain people had to go through with unresponsive system, which really helped it hit home on how important their systems performance is.

Really nice to see a government department doing the right thing.

## 100% Observability
### Jason Yee

#### Technical Writer

#### DataDog

Jason told us that Devops can be summed up with the acronym CAMS
* Culture
* Automation
* Metrics
* Sharing

Jason talked about the difference tools we can use to analyse different part of the stack
* infrastructure
  * metrics
  * logs

* backend
  * metrics
  * logs
  * traces

* frontend
  * metrics 


Metrics allow us to see Trans and Patterns which allow us to see known unknowns

Logs allow us to find the unknown unknowns

Traces allow us to see where and how long things took

Jason went on to mention using RUM to develop synthetic tests to assist with anomaly detection

To get a true picture, we need to think about the app as a whole from top to bottom.

By looking at it from multiple perspectives we can gain great insights.

## Scaling for growth - a Kiwi startup's story
### Andrew Schofield

#### Co-Founder and CTO

#### Timely

Andrew gave us a history on the creation of timely, from the idea he and a couple of friends had right through to today.

Timely is a SaaS based scheduling service used mainly by the beauty and wellness industry.

Having public cloud available made it very easy to get started, they had very little initial capital.

Once they went public with the product, they spent the initial period implementing as many key features as they could, as it was easy to do this before the platform became heavily used.

One of the key points from this talk were you need to know about issues before your customers do.

Timely make heavy use of pager duty and new relic. 

## Lessons Learned from building a “serverless" API
### Pam Rucinque

#### Senior Software Developer

#### ThoughtWorks

Pam's team were given a 6 week lead time to implement an api that would allow people to use an app to find parking from various parking options in their local area.

They settled on using AWS's API gateway, and lambda.

This allowed them to focus on the code, and not have to worry about setting up infrastructure.

Their functions would call out to parking providers api's, and present the results on a map to the customers.

They chose to go "serverless" because of the low cost, and build in scalability and performance.

They used the [serverless](https://serverless.com/) framework to do local dev.

Their most valued lesson learned was that memory is key, if you provide your function more memory, then the cpu is increased also.

Pam did some price comparisons showing that if you double the memory, it was cheaper than your function taking twice as long.

They found by not printing debug info to stdout, they reduced their functions runtime by a second.

Timeouts need to be handled gracefully, as these will increase the length of your functions run.

Init times also need to be considered, and jvms can take time to start. Although you are not charged for the jvm start time it impacts the performance of your functions.

They would call their own API to *pre warm* their functions. Later they found you can phone AWS and ask them to warm some up. They did they when they were going to run add campaigns.

## Designing interfaces that don't suck when your customers scale!
### Rob Pearson

#### Software Engineer

#### Octopus Deploy

Rob used the example of the octopus deploy console for most of his talk.

He pointed out that the UI looks very clean with a few projects and environments, but when you scale to hundreds of projects with hundreds of environments there is a hell of a lot of scrolling going on, both vertical and the cardinal sin: horizontally.

We need to test for **too much data** and also for no data.

Dashboards are a way of either aggregating or dividing up data.

For Dashboards there are a number of techniques tat can be employed.
* Seek pattern
Google is a good example of this, you search for key words and results are presented
* Show pattern
Outlook was the example used for this case. All data is available and its up to the user to filter on what they want to see
* Sticky user
Remembering what the user looked at last time and showing them this


## Thinking at Scale: Lessons learnt running Bing.com and Cortana
### Kumar Srinivasamurthy

#### Principal Group Engineering Manager

#### Bing

Kumar was another speaker that reinforced the idea that we need metrics.

He gave a great quote:

**Shit happens, but the same shit cant happen twice**

Again the message that knowing before you customers do is critical

Another great quote:

**Auto-mitigation is the only type of mitigation. Mitigate first, debug later.**

## Less Serverless: using Google App Engine for Serverless web apps
### Ashley Schroder

#### Software Engineer

#### A2X

Ashley explained that the Google app engine functionally sits somewhere between ec2 instances and lambda.

A2X is a financial product used by AWS resellers.

Their experience with the Google app engine was great. When they started, the low volume of transactions meant they sat in the free layer. This meant they had little to no initial capital investment.

The product was initially written to scratch an itch for themselves, as they had done AWS account management and the data from AWS is awful.

They followed the best practices of doing all of the work in the web layer, and keeping no state in the web layer.

The scaling is all built in, however there is tight integration with google, so moving off the platform would require some work.

The irony of hosting an AWS tool on GCE was not lost on the attendees.


## The Art of Coding a Conversation: Designing Bots
### Vishesh Oberoi

#### Technical Evangelist

#### Microsoft NZ

Bots are being create left right and centre. Vishesh gave us an overview of his experiences with bots.

There are two parts to a request, and intent and and entity.

"Send message" being the intent, and the message body being the entity.

Microsoft has open-sourced its [bot framework](https://dev.botframework.com/) 

It uses the [luis natural language interpreter](https://www.luis.ai/home)

He did a quick example of trying to book a flight via Skype with a bot, there was an initial hiccup where it didn't understand, by we got there in the end.

Bot dialogues should be treated like app screens.

Bots have got a hard job trying to impersonate humans, as we are complex, random and illogical

## Summary
I thoroughly enjoyed the conference, I was very impressed with the calibre of the speakers and content considering the size of conference.

I will definitely be keen to attend next year, and will be recommending more people go next year.
