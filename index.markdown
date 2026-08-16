---
layout: page
title: Home
permalink: /
---

# Professional Self-Assessment
## Kaylin Hooper

Since pivoting into Computer Science from Accounting in September of 2024, I can confidently say that this program has done much more than just teaching me languages and syntax. It has given me a structured way of thinking about problems that I now bring into everything I build and develop, and it has clarified exactly what I can offer an employer or team from day one.

Building this ePortfolio, and specifically going back through my code with a critical eye during the CS-499 Code Review, forced me to see my own work the way a future colleague or manager eventually will: not just "does it run," but "is it correct, secure, efficient, and something someone else could pick up and maintain?"

- [Program Reflection](#program-reflection)
- [Portfolio Overview](#portfolio-overview)

---

## Program Reflection

### Collaborating in a Team Environment
Completing the course CS-250 Software Development Lifecycle showed me that team-based development often struggled if the approach taken was very plan-driven and step-by-step, like the Waterfall Model. This is because the software development lifecycle (SDLC) is never often stable, structured, nor predictable.

The final project for this course, the SNHU Travel Project, put that lesson into practice directly. Our team moved away from a rigid, Waterfall-style structure and instead implemented the Agile Process by building a functioning and motivated Scrum-Agile team, made up of a Product Owner, a Scrum Master, and the Developers.

This became a genuine team-collaboration experience when, partway through development, stakeholder requests shifted the original goal -- a Top 10 Destinations List -- into a Top 10 Wellness Destinations Slideshow. From the developers perspective of the team, there was a major concern about wasted work. But the Scrum framework actually provided the structure that allowed developers to voice that concern and be heard, rather than just absorb the change silently. The Product Owner reorganized the product backlog around new user stories, communicated the shift clearly to the developers, and kept the team's existing progress from being thrown out where it didn't need to be.

Holding onto that agile mindset of staying committed and keeping communication open, even as the target moved, is what let the team adapt without the cost of losing morale or cohesion. That experience showed me that collaborating on a team isn't about avoiding disruption at all, but more about having a shared process that can absorb it and continue forward.

### Communicating with Stakeholders
The course CS-305 Software Security was not only about developing secure code and implementing security concepts, but also focused heavily on communicating those implementations properly through a "Practices for Secure Software Report." This report was written in a way that would break down complex technical problems into pieces that were easier to understand, explaining why certain decisions were made so that stakeholders with varying backgrounds -- not necessarily in tech -- could still understand and follow.

Structuring that report was, in itself, a communication exercise. It moved from client-facing framing and plain instructions before ever getting into the technical substance (algorithm and cipher choices, certificate generation, secure communications, secondary and functional testing) and then closed by tying those specific decisions back to industry standard best practices in a way that a non-technical reader could recognize as credible, even without understanding the underlying cryptography.

Writing that report taught me that communicating with stakeholders is about sequencing and framing it so a reader without your technical background can still follow why a decision was made and trust that it was the right one. It doesn't have to be just simplifying the technical work and watering it down completely. That's a skill I go back to anytime I have to explain a technical trade-off to someone who's going to judge the decision on its outcome and not on its implementation.

### Data Structures and Algorithms
The course CS-300 Data Structures and Algorithms: Analysis and Design had me build the exact same program (a course catalog that could load, search, and print course information with prerequisites) three separate times, with one using a vector, one using a hash table, and one using a binary search tree. Implementing the identical functionality three different ways made the trade-offs between data structures something I could actually see and understand in a concrete way rather than just theoretically. The vector was fast to load and append to, but its unsorted insertion meant that later sorting and mid-list insertions were costly. The hash table offered average constant-time O(1) lookups as long as collisions were minimized, but degraded toward O(N) in the worst case depending on how well the hash function distributed keys. The binary search tree kept data naturally sorted with an efficient O(log n) traversal, but only if the tree stayed reasonably balanced, as an unbalanced tree could degrade to the same O(N) worst case like the others.

The final project wasn't implementing all three, and instead required recommending one and justifying that choice with the actual runtime analysis. I ended up recommending the hash table, since the course data didn't need to stay in a particular order and wasn't updated frequently, so the priority was fast lookups over guaranteed ordering.

That exercise of weighing a data structure's real-world access pattern against its theoretical complexity instead of defaulting to whichever structure felt most familiar is the same instinct I had to rely on again in my CS-499 capstone, when I chose a LinkedHashMap and a diffing algorithm specifically because of how the inventory screen was actually being read and refreshed, not because it was the first solution that came to mind (and it definitely wasn't).

### Software Engineering and Databases
The course CS-340 Client/Server Development had me build a full-stack application for a client scenario: Grazioso Salvare, an animal-rescue training company, needed a way to identify rescue dogs matching specific training profiles across five partner shelters. The project paired a Python CRUD module built with PyMongo against a MongoDB database with an interactive Dash dashboard that displayed the results as a filterable data table, a geolocation map, and a pie chart, letting the client narrow results down to Water Rescue, Mountain Rescue, or Disaster Rescue candidates on demand.

Structuring the CRUD module around three clear attributes (the authenticated client connection, the database reference, and the collection reference) was my first real experience separating a data-access layer cleanly from the interface built on top of it.

The most instructive part of this project was debugging the dashboard's filtering logic, not the build itself. Buttons worked correctly the first time they were clicked, but broke on specific sequences of repeated clicks, because Dash was treating each button's input independently instead of tracking which filter was actually active. I had to resolve that with dash.callback_context. A second, related bug meant that fixing the pie chart's "Other" category threshold for the full dataset broke it for every filtered view, since the threshold logic didn't distinguish between a broad, unfiltered dataset and a narrow, filtered one. Solving that required introducing a dcc.Store component to track the active filter state across every callback.  
Both bugs taught me the same lesson from two different perspectives, though: in an application with multiple interactive pieces reacting to shared state, treating each piece of UI as independent is what breaks first. The database and the interface both need a single, consistent source of truth to stay correct as the application grows more interactive.

### Security
The course CS-405 Secure Coding had me build a complete secure software development policy for a fictional company, Green Pace. That meant defining ten C/C++ coding standards tied to real, cited vulnerability classes, and then, for each standard, completing a full risk assessment scoring severity, likelihood, remediation cost, and overall priority, so that fixing a vulnerability was justified by its actual risk rather than treated as equally urgent across the board.

The project also required thinking about security as something that has to be built into a team's existing workflow, as just adding it at the end would be inefficient and redundant. I had to explain where automated detection tools should be integrated into Green Pace's existing DevOps pipeline -- during Build, fully enforced by Verify and Test, and continuously active through Maintain and Monitor -- which is basically mapping out a DevSecOps process rather than a one-time code review. I also had to write policy covering all three states of data (in flight, at rest, and in use) and map each of the ten security principles back to the specific coding standards they justified, so every rule in the policy could be traced to a reason and not just asserted.

That habit of justifying a security decision by its actual risk and tracing it back to a clear principle is exactly what I relied on during my CS-499 code review. Specifically, when I found plaintext password storage in my own Inventory App and had to reason through how to fix it and what happens to the accounts that already existed under the old, insecure scheme.

---

## Portfolio Overview

The three artifacts that follow this self-assessment all come from a single project, an Android Inventory Management application. It was enhanced across three areas of computer science to demonstrate range within one coherent body of work, rather than three disconnected exercises.

The **software design and engineering** artifact shows the app's login and account creation flow, which was originally written in Java and had database calls that were executed directly on the UI thread. The enhancement converts this code to Kotlin, restructures it around modern / properly threaded practices, and eliminates a resource leak. This demonstrates my ability to modernize existing software using current, well-founded tools and not just leaving working-but-flawed code untouched.

The **algorithms and data structures** artifact shows the app's inventory display and update flow. The enhancement replaces an inefficient full-list-reload pattern with a data structure and diffing algorithm that is suited to how the screen is actually used, and also closes a gap between the app's original requirements and what was actually built. This demonstrates my ability to evaluate a design against its actual costs and trade-offs, not just its surface-level functionality.

The **databases** artifact shows the app's data persistence layer. The enhancement replaces plain-text password storage with properly salted and hashed credentials, replaces a destructive schema-upgrade path with one that preserves existing data, and adds a new audit table to track inventory changes over time. This demonstrates a security-conscious approach to data that anticipates malicious threats and ordinary operational risk, like a careless update that would wipe out real user data.

All three of these artifacts together are meant to show more than three isolated fixes: they show a consistent process -- read the existing code critically, identify what is actually wrong instead of what is easiest to change, and implement a fix that is proportionate to the problem -- applied across three distinct areas of computer science. That process is what I am hoping to demonstrate to anyone reviewing this work.
