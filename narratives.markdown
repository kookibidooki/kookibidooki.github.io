---
layout: page
title: Enhancement Narratives
permalink: /narratives/
---

This page documents the three enhancements made to my capstone artifact,
covering software design, algorithms and data structures, and databases.
Each narrative explains what was changed, why, and how it connects to my
course outcomes.

- [Narrative One: Software Design and Engineering](#narrative-one-software-design-and-engineering)
- [Narrative Two: Algorithms and Data Structures](#narrative-two-algorithms-and-data-structures)
- [Narrative Three: Databases](#narrative-three-databases)

---

## Narrative One: Software Design and Engineering

<div class="file-list"><strong>Relevant files:</strong> (1) LoginActivity.kt, (2) CreateAccountActivity.kt, (3) PreferenceKeys.kt, (4) SubmitButtonEnabler.kt</div>

### Artifact Description
The artifact for the Software Design and Engineering enhancement is the login and account creation sections of my Inventory App, an Android application I originally built for the CS-360 Mobile Architecture and Programming course at SNHU. It consists of LoginActivity, CreateAccountActivity, their supporting layouts, and the handling of user authentication against a local SQLite database, as well as persisting their login state between sessions.

### Justification for Inclusion
I selected this artifact because it is a self-contained piece of the app that still represents real and common software engineering problems:
(1) a blocking database call on the UI thread

<figure class="narrative-figure">
  <img src="/images/artifact1/a1_image1.png" alt="LoginActivity.java's blocking DB call. db.checkUser line sitting directly inside setOnClickListener.">
  <figcaption>LoginActivity.java's blocking DB call. db.checkUser line sitting directly inside setOnClickListener.</figcaption>
</figure>

(2) an unclosed database connection

<figure class="narrative-figure">
  <img src="/images/artifact1/a1_image2.png" alt="Same block as before. There is no db.close() anywhere.">
  <figcaption>Same block as before. There is no db.close() anywhere.</figcaption>
</figure>

(3) duplicated validation logic on two nearly identical screens

<figure class="narrative-figure">
  <img src="/images/artifact1/a1_image3.png" alt="TextWatcher block from LoginActivity.java. It is the exact same block in CreateAccountActivity.java.">
  <figcaption>TextWatcher block from LoginActivity.java. It is the exact same block in CreateAccountActivity.java.</figcaption>
</figure>

and (4) hardcoded strings scattered across multiple files.

<figure class="narrative-figure">
  <img src="/images/artifact1/a1_image4.png" alt="Some examples -- the raw &quot;login_state&quot;, &quot;is_logged_in&quot;, and &quot;username&quot; literals.">
  <figcaption>Some examples -- the raw "login_state", "is_logged_in", and "username" literals.</figcaption>
</figure>

It is small enough to walk through in a code review, but also important enough to show meaningful improvements instead of just cosmetic changes.
The enhanced version shows a few real skills. Converting the classes from Java to Kotlin and then using View Binding shows I can modernize existing code using current standard utilities rather than just leaving it as-is because it “still works.”

<figure class="narrative-figure">
  <img src="/images/artifact1/a1_image5.png" alt="The View Binding setup from LoginActivity.kt">
  <figcaption>The View Binding setup from LoginActivity.kt</figcaption>
</figure>

Moving the database calls onto a background coroutine and confirming that the connection is closed after shows an understanding of Android’s single-thread model and resource lifecycle, not just how to make a click listener that functions on a fast device.

<figure class="narrative-figure">
  <img src="/images/artifact1/a1_image6.png" alt="The coroutine and db.close() block from LoginActivity.kt's attemptLogin()">
  <figcaption>The coroutine and db.close() block from LoginActivity.kt's attemptLogin()</figcaption>
</figure>

Taking the repeated TextWatcher logic and putting it into a single reusable function shows an inclination towards DRY (Don’t Repeat Yourself) coding,

<figure class="narrative-figure">
  <img src="/images/artifact1/a1_image7.png" alt="enableButtonWhenFieldsFilled() from SubmitButtonEnabler.kt">
  <figcaption>enableButtonWhenFieldsFilled() from SubmitButtonEnabler.kt</figcaption>
</figure>

<figure class="narrative-figure">
  <img src="/images/artifact1/a1_image8.png" alt="Paired with the last image, the call of that function in LoginActivity.kt">
  <figcaption>Paired with the last image, the call of that function in LoginActivity.kt</figcaption>
</figure>

and replacing the string literals with a PreferenceKeys object shows diligence towards maintainability details.

<figure class="narrative-figure">
  <img src="/images/artifact1/a1_image9.png" alt="The full PreferenceKeys.kt">
  <figcaption>The full PreferenceKeys.kt</figcaption>
</figure>

<figure class="narrative-figure">
  <img src="/images/artifact1/a1_image12.png" alt="A usage of PreferenceKeys.kt in LoginActivity.kt">
  <figcaption>A usage of PreferenceKeys.kt in LoginActivity.kt</figcaption>
</figure>

  

### Course Outcomes
In Module One, I planned for this enhancement to support Course Outcome Four: “Demonstrate an ability to use well-founded and innovative techniques, skills, and tools in computing practices for the purpose of implementing computer solutions that deliver value and accomplish industry-specific goals.” I believe the completed enhancement was able to meet that outcome with the Kotlin conversion, coroutine-based threading, and View Binding, as they are all current and well-founded Android practices. Also, fixing the data leak and duplicated code directly improves the delivered value of the artifact.
There are not any major updates to my outcome-coverage plan at this point. If anything, doing the actual implementation work made me a bit more confident that Outcome Four is the right fit, as the enhancement ended up being less about an algorithmic change and more about applying the right tools and patterns for the job.

### Process Reflection
I expected the biggest challenge was going to be the Java-to-Kotlin conversion, since that is the most visible change and because I haven’t worked with Kotlin too much outside of small personal projects. In a way, it sort of was, even if the process was mostly mechanical. Making sure everything was wired in correctly led to a lot of headaches due to different versions including different things innately -- I spent too long trying to get EdgetoEdge to work properly.
Turns out, the harder part of the whole process was actually making sure the coroutine-based rewrite didn’t change the app’s behavior. Making sure the UI update (saving login state, navigating, or showing an error) still only happens after the background database check finishes, and still happens on the main thread where it’s safe to touch the views. I suppose this shows that “modernizing” code isn’t just about syntax. It is easy to accidentally introduce a race condition while trying to fix a different problem if you’re not careful about where a thread switch happens.

<figure class="narrative-figure">
  <img src="/images/artifact1/a1_image10.png" alt="The same coroutine shown before, but specifically the withContext(Dispatchers.Main) { } boundary">
  <figcaption>The same coroutine shown before, but specifically the withContext(Dispatchers.Main) { } boundary</figcaption>
</figure>

I also learned something from how small a fix the unclosed database connection was, code-wise. All I was missing was one line, db.close(), but that one line matters a LOT. It reinforced that some of the most important findings from a code review aren’t the ones that need the most rewriting, but instead are the ones that are easy to miss because the code still runs fine without them.

<figure class="narrative-figure">
  <img src="/images/artifact1/a1_image11.png" alt="The tiny, yet very important one-line that I missed before. db.close()">
  <figcaption>The tiny, yet very important one-line that I missed before. db.close()</figcaption>
</figure>

Lastly, the main overall challenge was making sure I didn’t overdo the enhancement, or get too ambitious. Once I was converting the code to Kotlin, it was a bit tempting to start restructuring towards a full Model-View-ViewModel (MVVM) architecture with a ViewModel and Repository layer, since I found out that is the “more correct” long-term architecture for interactive applications. I decided to keep this enhancement focused on the things found in the code review so that the artifact stayed within the plan I had already created.

---

## Narrative Two: Algorithms and Data Structures

<div class="file-list"><strong>Relevant files:</strong> (1) Item.kt, (2) InventoryAdapter.kt, (3) InventoryActivity.kt, (4) ItemDetailsActivity.kt, (5) InventoryDiffCallback.kt.</div>

### Artifact Description
The artifact for the Algorithm and Data Structures enhancement is the inventory display and update flow of my Inventory App. It consists of InventoryActivity, InventoryAdapter, and ItemDetailsActivity, which is responsible for loading the list of items, displaying them in a scrollable grid, and letting a user tap into an item to change its quantity.

### Justification for Inclusion
I selected this artifact because it shows a real-world data structure and algorithm problem, as the original code had “refreshing the screen” and “reload everything from scratch” as the same operation. Meaning, every time the inventory screen resumed, it cleared its entire in-memory list and rebuilt it from the database, then told the RecyclerView that the whole dataset had changed, even if only ONE item had changed.

<figure class="narrative-figure">
  <img src="/images/artifact2/a2_image1.png" alt="The original loadInventory() that clears and rebuilds the entire list on every refresh.">
  <figcaption>The original loadInventory() that clears and rebuilds the entire list on every refresh.</figcaption>
</figure>

It was an inefficient design that this course outcome had me identify and fix.
The enhanced version shows a few specific skills. Introducing a LinkedHashMap keyed by item ID instead of a plain ArrayList shows I can choose a data structure based on what the access pattern code needs: constant-time lookup by ID rather than a linear scan.

<figure class="narrative-figure">
  <img src="/images/artifact2/a2_image2.png" alt="The LinkedHashMap declaration and lookup from InventoryActivity.kt">
  <figcaption>The LinkedHashMap declaration and lookup from InventoryActivity.kt</figcaption>
</figure>

Using DiffUtil to compute the difference between the old and new item lists and only rebinding the rows that changed shows an understanding of the cost of a UI operation and how to avoid unnecessary work instead of just making the feature “work.”

<figure class="narrative-figure">
  <img src="/images/artifact2/a2_image3.png" alt="The NEW loadInventory() from InventoryActivity.kt">
  <figcaption>The NEW loadInventory() from InventoryActivity.kt</figcaption>
</figure>

 
I also used the code review to identify a feature that was described in the original app but never fully implemented: a notification for when an item’s quantity reaches zero. This was added in the one place in the codebase where it can be checked easily, right when the new quantity is already known, instead of scanning the whole inventory separately to find the items at zero.

<figure class="narrative-figure">
  <img src="/images/artifact2/a2_image4.png" alt="The zero-quantity check from ItemDetailsActivity.kt">
  <figcaption>The zero-quantity check from ItemDetailsActivity.kt</figcaption>
</figure>

### Course Outcomes
I planned for this outcome to support Course Outcome Three: “Design and evaluate computing solutions that solve a given problem using algorithmic principles and computer science practices and standards appropriate to its solution, while managing the trade-offs involved in design choices.” I believe the completed enhancement was able to meet that outcome with the LinkedHashMap and DiffUtil changes as a direct example of managing a trade-off: a small amount of added bookkeeping -- maintaining the map and running a diff calculation -- in exchange for less UI work on every screen refresh.
There are not any major updates to my outcome coverage-plan. One note for a future pass, though, is that fixing the negative-quantity validation and the missing zero-quantity notification leaned a bit into defensive programming alongside algorithm design. I don’t think that changes which outcome this enhancement supports, but at least shows that artifact’s issues won’t often fit into just one category.

### Process Reflection
The part I underestimated going into this enhancement was how much the original InventoryAdapter and InventoryActivity were built around the assumption that the whole list would be just replaced every time. Once I decided to move into DiffUtil, I had to rework how the adapter owns and exposes its data. Instead of the activity handing it a mutable list it manipulates directly -- which is what the delete button was doing -- the adapter now owns an immutable list internally and only exposes a single entry point at submitList(). 

<figure class="narrative-figure">
  <img src="/images/artifact2/a2_image5.png" alt="InventoryAdapter.kt's submitList() function">
  <figcaption>InventoryAdapter.kt's submitList() function</figcaption>
</figure>

<figure class="narrative-figure">
  <img src="/images/artifact2/a2_image6.png" alt="Paired with the delete button's call right below the submitList(). The adapter is not being handed a mutated list directly anymore.">
  <figcaption>Paired with the delete button's call right below the submitList(). The adapter is not being handed a mutated list directly anymore.</figcaption>
</figure>

That was a much bigger structural change than I expected from what started as “swap ArrayList for LinkedHashMap.”
Working through the zero-quantity notification was a smaller challenge, but still very interesting. It probably would’ve been easy to just add it onto InventoryActivity’s refresh logic, since that is the class that sees the full current list of items. But tracing the actual flow of the app shows that ItemDetailsActivity was the only place that a quantity change originates, and it already has the new value in hand the moment the database write is successful. So checking here was not only simpler, but also way more immediate than waiting for the inventory screen to reload and THEN checking the whole list for zeroes.

<figure class="narrative-figure">
  <img src="/images/artifact2/a2_image7.png" alt="The whole zero-check, this time including the surrounding updateQuantity to show where it is in the file">
  <figcaption>The whole zero-check, this time including the surrounding updateQuantity to show where it is in the file</figcaption>
</figure>

The main challenge was making sure the DiffUtil comparison was actually correct. DiffUtil needs a way to tell whether two items are “the same item” by ID or whether the contents are identical. Getting the distinction right in InventoryDiffCallback took a bit of extra work by using ‘id’ for identity and the full data class equality for content, since Item is now a Kotlin data class with structural equality built-in.

<figure class="narrative-figure">
  <img src="/images/artifact2/a2_image8.png" alt="The two override methods in InventoryDiffCallback.kt">
  <figcaption>The two override methods in InventoryDiffCallback.kt</figcaption>
</figure>

---

## Narrative Three: Databases

<div class="file-list"><strong>Relevant files:</strong> (1) UserDatabase.kt, (2) InventoryDatabase.kt, (3) PasswordHasher.kt</div>

### Artifact Description
The artifact for the Databases enhancement is the data persistence layer of my Inventory App. It consists of UserDatabase and InventoryDatabase,

<figure class="narrative-figure">
  <img src="/images/artifact3/a3_image1.png" alt="The original onCreate() in UserDatabase.java that has the plain-text password column. Sigh.">
  <figcaption>The original onCreate() in UserDatabase.java that has the plain-text password column. Sigh.</figcaption>
</figure>

which are two SQliteOpenHelper subclasses that manage the app’s user accounts and inventory records. It also includes the new PasswordHasher, which turns the plain-text passwords into salted hash passwords before storing them in the database. 

### Justification for Inclusion
I selected this artifact because it contains the most consequential issue found anywhere in the code review: UserDatabase stored every password as plain-text. 

<figure class="narrative-figure">
  <img src="/images/artifact3/a3_image2.png" alt="The original checkUser() from UserDatabase.java">
  <figcaption>The original checkUser() from UserDatabase.java</figcaption>
</figure>

This isn’t a stylistic weakness, but actually a huge security vulnerability, and fixing it properly (instead of just patching around it) is a strong demonstration of a security-minded approach to database design.

<figure class="narrative-figure">
  <img src="/images/artifact3/a3_image3.png" alt="The new PasswordHasher.hash() from PasswordHasher.kt">
  <figcaption>The new PasswordHasher.hash() from PasswordHasher.kt</figcaption>
</figure>

<figure class="narrative-figure">
  <img src="/images/artifact3/a3_image4.png" alt="The updated checkUser() in UserDatabase.kt">
  <figcaption>The updated checkUser() in UserDatabase.kt</figcaption>
</figure>

Alongside that, both database classes handled schema upgrades destructively, deleting and recreating their tables on any version change, which would wipe every user account and every inventory record whenever the app updated.

<figure class="narrative-figure">
  <img src="/images/artifact3/a3_image5.png" alt="The original onUpgrade() that was destructive">
  <figcaption>The original onUpgrade() that was destructive</figcaption>
</figure>

The enhanced version shows a few specific skills. Adding salted, hashed password storage using PBKDF2 shows I understand why a password should never be stored or compared to in its original form, and how to implement that correctly rather than just sort of “hashing.” Rewriting onUpgrade() to migrate the schema (adding columns instead of deleting the table) shows an understanding that a database’s job is to preserve data across changes, not just to exist.

<figure class="narrative-figure">
  <img src="/images/artifact3/a3_image6.png" alt="The updated onUpgrade() from UserDatabase.kt">
  <figcaption>The updated onUpgrade() from UserDatabase.kt</figcaption>
</figure>

Replacing the hardcoded column-index reads in getItemsList()

<figure class="narrative-figure">
  <img src="/images/artifact3/a3_image7.png" alt="The original getItemsList() from InventoryDatabase.java">
  <figcaption>The original getItemsList() from InventoryDatabase.java</figcaption>
</figure>

with named-column lookups shows attention to a specific and easy to miss fragility: code that works today but would corrupt itself the moment someone reorders a column.

<figure class="narrative-figure">
  <img src="/images/artifact3/a3_image8.png" alt="The updated getItemsList() from InventoryDatabase.kt">
  <figcaption>The updated getItemsList() from InventoryDatabase.kt</figcaption>
</figure>

  
Lastly, adding the inventory_transactions table with foreign keys to both existing tables shows I can extend a relational schema in a way that’s properly related to the data it’s tracking.

<figure class="narrative-figure">
  <img src="/images/artifact3/a3_image9.png" alt="The new createTransactionsTable() from InventoryDatabase.kt">
  <figcaption>The new createTransactionsTable() from InventoryDatabase.kt</figcaption>
</figure>

### Course Outcomes
I planned for this enhancement to support Course Outcome Five: “Develop a security mindset that anticipates adversarial exploits in software architecture and designs to expose potential vulnerabilities, mitigate design flaws, and ensure privacy and enhanced security of data and resources.” I believe the completed enhancement was able to meet that outcome with the password hashing change, which is a textbook example of anticipating what would happen if a database file is exposed, and the non-destructive migration change, which addresses a design flaw that would damage user trust and data integrity on every update. 
I do not have any major updates to my outcome-coverage plan. I will note that the column-index fix leans a little towards general code robustness instead of just security specifically, but it is still fair to keep it bundled in this enhancement since it came out of the same close read of the database layer and reflects the same instinct to not trust implicit assumptions about the data.

### Process Reflection 
The most important thing I learned working on this enhancement is that fixing a security flaw isn’t just “add a hashing function.” I had to really think through what happens to a database that is already existing and being used: existing accounts in version 1 have a plain-text password and no salt or hash at all. Since a hash can’t be reversed, there’s no way to automatically rehash an existing plain-text password without a code path that briefly has access to it, which only exists at login or account creation.
I ended up documenting that limitation directly in onUpgrade()

<figure class="narrative-figure">
  <img src="/images/artifact3/a3_image10.png" alt="The comment block inside onUpgrade in UserDatabase.kt">
  <figcaption>The comment block inside onUpgrade in UserDatabase.kt</figcaption>
</figure>

instead of pretending that the migration would fully solve the problem on its own.  A complete rollout would need a forced password reset for accounts created under the old schema. That was a good reminder that a security fix isn’t always finished when the new code is “correct,” but is actually finished when what happens to the data that existed beforehand is accounted for.
I also learned something from the audit table -- it was tempting to make it a bigger feature immediately, like wiring the ItemDetailsActivity to call logTransaction() the moment I added it. I decided to instead keep this milestone focused on the database layer itself by creating the table correctly with the right relationships and creating a method that other screens can call.

<figure class="narrative-figure">
  <img src="/images/artifact3/a3_image11.png" alt="The comment above logTransactions() in InventoryDatabase.kt">
  <figcaption>The comment above logTransactions() in InventoryDatabase.kt</figcaption>
</figure>

Actually wiring a screen to use it is a future plan so that I only worked on this milestone’s artifacts, not a previous one. 
