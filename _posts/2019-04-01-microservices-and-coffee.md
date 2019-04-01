---
title : to  understand microservices have a coffee at Starbucks
---

## microservices

microservices is one of the latest buzzwords that everybody hears from the other, the other claims to have done many times over but yet its hard for them to explain what it is. when i work with my customer's and try and explain it to them, it sounds very difficult for them to understand what it is and why it's required.  so i thought about it a lot on how best to make it relatable with something that we see everyday and try it explain it that way. actually the more i think about it, largely all computer technology patterns / solutions are in some way derived from solutions we already have in our daily lives with people. well isnt everything about a computer is to do what people did (at a larger scale including AI)?

coming back to understanding microservices, one of the best ways to look at it to have a coffee at starbucks. to understand this we also will have to understand how to order for coffee at a regular coffee shop.

### regular coffee shop

you go to the counter, look at the display and you tell the person behind the counter what you want, the person repeats your order jots it down on a piece of paper or puts in POS machine, tells you what your bill is, you pay, and she gives the order to the person who makes the coffee and you go and wait at the side as she moves to the next customer. you wait for your order to be ready and she too is checking in between if your order is ready as she repeats the process for the next customer. as your order is ready she will get the order and give it you and off you go with your coffee.

this is your traditional monolith (client / server, 3 tier architecture) where there is a thread (person) or a group of threads (persons) waiting for a request (you) or requests (bunch of you's) to place an order. as one of the threads take your order, they see another one (next in line) ready to place an order and has a question. that thread simply puts you on hold and answers that next in line on their question (thats context switching) and comes back to you to bill you and then put in a request to the another process (kitchen) to make process your request as the thread moves to next in line. you are waiting for the thread to respond back. the thread in between puts the next in line on hold (again context switching) to either answer the next's next in line or to check on your order. the thread does this multiple times before it receives the response for the order from the other process and responds back to you (obviously again switching context between you and the next in line).

in this process everytime you need to scale operations (add more people), every person needs to know the entire process (scale the entire application). in case of anybody messing the request, its difficult to know exactly what went wrong where as everything was in that person's memory (stateful). also every person needs to have all the skills (being the counter person, know billing, sometimes know how to make coffee and also serve). updating the billing process means you have to train everybody.

while the above explanation is oversimplified but that's how basically it works.

### starbucks

you go to the counter, look at the display and you tell the person behind the counter what you want, the person repeats your order jots it down on a paper cup with your NAME and passes it on to another person. this person looks at the cup, punches it in POS and tells your bill amount, you pay and the person gives the cup to the next person who makes the coffee and gives it to the next person who announces the name on the coffee and you come and off you go with your coffee. all the while the first, second, third person is already taking the order from the fourth third and second in line to you.

this is the microservices architecture. every thread (person) has a fixed and only one job. that thread does that one job and passes the job to the next thread (process / person) and all the information required by the next thread is contained in the message (paper cup) for the next thread to do it job without asking the previous thread (person). this continues till the last thread is ready with response (coffee) and gives back the response to you. throughout the process no person is mutlitasking handling more than one request (except may the first thread sometimes to answer the query but thats rare & the third thread which may be is the database / coffee preparer who has a wait time when the coffee is being brewed in the machine). 

In this model you can add multiple persons at every level to scale operations (horizontal scaling) without worrying too much about how they will talk with each other. anything goes wrong you know exactly where what went wrong by looking at the paper cup (request details). you can hire people with different backgrounds and they can do a single thing effectively (single skillset). updating the billing process means you have to train only the billing person.

### advantages of microservices

- a bounded responsibility to do one thing and that one thing only
- choice / diversity of platform / technology
- extremely scalable & cost efficient
- easy for change / update management
- better traceability

### pains of microservices

- complex to understand and implement
- easy to have sprawl (platform / technology) issues if no proper standards / governance
- prone to marginally higher latency per transaction which may be an issue for low latency requirements
- new skillset

### conclusion

the decision is upto you. do you like the coffee buying experience at your regular coffee shop or at starbucks ?
the coffee still may still taste better at your regular coffee shop :)

Enjoy your coffee!



