---
layout: page
title: Artifacts
permalink: /artifacts/
---

This page links to the original artifact and each of its three enhancements.
Full write-ups explaining the reasoning behind each change are on the
[Enhancement Narratives](/narratives/) page.

- [Original Artifact](#original-artifact)
- [Enhancement One: Software Design and Engineering](#enhancement-one-software-design-and-engineering)
- [Enhancement Two: Algorithms and Data Structures](#enhancement-two-algorithms-and-data-structures)
- [Enhancement Three: Databases](#enhancement-three-databases)

---

## Original Artifact

**Inventory Management App**

This application was originally developed for a mobile architecture and programming course, CS-360 at SNHU. The entire app is written in Java. This one artifact was enhanced in three ways, following the guidelines for each: (1) Software Design and Engineering, (2) Algorithms and Data Structures, and (3) Databases.

- [View Original Code](https://github.com/kookibidooki/CS-499/tree/original)

---

## Enhancement One: Software Design and Engineering

<div class="file-list"><strong>Relevant files:</strong> LoginActivity.kt, CreateAccountActivity.kt, PreferenceKeys.kt, SubmitButtonEnabler.kt</div>

I selected this artifact because it is a self-contained piece of the app that still represents real and common software engineering problems: (1) a blocking database call on the UI thread, (2) an unclosed database connection, (3) duplicated validation logic on two nearly identical screens, and (4) hardcoded strings scattered across multiple files.

The classes were converted from Java to Kotlin, the database calls were moved onto a background coroutine with closed connections afterwards, the repeated TextWatcher logic was put into a reusable function, and the string literals were replaced with a PreferenceKeys object.

- [View Enhanced Code](https://github.com/kookibidooki/CS-499/tree/enhancement-one-software-design)

---

## Enhancement Two: Algorithms and Data Structures

<div class="file-list"><strong>Relevant files:</strong> Item.kt, InventoryAdapter.kt, InventoryActivity.kt, ItemDetailsActivity.kt, InventoryDiffCallback.kt</div>

I selected this artifact because it shows a real-world data structure and algorithm problem, as the original code had "refreshing the screen" and "reload everything from scratch" as the same operation. Every time the inventory screen resumed, it cleared its entire in-memory list and rebuilt it from the database, then told the RecyclerView that the whole dataset had changed, even if only one item had changed.

Again, the files were converted from Java to Kotlin. A new LinkedHashMap keyed by item ID replaced a plain ArrayList, giving constant-time lookup by ID rather than a linear scan. DiffUtil was also added to compute the difference between the old and new item lists, so only rows that actually changed get rebound. Lastly, a feature that was described in the original project but never fully implemented was added: a notification for when an item's quantity reaches zero.

- [View Enhanced Code](https://github.com/kookibidooki/CS-499/tree/enhancement-two-algorithms)

---

## Enhancement Three: Databases

<div class="file-list"><strong>Relevant files:</strong> UserDatabase.kt, InventoryDatabase.kt, PasswordHasher.kt</div>

I selected this artifact because it contains the most consequential issue found anywhere in the code review: UserDatabase stored every password as plain text. Alongside that, both database classes handled schema upgrades destructively, deleting and recreating their tables on any version change, which would wipe every user account and every inventory record whenever the app updated.

These files were also converted from Java to Kotlin. Salted and hashed password storage using PBKDF2 was added, along with a rewrite of the onUpgrade() function to migrate the schema (adding columns instead of deleting the table). The hardcoded column-index reads in getItemsList() were replaced with named-column lookups, and an inventory_transactions table with foreign keys to both existing tables was added as a properly functioning audit table to log any transactions that occur.

- [View Enhanced Code](https://github.com/kookibidooki/CS-499/tree/enhancement-three-databases)
