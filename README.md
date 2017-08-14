# Notes from Scaleconf 2017

## Disclaimer
These notes were fleshed out from some chicken scribbles that I took during the conference.
If I have misrepresented anyones talks, I am happy to ammend the notes.
Pull requests are welcome

## Intro
I had never heard of scaleconf before I was offered a ticket by my employer. After reading the breifs I thought it sounded well worth the very reasonable cost of a ticket.
Scaleconf was held at Tepapa which did a great job of catering the event.
Gregory was the MC, he did a great job of both informing and entertaining the attendees. Gregory is hard to describe, but if you think about John Clarke and Kramer from Seinfeld having a love child you are half way there.

## Cloud-scale: Lessons from the field
### Daniel Larsen

#### Senior Premier Field Engineer

#### Microsoft NZ

Daniel asked why we are not seeing the sort of scale in NZ that is seen elsewhere other than some outlyers.
Some reasons he thought this might be are:
We have an over reliance on databses and tend to use tend to use them for everything, even when not appropriate.
He suggested we should be taking as much out of the database as possible, ie using blob storage instead of plonking files in databases.
We also need to focus on performance tuning, and measure things like the number of customers we can fit on a core.
This is especially valuable in the cloud with the pay for what you use terms.
His example was SQL server on azure.
There are steep price breaks on the number of transactions, if you are making database calls for the sake of making calls then you are wasting money.
We can use this phylosophy on internal hardware too.

## Automating Migrations at Scale
### Sarah Dapul-Weberman

### Software Engineer

### Pinterest

Pinterest had an old in house developed web framework based on backbone and python.
This framework we creeking under the pressure of a large number of devs joinign the team,
They made the desicion to move to React with a Node back end.
There are talks on youtube from her colleagues on the move from backbone to react.
Her talk was focused on the python -> node migration.
Sarah wrote a transpiler to extract all of the python code into individual elements, then convert these into node.
They used a tool named Shadowtraffic to run production traffic in realtime against their transpiled node code.
The customers would be served by the old infrastructure and the outputs of the two would be compared to test the functionailty of the machine written code.
They would run these tests and fix the transpiler to repair any coding issues they found, then repeat the process.
Sarah told us its ok to make throw away tools, and these tools dont have to be perfect.
That is some serious brain power to write your own transpiler, but she seemed to think it wasnt much of a big deal.

## Databases and Containers
### Ronen Baram

#### MySQL Principal Consultant

#### Oracle

Ronen's talk was mainly about a new feature in mysql **group replication**
This is a multimaster synchronous write replication technology.
Ronan put his faith in the demo gods, and it paid off.
He ran through the process of setting up a 3 node cluster running a demo webapp that displayed node status and function.
Removing a node successfully offlined itsself from the cluster and then was able to be bought back online.
The cluster uses mysql proxy which they recommend to have one of running on each database client server.
At the moment the proxy is pretty dumb, but it sounds like they have plans to make it a little smarter in the future.
There were questions from the audience about weighting different database servers for cross datacentre usage. At this stage it is not a thing.
From what Ronen said, someone else was supposed to do this talk and he had it dropped on him at the last minute, so he did an awesome job, especially with the live demo.

## Planning your automation from scratch, what not to bring from your Word document!
### Vanessa Love

#### Support Guru

#### Octopus Deploy

Vanessa bought a wealth of information from her time supporting customers using octopus deploy in ways it was never meant to be used.
There were a number of things she told us **NOT** to do as part of your automated deploy
* Do not backup
  * You will have to write logic into your deploy to clean up old backups and there is a risk of accidentally deleting everying
  * If you have to back out then an automated deploy is going to be the quickest way to back out rather than manually copying backed up files around the place
    * She mentioned that there can be issues with databases schema updates so recommended reading [Alex Yates Blogs](https://www.red-gate.com/blog/author/alex-yates)
* Do not do partial deployments
  * This relies on you knowing the current state of the server you are deploying to, which is not guaranteed
  * Create a single deployment artifact
  * Version the state of the software with the software install scripts so that you have a point in time snapshot of your software
  * Never have dependencies on older releases
  * She refernced a blog about the history of deployments over the years [Dylan Beattie Blog](http://www.dylanbeattie.net/2017/07/deployment-through-ages.html)
* Always deploy to a new uniquley versioned directory
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
* Heterogenous environments

He said that most security events are the result of bad configuration rather than a bug in code.
There is a trade off between usability and performance
If you have a central hub for all IAM functions then usability is much better, but there is a performance pentalty calling back to the central hub
Individual config will give you better performance, but there is a larger management overhead.

He has been working with a number of large tech companies to come up with some frameworks for IAM
There are 4 components
* Identity
* Authentication
* Authorisation
* Enforcement

 He gave us a few links to the various projects
[spiffe](spiffe.io)
[padme.io - Only a form asking if you want to collaborate](padme.io)
[istio.io](istio.io)


## Reduce time to market, Increase quality to market
### Vaughan Westray

#### IT Performance Manager

#### Ministry of Social Development NZ

Vaughan gave us an overview of what they have been doing at MSD.
He told us that quality needs to be developed, and tech skills need constant updating.
Poor quality output causes unplanned work with in the end has a negative impact on overall performance.
Performance and securtiy is everyones problem
For everyone to be able to help with these, the data needs to be made visible to them.
MSD has done extensive work with Dynatrace to measure performance of thier applications.
They have recently been using the application performance monitoring tools provided by Dynatrace to see the proportion of end users that were happy/frustrate/unhappy. This was based on pahe load times and whether the person waited for the page to load, or gave up and quit.
Vaughan said they had some some branch vists and seen the pain people had to go through with unresponsive system.
Really nice to see a government department doing the right thing.

## 100% Observability
### Jason Yee

#### Technical Writer

#### DataDog

Jason told us that Devops can be summed up with the acronym CAMS
* Culture
* Automation
* Martics
* Sharing

Jason talked about the difference tools we can use to analyse different part ofs the stack
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
Traces alow us to see where and how long things took

Json went on to mention using RUM to develop synthetic tests to assist with anomoly detection
To get a true picture, we need to think about the app as a whole from top to bottom.
By looking at it from multiple perspectives we can gain great insights.

## Scaling for growth - a Kiwi startup's story
### Andrew Schofield

#### Co-Founder and CTO

#### Timely

## Lessons Learned from building a “serverless" API
### Pam Rucinque

#### Senior Software Developer

#### ThoughtWorks

## Designing interfaces that don't suck when your customers scale!
### Rob Pearson

#### Software Engineer

#### Octopus Deploy

## Thinking at Scale: Lessons learnt running Bing.com and Cortana
### Kumar Srinivasamurthy

#### Principal Group Engineering Manager

#### Bing

## Less Serverless: using Google App Engine for Serverless web apps
### Ashley Schroder

#### Software Engineer

#### A2X

## The Art of Coding a Conversation: Designing Bots
### Vishesh Oberoi

#### Technical Evangelist

#### Microsoft NZ
