---
tags: argo-write
title: Argo Direct Write
---

[![hackmd-github-sync-badge](https://hackmd.io/UG_Lai1iRaC2posiQzl0zw/badge)](https://hackmd.io/UG_Lai1iRaC2posiQzl0zw)


{%hackmd qFrEnWCZRxezInZtIIYarg %}

---

:::info
The Argo Write Implementation Guide builds on the FHIR RESTful API specification. Note that the terms "EHR" and "Server" are used interchangeably throughout this guide
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

1. If the data is supplied by a patient facing app.
1. Who authored the data (a.k.a its "integrity")
2. If the submission was solicited by a provider or not.
3. Overall data provenance


The elements listed below supply this context and enable the EHR to process patient submitted data.


**Required**
  * `patient-supplied` Tag (e.g., `Observation.meta.tag`)

**Recommended**
  * "integrity label" (e.g., `Observation.meta.security`): In addition, inbound data **SHOULD** be tagged by the EHR with an "integrity label" based on the authorization context so that the source (patient, parent, spouse) of the of the data is exposed.

**Optional**

If `patient-supplied` tagging alone in not enough for some use cases, these optional elements can be supplied with the patient submitted resource as additional metadata. 
* Represent fulfillment of a data request with a reference to an order using the `basedOn` element.
* For patient supplied measurements and observations,
    *  provide data about the Device used to capture the data using the `Observation.device` element.
    * provide data about the Gatewat Device used to transmit the data using the standard *observation-gatewayDevice* extension
    * a custom *observation-modality* extension to indicate whether data were entered by hand.
* `Provenance` resource to convey additional details about the source or process of creating these data.
    :::info
    Two types of  "small p" provenance exists for Patient-submitted and data will often have both types:
    1. Server supplied provenance is typically established automatically when data crosses the app/server boundary (e.g. client ID, user)
    2. Client supplied provenance provided by app/device and or end user
    This guide only documents using elements *implicit* in the resource (for example, `Observation.device`) to represent client supplied provenance. It is silent on the use of the Provenance resource. 
    :::

### "Patient-Supplied" Tag 
 
In order for the EHR to know that this is patient submitted data, at a bare minimum a *Patient-Supplied* tag is required.
- Technically a `Coding` datatype on the `meta.tag` element 
- FHIR searches find resources with this tag by default; clients that don't want them can filter them out, through search parameters
- Each concept should have a set of rules defined by the EHR.
- Providers and EHRs can do what they want with these data (e.g., leaving them tagged; reviewing and adding a provider-reviewed tag; reviewing and stripping the patient-supplied tag; deleting them...)

system = `http://www.fhir.org/guides/argonaut/argo-write/CodeSystem/tags`


|Code|Display|Definition|
|---|---|---|
|`patient-supplied`|Patient Supplied Data|Data is supplied by patient - either patient generated data or data generated elsewhere and forwarded by patient (todo get references to definitions of PGD)|
|`provider-reviewed`|Provider Reviewed Data|Data is supplied by patient and has been reviewed by provider ( either manually or through some automated fashion)|


#### Discussion...

:thinking_face: Why [`meta.tag`](http://hl7.org/fhir/R4/resource-definitions.html#Meta.tag) over using [`meta.security`](http://hl7.org/fhir/R4/resource-definitions.html#Meta.security)?  
:raising_hand: Standard FHIR search works "out-of-the-box" for both and based on feedback, both can be supported by implementers.  Using meta.security seems counter-intuitive for this use case. Security labels are used to "determine what handling caveats must be conveyed with the data". On the other hand, tags are "used to relate resources to process and workflow" which is aligned with the intent of patient supplied tagging.

:thinking_face: Instead of creating and defining set of concepts from scratch, adopt concepts from existing [V3 Value SetSecurityIntegrityObservationValue] (http://hl7.org/fhir/v3/SecurityIntegrityObservationValue/vs.html)  
:raising_hand: Although the concepts overlap, the definitions are narrowly focus on security.

:thinking_face: Should the App (data source) or EHR (data consumer) apply the tag?  
:raising_hand: This can be supplied by clients but ultimately must be enforced by servers receiving data, so even if clients omit the tag, servers just apply it upon receipt.

---

### "Integrity Label"

Inbound data **SHOULD** be tagged by the EHR with an "integrity label" based on the authorization context so that the source (patient, parent, spouse) of the of the data is exposed.

- Technically a `Coding` datatype on the `meta.security` element 
- FHIR searches find resources with this tag by default; clients that don't want them can filter them out, through search parameters
- Each concept should have a set of rules defined by the EHR.
- Providers and EHRs can do what they want with these data (e.g., leaving them tagged; reviewing and adding a provider-reviewed tag; reviewing and stripping the patient-supplied tag; deleting them...)

value set = `http://terminology.hl7.org/ValueSet/v3-SecurityIntegrityObservationValue`

Specifically these codes may be useful to indicate that the source is the patient or family member or caregiver

|Code|Display|Definition|
|---|---|---|
PATAST|patient asserted|Security provenance metadata observation value used to indicate that an IT resource (data, information object, service, or system capability) was asserted by a patient.
SDMAST|substitute decision maker asserted|Security provenance metadata observation value used to indicate that an IT resource (data, information object, service, or system capability) was asserted by a substitute decision maker.

---

### "Submission Key" 
 
In addition to decorating the resource with patient-supplied data, the resource can be identified as being solicited or ordered via a *Submission Key*. This is an identifier that can directly or indirectly refer back to the order for data. 

- Providers and EHRs can are able to match the patient submitted data to a particular request using the `basedOn` element.
- Technically a `Reference` datatype on the [`basedOn`](http://hl7.org/fhir/observation-definitions.html#Observation.basedOn) element which references the order that this Observation fulfills.
    - It can be a FHIRid, or a  business identifier.
- Through standard search parameters, FHIR searches can find resources that are linked to an order. 


Each time a new weight measurement is recorded, the app POSTs it to the EHR, populating `Observation.basedOn` with a link to the Order Identifier in step 2.

#### Discussion...

:thinking_face: Who supports the `basedOn` element today?  
:woman-raising-hand: Based on feedback, the element is supported by implementers.

:thinking_face: What would `basedOn` refer to if fhir_id?  
:woman-raising-hand: For patient reported observations, most commonly `ServiceRequest`. `Task` or other request resources could be potential targets depending on the type of data submitted.

:thinking_face: What about 'unsolicited' data.  
:man-raising-hand: See this sidebar on [Changing Unsolicited to Solicited Writes](/tHWOh7x4R8OTU5bpMXwkLQ) on turning 'usolicited' data into 'solicited' data

---

### Device Data

In addition to decorating the resource with patient-supplied data and the Submission Key the resource can provide different amounts of provenance. The [`device`](http://hl7.org/fhir/observation-definitions.html#Observation.device) element is one simple way to represent data provenance specifically for Observations.

- Clients can represent what entity is the source of the data using the `device` element.
- Technically a `Reference` datatype which may identify the device ( e.g, the provisioned scale) which took the measurement ( weight)
- It can be a FHIR_id, and/or a business identifier and/or text string.
 
For example in step 2 above, each time a new weight measurement is recorded, the app POSTs it to the EHR, populating `device` with a provider supplied FHIR_id, UUID, or display for the provisioned device.  The provider would then be able to use this information to identify the device used.

### Gateway Data

If the gateway's (a.k.a., phone app's) provenance is important, then use the standard extension: 

http://hl7.org/fhir/StructureDefinition/observation-gatewayDevice

### Performer Data

The `performer` element represents who made the measurement, it can be the patient or a care giver for example.

### Modality Extension

This **optional** extension would represent if the data entered by patient or directly uploaded from the device

orginally defined here: [OMH](https://healthedata1.github.io/mFHIR/StructureDefinition-extension-modality.html)

uri: `http://www.fhir.org/guides/omhtofhir/StructureDefinition/extension-modality`

*NOTE: This extension was originally defined to go on the device element, but think is better on the Observation*

|Code|Display|Definition|
|---|---|---|
|`sensed`|Sensed|Device measurement is sensed directly by the device|
|`self-reported`|Self Reported|Device measurement is entered by the user|


## Use Case 1: Solicited Weight Submissions

{%hackmd bJjjGTLaRbic73rrjRyd8Q %}


## Use Case 2: Patient Submitted Photo

{%hackmd bJjjGTLaRbic73rrjRyd8Q %}

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
:thinking_face: When client app write to server and references other resource (e.g. basedOn,device,patient) what are the options?
:raising_hand: Options are:
- FHIR_id to external resource (e.g. Device/123)
- FHIR_id to contained resource (e.g. #mydevice)
- business identifier (e.g. "submission key" for basedOn)
- text string (e.g. "Acme bluetooth enabled weight scale")

:thinking_face: How Does Patient Know Provider can access data?
:raising_hand: Successful write implies this

:thinking_face: What about `POST`ing bundles? First steps are to support a minimal interaction - observation only. Do we want to address Bundles now or in future? Transaction/Batch processing rules prohibit returning multiple responses when the resources reference each other within the bundle. Posting a collection Bundle will only post to the Bundle endpoint.
- Therefore, you can not write A,B and C in a batch/transaction bundle to serverX. because if A references B and C the bundle it is non-confomant for a batch and the transaction bundle will fail "where the entire set of changes succeed or fail as a single entity".
- options "If we want a bundle, we use a collection type bundle. Or play fast and loose with transaction-response semantics." Or create a "loose transaction operation" which I attempted and failed. see [Argo InDirect Write Options](/FrBp2-AOQLGH4mLuZWbhuQ)

:thinking_face: What about a http `202` response with a location header for an async response?
:raising_hand: This is not a thing for FHIRRestful API see [Asynchronous Request Pattern](http://build.fhir.org/async.html)

:thinking_face: Do we need to distinguish between what the server knows implicitly from authentication and what is provided by the client and needed for downstream users.
:raising_hand: Yes

:thinking_face: Should EHRs verify when data crosses their boundary?
:raising_hand: This is not in scope, but SHOULD be a best practice?


{%hackmd 8ffOHhmtSKuk035ydksKiA %}