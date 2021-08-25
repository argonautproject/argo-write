---
tags: argo-write
title: Argo Write Tag Code System
---

{%hackmd qFrEnWCZRxezInZtIIYarg %}

---

:::info
The Argo Write Implementation Guide builds on the FHIR RESTful API specification. Note that the terms "EHR" and "Server" are used interchangeably throughout this guide
:::

# Argo Write Tag Code System

[TOC]

:::success
CodeSystem url = `http://www.fhir.org/guides/argonaut/argo-write/CodeSystem/tags`
:::

|Code|Display|Definition|
|---|---|---|
|`patient-supplied`|Patient Supplied Data|Data is supplied by patient - either patient generated data or data generated elsewhere and forwarded by patient (todo get references to definitions of PGD)|
|`provider-reviewed`|Provider Reviewed Data|Data is supplied by patient and has been reviewed by provider ( either manully or through some automated fashion)|

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