---
layout: post
title: What does a PM *do*?
date: '2013-07-03 09:42:00'
---

"So…what does a PM actually _ **do** _?", is a question I usually face, especially from my developer friends, when I tell them that I am a Program Manager. This blog entry explains my understanding of this profession.

I moved over from being a developer to being a Program Manager 7 years ago, and have been practicing this art ever since. Now, PM is an acronym for various different sounding, but similar roles - Project Manager, Product Manager, (Technical) Program Manager, etc. And these roles sometimes overlap with Product Planning or Product Marketing. Recently I even came across the term Production Management :-) Over the past years, my understanding of the PM role has gone through 4 stages of progression, with each stage building upon the one before it.

1. Execution
2. Post-release activities
3. Planning
4. Strategizing

![4stages](/assets/imgs/what-does-a-pm-do/4stages.png)

**Execution**

Early on in a PM's career, their focus is on execution. Now, we all know that the PM does not actually write code. In this stage the PM's sole karma in life is to _ **facilitate development** _. At its most basic this means keeping _ **track of the project** _. Additionally, this involves being the human interface between different development teams to keep _ **track of dependencies** _and make any project adjustments in response to shifting realities in dependent teams. I learnt this part by jumping off the deep end when I became a PM in the Networking team in the Windows Phone team (or Windows Mobile as it was known then). Now, this being the Windows Phone product, almost every other component/feature team was dependent on my team. So I had a lot of "upstream" dependencies. On the other hand, my team was dependent on the OEM's radio driver team, which was an external team. This was my "downstream" dependency. As we would onboard different radios and chip architectures, or write a new abstraction layer to enable new scenarios like making a voice call and connecting to the Internet at the same time (yes, it was a long time ago :-)), I would need to ensure that as development progressed, all of these moving parts were moving in sync with each other. The last thing I wanted was the UI talking directly to the radio driver because my team failed to deliver the right interfaces, or some new radio capability not being exposed through my layer and thus being unavailable to the rest of the product.

**Post Release Activities**

Once a set of payload has been deployed to production a PM needs to _ **start raising awareness** _ of the new capabilities with customers and _ **gather their feedback** _. As a PM in Windows Azure, I used to regularly give talks about upcoming features to customer communities and get their initial reactions. Windows Azure has active MSDN forums and StackOverflow forums that I would regularly monitor and contribute to. It also helped that we have a very strong product marketing team. They would set up one-on-one face time with various customers and it was incredibly helpful to interact directly with the architects and engineers who were building their service on top of Windows Azure. Being part of Windows Azure, I was fortunate to have a lot of the supporting infrastructure that made it possible for me to perform these post-release activities. In case your team/company does not have this infrastructure set up, a few simple things that you can start off with are -

- Set up an email distribution list of your most vocal and passionate customers. This will provide you with a channel to pro-actively reach out to them to get early feedback on a new feature. This will also be very effective in surfacing problems early on.
- Create a StackOverflow tag for your product and communicate this to all your customers. This provides a good forum for customers to ask technical questions and get help from you as well as the community.
- Start participating in appropriate sub-reddits and start your own product's sub-reddit.
- Write regular blog entries describing new featues, or use cases of existing features.
- Set up a twitter account and a twitter hashtag for your product. Communicate summary of your reddit, StackOverflow, and blog updates on twitter.

**Planning**

In theory planning for the next release is easy. Pick up the top few features from your prioritized product backlog that will fit in your current release and get to work on that.

In reality this is rarely the case. For starters, maintaining a _ **prioritized product backlog** _ that has cost estimates is easier said than done. In order to create and maintain a credible product backlog, PMs have to engage in what I'll call _ **"continuous planning"** _. This means that as soon as a new feature or scenario is identified, you get to work on it and get a high level understanding of what it entails. This usually means having a clear understanding of the following -

- Priority from the customers' point of view and from your point of view
- Value proposition
- Goals
- Scenarios
- High level design
- T-shirt size cost estimates

Once you start engaging in the continuous planning model, you will discover a curious thing, there are multiple features that are all pri-0 (or the most important). And obviously you'll not be able to fit in all of them in a single release. Selecting a subset of these to do is mostly art. Sometimes you'll have to choose a lower priority feature over a higher priority feature simply because it fits, or because the developers who have the expertise to deliver that lower priority feature have time to do it in this release. Make sure you communicate the release payload to your upstream and downstream dependent teams. Over communication is better than under communication. Put this up on a team wiki or a sharepoint site so it is readily available upon demand.

Once you are fairly certain that you are going to implement a certain feature, it is time to _ **build the functional specification** _. This does not mean write a 30 page document for single button's functionality. The medium can be anything that can be communicated easily in your team - post-it notes, team wiki, whiteboard, etc. Contents of a functional spec will differ for different teams and different products and is the topic of another blog. But the one common thing all functional specs have is that they communicate very clearly and in great detail what it is that the team should be building. If there is an ops team or a test team, this spec will let these teams know how to deploy or test the product.

**Strategizing**

This is by-far the most difficult part of the job. Think hard about where you want to take your product/feature 2 years or 5 years down the line. Write down in vivid detail how your customers and possibly their end users will be using the product/feature in the future. Put on your sci-fi author's hat when you do this. Make this as vivid as possible - if you feel like describing what your customers will be wearing when using your app on their wall computers, do it. And at all costs avoid bland vision statements that you see on plaques of big companies. Unless the vision statement can help you make decisions about what to do next and how to behave in certain situations, it is not of much use. However, ensure that this aligns with the overall company's vision. There is nothing wrong with having your own unique vision that is different from the company you work for, but in that case go start your own company. Get buy-off on this vision from the team, they are the ones who will make this a reality, so it is important that they believe in it as much as you do. Then create a high level roadmap to get to this vision. The litmus test of an actionable vision is whether most of the work you do in next release is aligned to this roadmap.

And at all times remember this PM mantra - _ **"no plan survives contact with reality"** _. So after all your strategizing and planning, you have to keep a close eye on execution and constantly keep course correcting based on the ground realities of the moment, which brings us full circle back to the Execution stage!

