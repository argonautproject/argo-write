---
tags: argo-write
title: "Argo Write Elements"
---


Argo Write Provenance|Argo Write Element|Mandatory (min=1)|Must Support|Supplied By (C\|S)
---:|---:|:---:|:---:|:---:
 If the data is supplied by a patient facing app. (a.k.a *uploaded-data* Tag)|`.meta.tag`)|:X:|:heavy_check_mark:|S
If the submission was solicited by a provider or not.|`.basedOn`|:X:|:question:|C
Who authored the data|`.perfomer`|:X:|:question:|C
Devices  used to collect the data|`.device`|:X:|:question:|C
Gateways used to collect the data|gateway extension|:X:|:question:|C
How the data was collected|modality extension|:X:|:question:|C

These "little p" provenance data element are discussed in detail in the sections below