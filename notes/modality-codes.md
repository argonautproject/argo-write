---
tags: argo-write
title: Argo Write Modality Code System
---

{%hackmd qFrEnWCZRxezInZtIIYarg %}

---

:::info
The Argo Write Implementation Guide builds on the FHIR RESTful API specification. Note that the terms "EHR" and "Server" are used interchangeably throughout this guide
:::

# Argo Write Modality Code System

[TOC]

Concepts for data entered by patient or directly uploaded from the device

orginally defined here: [OMH](https://healthedata1.github.io/mFHIR/StructureDefinition-extension-modality.html)

:::success
uri: `http://www.fhir.org/guides/argonaut/argo-write/CodeSystem/modality-codes`
:::


|Code|Display|Definition|
|---|---|---|
|`sensed`|Sensed|Device measurement is sensed directly by the device|
|`self-reported`|Self Reported|Device measurement is entered by the user|

## Formal Definition

~~~yaml
resourceType: CodeSystem
status: active
content: complete
name: ArgonautWriteModalityCodeSystem
id: modality-codes
title: Argonaut Write Modality Code System
description: >-
  Used to tag patient submitted resources for writing and searching from EHRs
version: 0.1.0
url: >-
  http://www.fhir.org/guides/argonaut/argo-write/CodeSystem/modality-codes
concept:
  - code: sensed
    display: Sensed
    definition: >-
      Device measurement is sensed directly by the device.
  - code: self-reported
    display: Self Reported
    definition: >-
      Device measurement is entered by the user.
count: 2
~~~

{%hackmd 8ffOHhmtSKuk035ydksKiA %}