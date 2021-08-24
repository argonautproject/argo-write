---
tags: argo-write
title: Functional Requirements
---

[![hackmd-github-sync-badge](https://hackmd.io/ugzK-AhsRWK5L-xKhH4Xrw/badge)](https://hackmd.io/ugzK-AhsRWK5L-xKhH4Xrw)


{%hackmd qFrEnWCZRxezInZtIIYarg %}

---

# Functional Requirements


[TOC]

## Actors

1. EHRs = “Server”  
   - In this case the EHR is ultimate recipient of data.
   - When we say “EHR” in this document, we don’t mean to limit ourselves to electronic health record systems; the same technology can be used in HIEs, care coordination platforms, population health systems, etc. Please read “EHR” here as a short-hand notation for something like “interoperable healthcare platform 
1. Client App = “Client”  
   - As defined in the Scope section below, the Client App is a *patient facing* application
   - In this case the Client App is adding/updating data
   - Often a phone app
1. Patient  = client end-user
1. Provider =  server end-user

## Preconditions/Assumptions
- **SHALL** support FHIR, SMART and Bulk Data APIs
- Common behaviors **SHALL** apply across all requests and responses.
- Server **MAY** Need to Pre-Coordinate with Client App. (for example, an EHR may supply end user with a device that needs to be registered with App and EHR)
- Client only interacts with a single URI
    :::info
    In other words, no assumptions are made about the URI space of Server. The EHR may have Multiple Databases With One “System of Record” but this is often a proxy for whether data should be treated as "reliable" and where it came from. (The details of how a server determines data reliability is an internal implementation detail.) Using multiple URIs to reflect multiple endpoints was discussed, but was discarded in favor of "tagging" the submitted data to reflect its source.
    :::
## Functional Requirements
### Functional Requirements for Client 
1. FRC-1 : Client **SHALL** be able to create FHIR resources on a Server
1. FRC-2 : Client **SHOULD** be able to fetch and query for the FHIR resources it has created via the Server's FHIR API. 
1. FRC-3:  Client **SHOULD** be able discover the Argo Write defined tags on resources.

### Functional Requirements for Server
1. FRS-1 : Server **SHALL** be able to create a FHIR resource written by the Client
1. FRS-2 : Server **SHOULD** be able support the fetching and querying for the written resource 
1. FRS-3:  Server **SHOULD** be able to communicate the the Argo Write defined tags on resources.


:::info
These Functional Requirements were considered, but were determined to be out-of-scope for this version:
- FR-4 : Whether Client written resource is readable by other EHR clients
- FR-5 : Whether Client written resource will become readable by other EHR clients
- FR-6 : Ability to assign priority to resource
- FR-7 : Ability to provide provenance
:::

<!-- Enter your content here -->




{%hackmd 8ffOHhmtSKuk035ydksKiA %}