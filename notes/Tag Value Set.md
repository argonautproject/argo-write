---
tags: argo-write
title: Argo Write Tag Value Set
---

[![hackmd-github-sync-badge](https://hackmd.io/rNeFJvrpTiWifNNjDElxYA/badge)](https://hackmd.io/rNeFJvrpTiWifNNjDElxYA)


{%hackmd qFrEnWCZRxezInZtIIYarg %}

---

:::info
The Argo Write Implementation Guide builds on the FHIR RESTful API specification. Note that the terms "EHR" and "Server" are used interchangeably throughout this guide
:::

# Argo Write Tag Value Set

[TOC]

<!-- Enter your content here -->

  {%hackmd  areWLwOMSLSx6xh_kkn6Nw %}

## Formal Representation:
~~~yaml
resourceType: ValueSet
status: active
content: complete
name: ArgonautWriteTagValueSet
id: tags
title: Argonaut Write Tag Value Set
description: >-
  Used to tag patient submitted resources for writing and searching from EHRs
version: 0.1.0
url: >-
  http://www.fhir.org/guides/argonaut/argo-write/ValueSet/tags
copyright: >-
  This Content is created by the Argonaut Project and HL7. No copyright acknowledgement is needed, nor are there any license terms to adhere to.
compose:
  include:
    - system: 'http://www.fhir.org/guides/argonaut/argo-write/CodeSystem/tags'
~~~

{%hackmd 8ffOHhmtSKuk035ydksKiA %}