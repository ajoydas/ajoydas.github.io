---
title: "🚀 Breaking Down the Monolith: A Quick Guide to Application Migration Patterns"  
tags: [microservices, software architecture, application migration]
style: border 
color: secondary
description: 
---
<br/>
<img src="/assets/img/posts/monolith-pattern-part1-cover.png" alt="Breaking Down the Monolith" style="width: 100%; height: auto;"/>
<br/>

I recently finished reading “Monolith to Microservices” by Sam Newman, and I thoroughly enjoyed its practical approach to tackling monolithic decomposition. Newman delves into migration patterns using real-world examples, making the transition to microservices much more accessible and understandable.

Here’s a brief overview of the application migration patterns discussed in the book- 

**1. Strangler Fig 🌱**

This pattern gradually transitions the functionality of an application to new microservices, akin to how a strangler fig gradually overtakes a tree. For instance, to migrate an e-commerce site’s payment processing, you would create a new microservice and route payment requests through a proxy. Initially, the proxy would direct all traffic to the monolith, but as the new service stabilizes, it gradually starts handling a larger percentage of requests.

**2. UI Composition 🎨**

Break down the UI into smaller, independent widgets backed by separate microservices, enabling independent development and deployment. For example, in a social media app, the news feed, user profile, and messaging can each be separate components, managed by their own microservices, making the updates more independent.

**3. Branch by Abstraction 🌉**

This pattern lets you make significant changes without a full rewrite or long-lived branches by creating an abstraction layer. Say we’re refactoring a notification system: we’d create an interface for sending notifications, with the old system connected initially. Then, build a new microservice behind the scenes and switch over at the interface level once ready!

**4. Parallel Run 🏃‍♂️**

Run both old and new implementations side-by-side, comparing their results to verify correctness and performance before fully switching over. If we’re testing a new fraud detection system, for instance, we can run it in parallel with the old one and compare outputs to detect any discrepancies. This approach is resource-intensive but provides strong validation.

**5. Decorating Collaborator 🎁**

For cases where we can’t change the monolith directly, this pattern lets us intercept calls post-processing to trigger new microservices. One example of this could be adding a loyalty points system: after the monolith confirms an order, we can add a decorator to intercept this event and call a loyalty service to award points.

**6. Change Data Capture 🔄**

This pattern listens for data changes in the monolith’s database to trigger actions in microservices, ideal for asynchronous workflows. For example, automating invoicing could be done by listening for new orders in the database, then triggering an invoicing microservice to create and send invoices.

The book dives deep into each of these patterns, with valuable strategies for those tackling the transition to microservices. Highly recommended for anyone on the microservices journey!

Feel free to share your thoughts on the [LinkedIn post](https://www.linkedin.com/posts/ajoy-das_microservices-systemdesign-softwarearchitecture-activity-7261470923755593728-3MkO?utm_source=share&utm_medium=member_desktop&rcm=ACoAACFoy40BiX6eAmWacHlefJGB6wKBjkuteKs).
<br/>