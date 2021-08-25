---
tags: argo-write
title: Argo Direct Write
---

[![hackmd-github-sync-badge](https://hackmd.io/UG_Lai1iRaC2posiQzl0zw/badge)](https://hackmd.io/UG_Lai1iRaC2posiQzl0zw)


{%hackmd qFrEnWCZRxezInZtIIYarg %}

---

:::info
the Argo Write API builds on the FHIR RESTful API specification. The terms "EHR" and "Server" are used interchangeably in the document below.
:::

# Argo Direct Write

[TOC]

## Overview

In the Argo Direct Write API approach, the provider accepts  patient supplied data *within the approved context of the user's permissions and the client's OAuth scopes* - in other words if an EHR policy allows this user to write vitals and a client is scoped to write vitals, then vitals are accepted. If a policy prohibits these data (e.g., an app is submitting too many vitals in a given time period), the vitals are rejected.  After data are accepted, they become visible over the FHIR API and will appear in search results.

The Providers and EHRs can do what they want with the accepted data (discard, queue up, review, summarize etc). They **MAY** update the Client on what happened to the data.  ( e.g., Dr Smith reviewed this!!) This part all occurs in the EHR backend. **Whether this data is "part of the clinical chart" becomes an internal distinction that providers can make; this distinction shouldn't make an external-facing difference. We leave this "clinical chart" distinction out of scope.** 

<!--
recall the Functional Requirements for Client:
- FRC-1 : Client SHALL be able to create Resource X on Server
- FRC-2 : Client SHOULD be able to interact with Resource X via the Server API normally after create
- FRC-3:  Client SHOULD / MAY(?) discover the "reliability" of data
-->

And from the viewpoint of the App ...

- immediate acceptance by the EHR, or
- immediate error response from the EHR

{%hackmd /WwsA0bNWSQ2OS5zbJFM_rw %}

---

## Client Supplied Context

In order to support this simple client facing API, there needs to be a way for the EHR to have enough context about the patient supplied data to be able to decide how to process it for downstream use. The following context could be important:

1. If the data is patient supplied (a.k.a its "reliability")
2. If the submission was solicited by a provider or not.
3. Overall data provenance

The elements listed below supply this context and enable the EHR to process patient submitted data.


### **Required**
  * `patient-supplied` Tag (e.g., `Observation.meta.tag`)


### **Optional**

If `patient-supplied` tagging alone in not enough for some use cases, these optional elements can be with the patient submitted resource as additional metadata.
* Represent fulfillment of a data request with a reference to an order using the `basedOn` element.
* For patient supplied measurements and observations,
    *  provide data about the Device used to capture the data using the `Observation.device` element.
    * provide data about the Gatewat Device used to transmit the data using the standard *observation-gatewayDevice* extension
    * a custom *observation-modality* extension to indicate whether data were entered by hand.
* `Provenance` resource to convey additional details about the source or process of creating these data.

{%hackmd bJjjGTLaRbic73rrjRyd8Q %}

### Questions
:thinking_face: Do we need to distinguish between what the server knows implicitly from authentication and what is provided by the client and needed for downstream users.
:raising_hand: Yes

:thinking_face: Should EHRs verify when data crosses their boundary?
:raising_hand: This is not in scope, but SHOULD be a best practice?

## Pros/Cons of "Direct Write"

:::info
We explored how to directly or indirectly write to an EHR using the FHIR RESTful API to cover the scenarios when submitting patient data to an EHR:

- immediate incorporation into the EHR
- immediate rejection from EHR
- asynchronous administrative or clinical review before deciding to incorporate or reject. (see [Questions](#Questions))

We heard from clients that the end-user experience will be greatly harmed if their writes are rejected asynchronously and thus chose this approach.
:::

* `+` Simple API at submission time
    * Client knows whether the request was accepted or rejected synchronously 
* `+` Avoids the need to manage/monitor request status (data are submitted and land directly in FHIR server)
* `-` Only works for automated execution where the decision to perform the request and the execution of the request can be done synchronously within the HTTP timeout period (generally on the order of 10s of seconds).
* `-` No way to cancel request.
* `-/+` What EHR does beyond accepting the data is opaque to client.
* `-/+` Need to decorate the resource up front to give EHR enough information to process data.
* `-/+` Lots of pre-coordination may be needed.
m thje

## Questions
:thinking_face: How Does Patient Know Provider can access data?
:raising_hand: Successful write implies this


:thinking_face: What about `POST`ing bundles? First steps are to support a minimal interaction - observation only. Do we want to address Bundles now or in future? Transaction/Batch processing rules prohibit returning multiple responses when the resources reference each other within the bundle. Posting a collection Bundle will only post to the Bundle endpoint.
- Therefore, you can not write A,B and C in a batch/transaction bundle to serverX. because if A references B and C the bundle it is non-confomant for a batch and the transaction bundle will fail "where the entire set of changes succeed or fail as a single entity".
- options "If we want a bundle, we use a collection type bundle. Or play fast and loose with transaction-response semantics." Or create a "loose transaction operation" which I attempted and failed. see [Argo InDirect Write Options](/FrBp2-AOQLGH4mLuZWbhuQ)

:thinking_face: What about a http `202` response with a location header for an async response?
:raising_hand: This is not a thing for FHIRRestful API see [Asynchronous Request Pattern](http://build.fhir.org/async.html)

{%hackmd 8ffOHhmtSKuk035ydksKiA %}