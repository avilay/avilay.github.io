---
layout: post
title: What is Good Software Design
date: '2012-09-01 07:43:00'
tags:
- from-wordpress
---

"_But that is not good design.._" is a refrain I have heard in countless software design meetings arguments. And usually these arguments devolve pretty quickly into philosophic disagreements based on opinions rather than facts. This obviously begs the question - so **what is good software design?** I'll add my own opinion to this ongoing discourse with 7 design principles of good software design.

**1. Functional**  
A few months ago I was at a Lean Startup Meetup where one of the panelists was the VP of technology of a well known company that had just acquired a smaller company. Software architects working for her were commenting that the design of the acquired software was terrible, when one of them had the sudden "insight" - "but we just paid millions of dollars for this, because the shi\*t works!" And that to me is the first and foremost principles of good design. Any piece of software must be able to execute its core functionality well. Otherwise it is all for naught. _ **Good software is functional.** _

**2. Robust**  
I was showing off my mad coding skillz in asp.net/mvc to a friend of my mine who happens to be a software architect. He gave me a kind fatherly look and said "this is all fun and good, but can you write code that will live on for years and years? This phone that you are using has a piece of code that I wrote 5 years ago". Which brought me to my second epiphany about good software design. Its got to be robust. But what does robust really mean? To me it means three things - software is resistant to failures, it is able to recognize when failures occur, and it is able to recover from failures. _ **Good software is robust.** _

**3. Measurable**  
My third design principle was not a sudden a-ha moment for me. But rather it grew on me gradually, as a result of working in environments that were extremely data driven, sometimes ruthlessly so. It should be possible to measure how the software is doing out in the wild, outside of the confines of my test fixtures. Of course the actual measurement metrics would differ based on the business and nature of the software. An oft used metric for web applications is the number of HTTP 500 ISEs that are returned. For UI based applications, it is usually the number of seconds for the UI to respond. But if you find yourself writing software without putting any thought as to how to measure it (whatever measure means for you), know that you are violating a good design principle. _ **Good software is measurable.** _

**4. Debuggable**  
I remember the days working as a developer in an e-commerce company (yes, that one), where I would get paged because the ordering system was misbehaving. I would be the only developer debugging the live system, all the time surrounded by a bunch of manager-types who would demand "status" every second minute. Walking through the code in that situation was - to put it mildly - not fun. And it brought home to me the realization that any software I write should have logging that is not just there for the heck of it, but that would really help me debug when the need arose. Since then I have put in debug APIs on my web services, put in ability to dump debug state upon demand, etc. _ **Good software is debuggable.** _

**5. Maintainable**  
One of the ways in which recruiters try to attract software developers is by telling them that in the new company, they will be writing brand new software from scratch. Which highlights the fact that a lot of work that software developers do is maintaining software that somebody else wrote a while back. Software can be easy to maintain if has consistent styling, good comments, is modular, etc. In fact, there is a lot of literature on good software design that just focuses on design principles that make it easy to make changes to parts of the software without breaking its functionality. _ **Good software is maintainable.** _

**6. Reusable**  
Software developers worry about writing reusable software incessantly. A lot of software developer interviews that I have sat in (as an observer, interviewer, interviewee) invariably pose an algorithm question - "how can you find the least common ansector in a tree", then the follow up is - "how can you make it more general". But note, that this is #6 on my list. That is because generilizing a specific solution is hard and time consuming. And a lot of times nobody ends up reusing that piece of code anyway (of course you can argue that it is because the code was not reusable enough to begin with…). So unless you are absolutely sure that you are going to reuse this piece of functionality elsewhere, and you can time bound the effort of making it reusable, do not try this at home. _ **Nevertheless,**  **good software is reusable** _.

**7. Extensible**  
Surprisingly, the topic of maximum number of design arguments I have sat through makes it last in my list. Usually, this is brought up when you are building infrastructure software. And it usually starts with - "but suppose that tomorrow somebody wants to add X here…". Be wary of this design principle. Be very wary. Unless "tomorrow" is a real date in the future, and "somebody" is a real person, this design principle can lead you down a deep rabbit hole with nothing but dirt at the bottom. But I have to say this - _ **really good software is extensible.** _

