---
tags: argo-write
title: Argo Write Template
---

{%hackmd qFrEnWCZRxezInZtIIYarg %}

---

:::info
The Argo Write Implementation Guide builds on the FHIR RESTful API specification. Note that the terms "EHR" and "Server" are used interchangeably throughout this guide
:::

# Argo Write Modality Value Set

[TOC]

<!-- Enter your content here -->

## Formal Representation

~~~yaml
resourceType: ValueSet
status: active
content: complete
name: ArgonautWriteModalityValueSet
id: modality-codes
title: Argonaut Write Modality Value Set
description: >-
  Used to tag patient submitted resources for writing and searching from EHRs
version: 0.1.0
url: >-
  http://www.fhir.org/guides/argonaut/argo-write/ValueSet/modality-codes
copyright: >-
  This Content is created by the Argonaut Project and HL7. No copyright acknowledgement is needed, nor are there any license terms to adhere to.
compose:
  include:
    - system: 'http://www.fhir.org/guides/argonaut/argo-write/CodeSystem/modality-codes'
~~~

{%hackmd 8ffOHhmtSKuk035ydksKiA %}