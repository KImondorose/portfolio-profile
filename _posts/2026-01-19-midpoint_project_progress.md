---
layout: post
title: "Midpoint Project Progress"
date: 2026-01-19
categories: [Outreachy, OpenStack, Internship]
author: "Rose Kimondo" 
tags: ["outreachy"] 
---

## Week 6 Project Blog: Mid-Point Progress Report

### The Journey So Far

I can't believe that I am already halfway through my internship. Time is passing by so fast, but as usual, I am very grateful for what has been and what is to come.  

My internship journey with OpenStack Manila UI began with a focus on enhancing how users interact with metadata across various share resources. The original timeline was ambitious, aiming to bring consistency and deeper visibility to metadata management for Share Export Locations, Share Network Subnets, and Share Snapshots.  

Looking back at the first half of this internship, I am proud of the functional milestones we have crossed.

* * *

### Accomplishments and Goals Met

The first half of my internship was dedicated to improving the metadata user experience. I successfully implemented metadata support for several key components:  

* Share Export Locations & Share Network Subnets: Streamlined how these technical details are surfaced to users and how they interact and modify them. Users can now view, add, and edit metadata directly from the detail view of these components.  

* Share Snapshots: Integrated a robust metadata management system, allowing users to view, add, and edit metadata directly from the Snapshots table using a clean, readable format.  

* UI Consistency: I focused on ensuring that metadata displays were scannable, implementing logic to truncate long strings and use line breaks and/or scroll boxes so the dashboard remains organized even with heavy data.  

* * *

### Lessons Learned and Adjustments

Some goals took longer than expected, particularly the integration of complex forms within Horizon's specific framework.

* Why it took longer: Navigating the specific interactions between Django, Horizon's SelfHandlingForm, and the server-side filtering required a deeper dive into the manila-ui architecture than initially anticipated.  

* What I would do differently: If I were starting over, I would spend more time tracing the data flow from the manilaclient through the API wrapper before building the UI components. Understanding the exact attribute names early on saves significant debugging time.  

* * *

### Goals for the Second Half

While the first half was about **Visibility**, the second half is about **Protection**. My plan shifts focus toward a critical security and administrative feature: **Resource Locks**.

#### The New Focus: Resource Locks

I am currently building a dedicated panel for Resource Locks under the Project dashboard. This feature is vital for preventing the accidental deletion of shares and access rules.

* Dynamic UI: I will develop a "switchable" form that changes fields based on whether a user is locking a Share or an Access Rule.

* Advanced Filtering: The new panel includes server-side filtering to allow administrators to quickly search by Resource Type, Action, or Resource ID.

* User Experience: I am implementing "scroll box" selections for large resource lists to keep the modal interfaces compact and user-friendly.

* I also hope to implement the creation and manipulation of resource locks on shares and access rules.

* * *

### Looking Ahead

As I move into the Resource Locks implementation, I am particularly looking forward to:

* Unit Testing: Writing comprehensive tests to ensure the stability of the new panel and the metadata enhancements.

* Iterative Refinement: I am eager to receive and iterate on code feedback from my mentors. Their insights have already been invaluable in helping me understand the OpenStack ecosystem.

I would like to extend a sincere thank you to my mentors **Goutham** and **Carlos** for their patience and guidance throughout these first six weeks. Your technical expertise and support have been the drivers of my progress, and I look forward to delivering a robust, high-quality feature set in the coming weeks.  

* * *
