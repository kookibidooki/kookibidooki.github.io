---
layout: page
title: Artifacts
permalink: /artifacts/
---

# Artifacts

## Original Artifact

**Inventory Management App**

This application was originally developed for a mobile architecure and programming course, CS-360 at SNHU. The entire app is written in Java. This one artifact was enhanced in these three ways, following the guidelines for each: (1) Software Design and Engineering, (2) Algorithms and Data Structures, and (3) Databases.

- [View Original Code](https://github.com/kookibidooki/CS-499/tree/main/original)

## Enhancement One: Software Design and Engineering

The relevant files of enhancement in this section were: (1) LoginActivity.kt, (2) CreateAccountActivity.kt, (3) PreferenceKeys.kt, (4) SubmitButtonEnabler.kt. 
I selected this artifact because it is a self-contained piece of the app that still represents real and common software engineering problems: (1) a blocking database call on the UI thread, (2) an unclosed database connection, (3) duplicated validation logic on two nearly identical screens, and (4) hardcoded strings scattered across multiple files.
The classes were converted from Java to Kotlin, the database calls were moved onto a background coroutine with closed connections afterwards, the repeated TextWatcher logic was put into a reusable function, and the string literals were replaced with a PreferenceKeys object. 

- [View Enhanced Code](https://github.com/kookibidooki/CS-499/tree/enhancement-one-software-design)

## Enhancement Two: Algorithms and Data Structures

The relevant files of enhancement in this section were: (1) Item.kt, (2) InventoryAdapter.kt, (3) InventoryActivity.kt, (4) ItemDetailsActivity.kt, (5) InventoryDiffCallback.kt. 
I selected this artifact because it shows a real-world data structure and algorithm problem, as the original code had “refreshing the screen” and “reload everything from scratch” as the same operation. Meaning, every time the inventory screen resumed, it cleared its entire in-memory list and rebuilt it from the database, then told the RecyclerView that the whole dataset had changed, even if only ONE item had changed.
Again, the files were converted from Java to Kotlin. I added a new LinkedHashMap keyed by item ID instead of a plain ArrayList in order to have a more appropriate data structure, where now it does constant-time lookup by ID raather than a linear scan. DiffUtil was also added to compute the difference between the old and new item lists to then only rebind the rows that changed. Lastly, a feature that was described in the original project but never fully implemented was added: a notification for when an item's quantity reaches zero. 

- [View Enhanced Code](https://github.com/kookibidooki/CS-499/tree/enhancement-two-algorithms)

## Enhancement Three: Databases

The relevant files of enhancement in this section were: (1) UserDatabase.kt, (2) InventoryDatabase.kt, (3) PasswordHasher.kt.
I selected this artifact because it contains the most consequential issue found anywhere in the code review: UserDatabase stored every password as plain-text. Alongside that, both database classes handled schema upgrades destructively, deleting and recreating their tables on any version change, which would wipe every user account and every inventory record whenever the app updated.
These files were also converted from Java to Kotlin. Salted and hashed password storage using PBKDF2 was added, as well as a rewrite of the onUpgrade() function to migrate the schema (adding columns instead of deleting the table). The hardcoded column-index reads in getItemsList() was replaced with named-column lookups, and lastly, I added an inventory_transactions table with foreign keys to both the existing tables as a properly functioning audit table to log any transactions that occur.

- [View Enhanced Code](https://github.com/kookibidooki/CS-499/tree/enhancement-three-databases)