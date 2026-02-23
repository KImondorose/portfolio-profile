---
layout: post
title: "Updating My Initial Wireframes"
date: 2026-02-16
categories: [Outreachy, OpenStack, Internship]
author: "Rose Kimondo" 
tags: ["outreachy"] 
---

## From Feedback to Functionality: Refining the Manila-UI Wireframes

---

One of the most rewarding parts of this journey has been the iterative process of taking initial wireframes and refining them based on mentor feedback. It’s one thing to build a feature that "works," but it’s another to build one that feels native to the OpenStack ecosystem and respects the user’s workflow.  

After several deep dives with my mentors, we’ve made significant structural changes to how metadata and resource locks are handled. Here’s a breakdown of the major updates.  

To view the original wireframes and the blog post on them, please refer to this [blog post](http://127.0.0.1:4000/outreachy/openstack/internship/2026/01/03/wireframing.html)  

### 1. Relocating Metadata: Export Locations & Subnets

---

Initially, metadata was tucked away in places that felt a bit buried. ALso, these were in the detail views of each of their parent resources, making to difficult to inherit styling from Horizon to make the new UI similar to what is already on the dashboard. To improve discoverability and follow the "Horizon way," we shifted these into dedicated tabs:  

- Export Location Metadata: Instead of having these in the Share Detail view, we moved them directly into the Export Locations tab. I developed a dedicated table that lists the metadata keys and values associated with specific export paths, including the other details that are displayed for export locations.  

- Share Network Subnet Metadata: Similarly, for Share Networks, we moved metadata into the Subnets tab. By creating a nested table structure here, users can now see subnet-specific metadata here, plus all the other Subnet details.We also allowed for the creation of metadata during the creation of the network.  

- Share Snapshots Metadata: The only missing thing was the ability to set metadata during the creation of the snapshot, which was added.  

### 2. The Evolution of the Resource Locks Panel

---

The Resource Locks panel underwent a major architectural shift. We moved away from a "one-size-fits-all" list and implemented separate tabs for Share Locks and Access Rule Locks.  

Key Changes:  

- Contextual Creation: We removed the "Create Lock" button from the main panel. Logic dictated that a user shouldn't go to a central repository to create a lock; they should lock the resource from the resource itself (the Share or the Access Rule).  

- Safety First: We disabled bulk deletion of locks. Unlocking a resource is a high-stakes action, and we want to prevent accidental mass-unlocking that could lead to data loss.  

### 3. Deep Integration: Locking during Share & Rule Creation

---

We wanted the locking mechanism to feel like a natural part of the resource lifecycle.

- Dynamic UI: In the "Create Share" and "Edit Share" forms, we added a "Lock Share" checkbox near the "Make visible to all users" option. To keep the UI clean, the Lock Reason text box only appears if that checkbox is ticked.  

- Visual Cues: We are implementing a "Locked" column (Yes/No) or a lock icon in the main listings so users don't have to click into a resource to know it's protected.  

- The "Unrestrict" Logic: While Manila supports an --unrestrict flag during deletion, we are adding an explicit "Delete Lock" action in the dropdown menu, complete with a confirmation modal. Unlocking is serious business!  

### 4. Handling Deletion Attempts on Locked Resources

---

What happens when a user tries to delete something that is locked? We decided on a two-pronged approach:  

- Individual Deletion: If a user clicks delete on a locked share, the button remains clickable, but they are met with a clear error message explaining why the action failed. This is the standard OpenStack approach.  

- Bulk Deletion: If a user selects multiple shares and even one is locked, the global "Delete Shares" button is disabled, accompanied by a warning that locks must be removed first.  

### 5. Access Rules: Security Alignment

---

A major logic change occurred regarding Access Rules. Originally, visibility and deletion locks were separate concepts.  

- The Change: When a user locks an access rule, it now automatically locks both visibility and deletion.  

This ensures that if you are protecting a rule from being changed, you are also protecting it from being wiped out entirely. We also ensured that Editing a Lock happens exclusively in the Resource Locks Panel, while Creating a Lock happens on the Shares/Access Rules pages.  

## THE WIREFRAMES

Here are the updated wireframes:  
**[Share Snapshot Metadata](https://app.diagrams.net/#G1LBqQnlHcVZqaDtccoyiNyITxaN-n6YLa#%7B%22pageId%22%3A%22khI8fEerpQ2f3SOm4UT9%22%7D)**  
**[Share Network Subnets Metadata](https://app.diagrams.net/#G1meohb9AympeiWg_opGrUI0g47qjKI2ES#%7B%22pageId%22%3A%226R8N52NH7Yi9054HXiX7%22%7D)**  
**[Share Export Locations Metadata](https://app.diagrams.net/#G1ly8IjVbR7Q0KVCShlsxVUgn-saqzX0_y#%7B%22pageId%22%3A%22R0JvyZ_vwMESuLTEeSjt%22%7D)**  
**[Resource Locks Panel](https://drive.google.com/file/d/1J1vjezJs8AVM8xpkUzP-RSCchT3PIxs9/view?usp=sharing)**  
**[Access Rule Locks](https://app.diagrams.net/#G1rBt6Cz2sw5i7hHg19bH3snBfxVjroYRd#%7B%22pageId%22%3A%22xtgie1VdkAJAhFkf50Ag%22%7D)**  
**[Share Deletion Locks](https://app.diagrams.net/#G1d-FtSYyzOxTYwg_5rsYNKkgXmeDOgKFx#%7B%22pageId%22%3A%22AZzZnc4egOZRNIlnPf8d%22%7D)**  

For the original wireframes. Please refer to this [blog post](https://rosekimondo.dev/outreachy/openstack/internship/2026/01/03/wireframing.html)

## Moving Forward

---

These changes move the project away from being just a "feature add-on" and toward being a robust, safety-conscious part of the Manila UI. By forcing the user to be intentional about unlocking and providing clear visual feedback, we're building a much more resilient interface.  

---
