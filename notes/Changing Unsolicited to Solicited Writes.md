---
tags: argo-write
title: Changing Unsolicited to Solicited Writes
---

{%hackmd qFrEnWCZRxezInZtIIYarg %}

---
# Changing Unsolicited to Solicited Writes:

Any compatible consumer-facing health app that connects to the provider and may complete a "link my weight scale to my health records" workflow. **magic happens here, and causes the app to learn a *Order Identifier*** to associate with any Observations it wants to post. Many possible ways for this magic to unfold:


* **Portal interaction resulting in an Order Identifier** ?? possibly via OAuth. e.g., app requests authz with special scopes like `Observation.c`)
  Portal is smart enough to see "*Oh, the provider has ordered data like that! Sure, we'll approve you and give an access token, but please click through this langauge to make sure you understand the limitations... provider may not see or respond to data immediately, if you're having an emergency go to the ED instead of relying on this workflow, etc, etc*" SMART access token response could include a `relevantOrder` object supplying details....
* **Query for a pre-allocated Order Identifier** ?? App queries for `GET /ServiceRequest?patient=123&category=data-submission&code=loinc:123` and finds one that matches its own capabilities....
* **Type or scan a pre-allocated Order Identifier** ?? Scan a provider-generated QR that embeds some authorization or `ServiceRequest.identifier`?
* **App creates its own Order Identifier** ?? Let the app POST its own Device directly to the EHR

:::



{%hackmd 8ffOHhmtSKuk035ydksKiA %}