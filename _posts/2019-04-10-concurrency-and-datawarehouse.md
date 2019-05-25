---
title : concurrency & datawarehouse (Azure SQL Datawarehouse)
---


for data guys who have had any experience with building datawarehouses (especially in the cloud), would have this awkward conversations with stakeholders on concurrency in datawarehouses. up untill a few years back datawarehouses (sizes usually in multiple TB's & higher) employed MPP (Massively Parallel Processing) architecture. this usually meant distributing data to multiple computers (nodes) and doing processing on this multiple nodes (getting compute closer to data). this involved expensive hardware (datawarehouse appliances) and multi-year projects.

with the popularity of cloud and big data platforms this (not the architecture. it still remains MPP with a few changes) has changed. everybody now has access to similar or better technology  at a far lower cost and way quicker GTM (in few months) with the datawarehouse. while their pitch comes with the promise of massive scale, huge storage (TB's & PB's), lightning fast response times and extreme agility, one small disclaimer sneaks in which is that of a far lower concurrency (concurrency can be thought of as the maximum number of queries in active execution) than the older on-premises appliance based solution.

the big question ### why the lower concurrency ?

to understand this we will have to dig a little deeper into the concept of concurrency and how concurrency is handled in modern day computers (servers).

concurrency to put it simply is the ability for a computer to serve multiple requests at the same time (time slice here being a second & concurrency expressed in terms of requests per second or queries per second or users per second). the heart of what allows a computer to do work (do a calculation, show an image etc) is a unit called the processor (equivalent of a brain in humans). these processors are further split into a smaller units called cores (there are multiple of them for each processor. loosely equate it to our brain's left hemisphere and right hemisphere) each of which can do one job at a time. what allows them to do that job is a construct called as a thread (may be thoughts in our brains. dont know how to simplify this further). usually a processor has multiple threads at any given time but at a time the core can entertain only a single thread (like we can think about or do one thought process at a given time). loosely put a computer with one processor and two cores can actually entertain two threads at any given time (do two jobs at any given time and thus have a concurrency of two). 
at this point somebody would argue that's quite ineffecient and even a human brain can do more things at a time than a computer (a person who can use his left side and right side of the brain can actually do two different things with their left and right limbs. ambidextrous person are known to do this). non ambidextrous people also do multiple things at the same (multi-tasking). computers are known to do 100's of things at the same time even on a single processor & a single core. how is this possible ?
all this is possible because concurrency is largely an illusion. computers usually are fast enough to switch in & out of multiple jobs between fractions of seconds that it appears to us that they are doing all those jobs at the same time (our slice of time is second where for really fast computers it's in milli-seconds or micro-seconds). this concept in the computer world is called context switching.

lost you ?

let me give a real world example that will hopefully clear this concept.

most of us have been driving cars and we have been sold one which touts ABS as a great safety feature (its is actually). ABS stands for Anti Lock Braking System. if you knew cars that didnt have ABS, you would usually have a problem where you cannot steer the car (to avoid collision) at the same time when you are braking. this is because you are trying to make the wheels turn while making them stop too. the wheels can do only one thing at a time and usually the brakes used to win this duel and lock the steering (that means the car wouldnt steer in the directon you want it to go). this was solved by ABS. ABS allowed dirvers to steer the vehicle at the same time when they were braking. the way this was done was by alternating between steering and braking very rapidly (usually multiple times a second) and make it feel as though both were being done at the same time. but this wasnt done by the driver. this is done by the car as the driver presses on the brake and steering the car away from a collision.

p.s. i tried this concept at home trying to listen to instructions from my wife while i was typing this blog, let me tell you didnt end well for me. i am not built for concurrency.

another example is that of how motion is percieved while watching video's. its actually not a video but still images changing at a very fast pace (frames per second) to give us an illusion of motion. hence the word motion picture :).

now that you understand concurrency, you would also understand the implications of it.

given a set of computer resources (processors and cores) designing for supporting higher concurrency may also mean higher the time per request it would take. this is because if the one core has to do 1000 jobs at a given time, it will keep switching between these jobs to do part of it and thus impacting the time per job. also one needs to understand that every context switch in itself has an overhead (usually very small but at higher concurrency can be significant if the quantum of job is higher). This is also evident when we try to multitask. jobs that require higher focus (quantum of job is higher) takes more effort to switch between. on the contrary jobs that are small (quantum of effort is small) have a lower overhead on context switching and sometimes may not require context switching as they take a very small fraction of time to complete (example on how we can think while doing mundane jobs like driving on the same road everyday largely through muscle memory).

typically oltp requests are very small (few bytes). for context switching (which would involve persisting this state to some store to move to another job and retreiving it back to execute the previous one) the overhead is very small or negligible. in datawarehouse world a request could be of sizes 100x - 10000x of a typical oltp request. context switching between these requests is extremely expensive and would hamper the overall performance of the system. effectively none of the requests would be served on time.

another real-world example of this is the difference between a cataract operation and a open heart surgery. its common (in developing countries) for eye doctors to conduct multiple cataract operations at once (procedure is fairly standard, low risk & quick) and allows for context switching. but an open heart surgery requires dedicated focus for hours as the risk is high, highly complex and time consuming. for a heart doctor to do multiple open hearts at the same time wouldnt be possible as the cost of context switching and the risk thereby (of no or adverse outcome) is very high.

given the above why do datawarehouses suffer from poor concurrency compared to other database systems (usually OLTP / ODS kinds) inspite of having higher magnitude's of computer resources. this is because of two differentiators

- datawarehouse workloads are inherently complex (queries span billions of data records, complex computation, spread across multiple nodes) which essentially means a higher quantum of work.
- they are designed not to do context switching too often

OLTP workloads are designed to handle concurreny by making the unit / quantum of work very small so that they can entertain larger no of requests by continously doing context switching between extremely small jobs.

Datawarehouse workloads usually act on large amounts of data with complex calculations (large unit / quantum of work), hence they are designed not to do context switching. if they were to emulate the OLTP architecture to do context switching to support more concurrency the overhead of context switching itself will be large enough to more than offset any gain in concurreny. Datawarehouse systems are hence designed to do a complex job as quickly as possible and then do the next. hence they also are architected to be lower on concurrency.

while there are solutions / patterns to support higher concurrency in datawarehouse systems by introducing intermediate layers like OLAP, scaling out to multiple units of datawarehouse, but at the core datawarehouses will retain the architecture choice of supporting lower concurrency for now.

hope this gave you some perspective on concurrency and datawarehouse.

to conclude, its not that datawarehouse technologies (like Azure SQL DW) cannot support higher concurrency, they chose not to in order to prioritize query performance. over time they may build new capabilities to support both higher concurrency and similar query performance but the architecture, technology may change. stay tuned to see if Microsoft will announce something this build <https://www.microsoft.com/en-us/build> on this. 

to read more about this you can check <https://docs.microsoft.com/en-us/azure/sql-data-warehouse/memory-and-concurrency-limits#concurrency-maximums> <https://docs.microsoft.com/en-us/azure/sql-data-warehouse/resource-classes-for-workload-management>

enjoy!

disclaimer : i am neither an expert on datawarehouse technologies nor on processor compute models. this is just an obeservation from my experience over the years looking at why certain things may work the way they do. i am happy to be corrected if i am way out of line in my understanding. thank you.