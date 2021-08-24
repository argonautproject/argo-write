---
tags: argo-write
title: Introduction
---

[![hackmd-github-sync-badge](https://hackmd.io/wTGb4Gk6R6O4NVut5yaJig/badge)](https://hackmd.io/wTGb4Gk6R6O4NVut5yaJig)


{%hackmd qFrEnWCZRxezInZtIIYarg %}

---

# Introduction

[TOC]

:::info
Note that the terms "EHR" and "Server" are used interchangeably throughout this guide
:::

## Background

Currently, limited FHIR Write capabilities are supported by EHRs today. In addition, little guidance is provided on writing and updating data in the context of USCDI Core profiles in FHIR.   There are multiple issues that need to be considered when defining expected behavior by the client and EHR to support updates and writes to the data. The Goal of this implementation guide is to provide a single standardized approach to patient FHIR writes to EHR.

This guide defines the basic *functional requirements* for the Client Apps and EHR.  It outlines the minimum set of use cases and scenarios that are in scope and what is out of scope. The technical details of the  *Argo Write API* are defined and implementer resources such as examples and FHIR artifacts are provided. The open issues and next steps in advancing this specification are also documented.

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

## In Scope Use Cases

***Patient Generated Data***: data originating with the patient, patient's device, or patient care-giver. This is **in scope** for this project.

### Use Case Categories Covered by Argo Write

|Use Case Category|Resource exchanged|
|---|---|
|Vitals (e.g. : Weight, Blood Pressure)|Observation|
|Activity (e.g. Steps,Sleep|Observation|
|At home diagnostics (Blood Glucose, Covid 19 Test)|Observation|
|non-FHIR Documents (e.g. photos, patient records)|DocumentReference|
|Assessments*(e.g., pain scores, patient reported Outcomes)|QuestionnaireResponse/Observation|
|Medications|Medication/MedicationRequest|
|Immunization|Immunization|
|Allergies|AllergyIntolerance|
|Problems|Condition|
|Goals|Goal|


\*Assessments:
1. For FHIR, assessments are written to EHR in the context of a Questionnaire/QuestionnaireResponse interaction with multiple actors such as a form sender, form filler, completed form repository.  See the [Structured Data Capture](http://build.fhir.org/ig/HL7/sdc/) Implementation Guide for more details.
2. Assessments may also be captured as Observations as well.  Should this be scope for this project?


:::info
These Use Cases are out-of-scope for this version:

- Provider Write
- Patient Updates
- Patient Corrections (see the draft [Patient Corrections Implementation Guide](http://build.fhir.org/ig/HL7/fhir-patient-correction/index.html), published by teh HL7 Patient Empowerment Workgroup)
- Patient Mediated Data: data forwarded which is authored by an outside provider
:::


{%hackmd 8ffOHhmtSKuk035ydksKiA %}