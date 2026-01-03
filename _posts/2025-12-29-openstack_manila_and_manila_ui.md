---
layout: post
title: "OpenStack Manila Structure and Commands"
date: 2025-12-29
categories: [Outreachy, OpenStack, Internship]
author: "Rose Kimondo" 
tags: ["outreachy"] 
---

## What is Manila?

[OpenStack Manila](https://docs.openstack.org/manila/latest/) is the Shared File System Service for OpenStack. It provides a standardized, cloud-based way for users to provision and manage file-based storage resources (like NFS or CIFS shares) on demand, just like they can provision virtual machines or block storage.

### Core Function: Storage Orchestration

Manila's primary job is to abstract and manage the complexity of various back-end storage systems.

* Self-Service Provisioning: It allows users to request a new "Share" (a file system) of a specific size and protocol (e.g., NFS, CIFS, CephFS) without needing to know the technical details of the underlying storage hardware.

* Back-end Management: It uses vendor-specific drivers (for NetApp, IBM, Ceph, etc.) to talk to the actual storage appliances. It translates the user's generic request into the specific commands required by the back-end storage system to create and export the file system.

* Access Control: It manages who can access the file share. Users specify access rules (e.g., allow read/write access to a specific IP address or security service like LDAP/Kerberos), and Manila enforces these rules on the back-end.

### Key Operations

* Create/Delete Shares: The fundamental lifecycle management.

* Snapshots: Create point-in-time, read-only copies of a share.

* Resize: Extend or, in some cases, shrink a share's size.

* Replication: Managing asynchronous copies of shares across availability zones for disaster recovery.

### The Manila Architecture Components

Manila itself is a set of services that handle the requests:

* manila-api: Receives the user's REST API requests, handles authentication (via Keystone), and forwards the request to the rest of the Manila services.

* manila-scheduler: Determines which back-end storage system (driver) is best suited to fulfill a new share request based on criteria like capacity, capabilities, and availability zone.

* manila-share: The manager service that communicates directly with the specific back-end storage device using its driver to perform the actual creation, deletion, and management tasks.

### Manila Commands

Here, I will list some of the commands that are relevant to my internship project which has two major tasks which are adding new capabilities to the UI plugin to: allow creating or manipulating metadata of a share, snapshot, access rule or share network subnet and  allow creating or manipulating a resource lock on a share or access rule.  

For a comprehensive list of all the commnds, view them [in Manila's documentation](https://docs.openstack.org/manila/latest/user/create-and-manage-shares.html)  

* Allow access: Allow read-write access (Create access rule). N.B: The uuid is the access rule's id.

```python
manila access-allow myshare ip 10.0.0.0/24 --metadata key1=value1    
or  
openstack share access create
```

* Update access rule's metadata and view it
N.B: The uuid is the access rule's id.

``` python
manila access-metadata 2950cc07-79c5-484c-b034-34fa6acc19a0 set key2=value2
or
openstack share access set --property

manila access-show 2950cc07-79c5-484c-b034-34fa6acc19a0
```

* Removing an access rule's metadata key value. N.B: The uuid is the access rule's id.

``` python
manila access-metadata 2950cc07-79c5-484c-b034-34fa6acc19a0 unset key1
or
share access unset --property

manila access-show 2950cc07-79c5-484c-b034-34fa6acc19a0
or
openstack share access show 2950cc07-79c5-484c-b034-34fa6acc19a0
```

* Share Metadata: Setting and unsetting metadata items on your share

``` python
manila metadata myshare set purpose='storing financial data for analysis' year_started=2020
or
openstack share set --property or share unset --property
```

* Snapshot metadata: Setting and unsetting metadata items on the share snapshot during creation or after creation

``` python
openstack share snapshot create myshare --name mysnapshot \
    --property key1=value1 --property key2=value2

openstack share snapshot set mysnapshot --property key1=value

openstack share snapshot unset mysnapshot --property key1
```

* * *

## What is Manila UI?

[Manila UI](https://docs.openstack.org/manila-ui/latest/) is the management dashboard component of Manila. It is the plugin that integrates into the main OpenStack user interface, [Horizon](https://docs.openstack.org/horizon/latest/)  

It provides the GUI/front-end presentation layer that allows cloud users and administrators to interact with the Manila service without needing to use the command line or API directly.  

It is implemented as a Django plugin for OpenStack and the code contains Python for the Django logic and HTML for the front-end templates.  

Through the Manila UI, users can perform operations like:

* Creating and deleting file shares.
* Managing access rules for shares.
* Viewing details, status, and usage of their file shares.
* Managing share networks and share types.

In short, the Manila UI renders the "Shares" tab (or similar panels) within the OpenStack dashboard, translating the Manila API calls into a friendly, web-based control panel.

* * *
