---
tags: argo-write
title: "Conformance"
---

[![hackmd-github-sync-badge](https://hackmd.io/ipt4Iyc3TMmwkB6PYNYnKw/badge)](https://hackmd.io/ipt4Iyc3TMmwkB6PYNYnKw)


{%hackmd qFrEnWCZRxezInZtIIYarg %}

---

# Conformance

:::info
The Argo Write Implementation Guide builds on the FHIR RESTful API specification. Note that the terms "EHR" and "Server" are used interchangeably throughout this guide
:::

## Argo Write Elements

{%hackmd OhZSpHqdS-izNFtCj9fGRQ %}


## Assumptions:

1. Actors  
   :medical_symbol: Data Consumer = EHR or "Server"  
   :iphone: Data Submitter = Client App or "Client"
3. EHR **SHALL** have a CapabilityStatement stating what resources/profiles it supports for write.
4. Client App **MAY** have a CapabilityStatement  - we are not depending on that.
5. In addition to the must support rules below, the EHR and Client App **SHALL** follow the base specification guidance for a [FHIR RESTful create](http://hl7.org/fhir/R4/http.html#create)

## MS Rules

|No|<div style="width:290px">Search/Read US Core Profiles</div>|<div style="width:290px">Write Argo Profiles</div>|
|-----|-----|-----|
|1|US Core Responders **SHALL** be capable of populating all data elements as part of the query results as specified by the US Core Server Capability Statement.|EHRs **SHALL** be capable of storing all must support data elements as part of the write transaction as specified by the Argo Write Capability Statement.|
|2|US Core Requestors **SHALL** be capable of processing resource instances containing the data elements without generating an error or causing the application to fail. In other words US Core Requestors **SHOULD** be capable of displaying the data elements for human use or storing it for other purposes.|Client Apps **SHALL** be capable populating all must support data elements|
|3|In situations where information on a particular data element is not present and the reason for absence is unknown, US Core Responders **SHALL** NOT include the data elements in the resource instance returned as part of the query results.|In situations where information on a particular data element is not present and the reason for absence is unknown, Client Apps **SHALL** NOT include the data elements in the resource instance.|
|4|When querying US Core Responders, US Core Requestors **SHALL** interpret missing data elements within resource instances as data not present in the US Core Responder’s system.|When accepting a submitted resource, EHR **SHALL** interpret missing data elements within resource instances as data unavailable to the Client App.|
|5|In situations where information on a particular data element is missing or suppressed refer to the the guidance for Missing Data and Suppressed Data. In situations where information on a particular data element is missing and the US Core Responder knows the precise reason for the absence of data (other than suppressed data), US Core Responders **SHOULD** send the reason for the missing information. This is done by following the same methodology outlined in the Missing Data section, but using the appropriate reason code instead of unknown.|Client App **MAY** send the reason for the missing information. This is done by following the same methodology outlined in the US Core Missing Data section, but using the appropriate reason code instead of unknown.|
|6|US Core Requestors **SHALL** be able to process resource instances containing data elements asserting missing information.|EHRs **SHALL** be able to process resource instances containing data elements asserting missing information.|




{%hackmd 8ffOHhmtSKuk035ydksKiA %}