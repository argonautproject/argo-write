---
tags: argo-write
title: Argo Write Tag Code System
---

[![hackmd-github-sync-badge](https://hackmd.io/vYg8IupeSSi4ZK4x3LOPEA/badge)](https://hackmd.io/vYg8IupeSSi4ZK4x3LOPEA)


{%hackmd qFrEnWCZRxezInZtIIYarg %}

---

:::info
The Argo Write Implementation Guide builds on the FHIR RESTful API specification. Note that the terms "EHR" and "Server" are used interchangeably throughout this guide
:::

# Argo Write Tag Code System

[TOC]

{%hackmd  areWLwOMSLSx6xh_kkn6Nw %}

## Formal Representation:

~~~yaml
resourceType: CodeSystem
status: active
content: complete
name: ArgonautWriteTagCodeSystem
id: tags
title: Argonaut Write Tag Code System
description: >-
  Used to tag patient submitted resources for writing and searching from EHRs
version: 0.1.0
url: >-
  http://www.fhir.org/guides/argonaut/argo-write/CodeSystem/tags
concept:
  - code: patient-supplied
    display: Patient Supplied Data
    definition: >-
      Data is supplied by patient - either patient generated data or data
      generated elsewhere and forwarded by patient (todo get references to
      definitions of PGD).
  - code: provider-reviewed
    display: Provider Reviewed Data
    definition: >-
      Data is supplied by patient and has been reviewed by provider (either
      manually or through some automated fashion).
count: 2
~~~


{%hackmd 8ffOHhmtSKuk035ydksKiA %}