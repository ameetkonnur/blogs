---
title: to sql or to nosql
---

## what

sql vs nosql. which is better ? but first a nosql joke.

`three database admins walk into a nosql bar. as soon as they get in they come out because they couldnt find a TABLE`

jokes aside, this is debate that i hear very often at my customer's (sometimes in my head too). in most large organizations (read non startups), sql / rdbms has ruled the database world for decades. these organizations have run their entire businesses on traditional relational database systems for ages. it would be fair to comment that pretty much the entire world's IT systems run on rdbms. but in the last few years (5-7 years), we have the popularity of nosql rise especially with google's, netflix's of the world. they have proved that nosql database's can also run mission critical real-time applications.

but before we get into the details of sql vs nosql, lets first understand what nosql is.
popular (mostly ignorance & hearsay) belief is nosql stands for NO SQL (while it originated that way). well actually it stands for Not Only SQL. that definition itself should lay rest some of the sql vs nosql debate, however there is a lot more to understand why nosql became popular and how should you think about it.

i liked this definition from a blog post on <www.techtarget.com>

`NoSQL is an approach to database design that can accomodate a wide variety of data models, including key-value, document, columnar and graph formats. NoSQL, which stand for "not only SQL," is an alternative to traditional relational databases in which data is placed in tables and data schema is carefully designed before the database is built. NoSQL databases are especially useful for working with large sets of distributed data.`

[history of nosql](https://en.wikipedia.org/wiki/NoSQL)

## why

### first question, was there a need for nosql ?

rdbms technologies have run pretty much every IT system in the world for decades. they have matured over the years and built a strong community (dba's) and a market surrounding them. however IT systems built in those days were built to address a specific need and there always was a certainity of what to expect in terms of what i will store, how much i will store and databases were designed keeping those parameters in mind. the rdbms solutions improved every few years anticipating the need for more data to be stored and more users to cater to. also there were surround technologies built around rdbms which helped these organizations with reporting, management, developer experience, syncing data between different rdbms platforms, syncing data between different locations etc.

there was always a need for better, faster and cheaper technology as the amount of data generated increased year on year. rdbms platforms struggled to keep up but still somehow managed to keep their heads above the water. with internet penetration, gaming, mobile devices, ecommerce, social platforms growing the amount of data generated increased exponentially and that put a huge strain on the traditional rdbms technology. the companies behind this huge data growth quickly realised it was unsustainable to use traditional rdbms to support their business and they got about putting their heads to solve this problem.

so what was the problem with managing growing data with traditional rdbms technology ? actually quite a few, but the important ones below

<<<<<<< HEAD
1. rdbms technology was built on a fundamental principle of scale-up which means if we have to store more data, attach more disks to the server, if we have to cater to more users, increase the processor & ram of the server. while this worked well for predictable and non-volumenous growth it wasnt a good model for the exponential growth that was being seen at these new age companies. for most very large traditional companies a few TB's a year growth was the maximum they would see, but for the new age companies they were adding few TB's a month or sometimes every week (this also is called big data now). adding disks and adding processors or shifting to larger server's every few months wasnt a scalable and economically sustainable model.
=======
1. rdbms technology was built on a fundamental principle of scale-up which means if we have to store more data, attach more disks to the server, if we have to cater to more users, increase the processor & ram of the server. while this worked well for predictable and non-volumenous growth it wasnt a good model for the exponential growth that was being seen at these new age companies. for most very large traditional companies a few TB's a year growth was the maximum they would see, but for the new age companies they were adding few TB's a month or sometimes every week (this also is called big data now). adding disks and adding processors or shifting to larger server's every few months wasnt a scalable option. also one had to factor is hardware failures (both availability and recovery) storage corruption etc. there was some answer to this in the form of appliances that promised linear performance increase as they add more compute to the same server / cluster, better availability & somewhat an quicker way to scale.  but this came at a huge cost, lockin in proprietary hardware & not the agility that they were looking at. 
>>>>>>> 912a40cfc649b545ca871718ac1f498b6c41ba46

2. rdbms is great when you know what you are going to store. when what you will store changes very often (every week / day), rdbms isnt great as changing table structures, or creating generic tables /columns isnt easy and may sometimes require downtime or moving data between tables.

3. as the concurrency on these applications increased, the database technology too faced increased concurrency pressures. this concurrency wasnt just a factor of front end applications having increased loads but also backend workloads like operational reporting, stream analytics, event driven architectures, replication. rdbms technologies could scale to handle quite a large number of conurrency but these new workloads demanded concurrency scales which was atleast 5 to 10 times the usual peaks that rdbms were known to handle.

all this led to an architecture evolution where you can scale out (instead of scale up) both compute & storage addressing increasing data volumes, increasing data velocity(concurrency), store anything (variety). this meant few big changes, 
- database technology had to understand new server's and new storage being added dynamically to the same cluster / server
- distribute / re-distribute data across multiple such server's and have ability to write & read data across all of them without too much complexity / changes
- have a data storage structure with metadata that allowed to store anything without changing the schema of table / database
- have a model to handle very high connection concurrency (in Millions / second)

### second question, cant rdbms scale out ?

yes rdbms can scale out too but there are a few problems in the approach.

1. native rdbms scale out solutions are very expensive to procure in terms of software licenses & certified hardware (server / storage)
2. non-native rdbms scale out patterns are hard to implement, harder to maintain. also they may require massive code changes on front end (mostly all nosql implementations also require code changes)
3. they still dont effectively address the variety problem (no fixed schema)
4. they also not as agile and scalable as native nosql for adding scale as and when needed

## how