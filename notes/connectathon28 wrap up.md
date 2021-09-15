---
tags: argo-write
title: "Argonaut Write Connectathon 28 Wrap Up"
---

{%hackmd qFrEnWCZRxezInZtIIYarg %}

---


# Argonaut Write Connectathon 28 Wrap Up

1. No Client/Servers Testing

    2. Still in early stages of client write.
    3. Recent updates to spec
4. Postman Collection available to demonstrate the simple transaction

    5. Uses a mock server

## Introduction Call


### Review Australian Specification: 

[AU FHIR Smart App Launch:Writing back to the clinical record](https://confluence.hl7australia.com/display/PA/Writing+back+to+the+clinical+record
)

Options being considered
  
 - For DocumentReference splitting out binary upon receipt
 - Use Transaction Bundles? reviewing options

### Recognize need for there pre-coordination steps including  *prior* to client being able to write to EHR

Including
- authorization
- "submission key" for requested data
- registration of a Device

### Other Specifications addressing Patient's Writing to EHRs:

- [POCD](https://build.fhir.org/ig/HL7/uv-pocd/)
- [PHD](http://build.fhir.org/ig/HL7/PHD/)
- [Finnish PHR](https://www.kanta.fi/en/system-developers/kanta-phr) 
- [MedMij FHIR Implementation Guide Vital Signs 2.0.11](https://informatiestandaarden.nictiz.nl/wiki/MedMij:V2020.01/FHIR_VitalSigns)


## Patient Empowerment Break-Out Comments

- [Patient Corrections Implementation Guide](http://build.fhir.org/ig/HL7/fhir-patient-correction/index.html) focus is  on ability to follow up on request.

- Considering Using Communication to record and provide addition context to resources in addition to resources.

-  Recognize that Performer vs Informant vs Author are different things instead of just referring to a perfomer
   - clean up language and mapping over resources.

- Argo Write Client can track workflow via tagging/deleted

## Breakout on US Core Must Support element for Write

Discussion on the [Draft Conformance Guidance](https://hackmd.io/ipt4Iyc3TMmwkB6PYNYnKw?view) and what does *Must Support* mean for client writing a profile and a Server consuming it.

Review draft of 6 bullets on *Must Support* to use when following a profile that define an element as Must Support.

Issues:

1. What about client authored DARs?
    1. The code meaning may change when server stores the data.
    
        When would this happen?  
        DAR codes for MS elements:
          - `unknown`
          - `masked`
          - `not-applicable`
          - `error`
          - `not-performed`
          - `not-permitted`

    1. Would there need to be provenance on that particular DAR element?

          e.g. a mandatory element as DAR = "unknown" - the Server store that DAR and supplies the provenance on the record vs the element - how would that be different?
          
1.  What if there were no MS requirements on Client Writes?
    1.  Server would only accept based on its business rules
    2.  How would Client know these business rules?
    3.  Profiles like US Core, provide the minimum expectations data elements across servers. Using them (and other profiles) provides a way for Client to interact with multiple servers without having to discover the requirements for each one separately.
    4.  Server May have additional requirements beyond US Core
        - how would you discover those additional requirements?

        e.g., for Observation server A also needs an episode of care too... (does this relate to the tagging/provenance like .basedOn)
  

## Wrap up Discussion

### Whether the uploaded data tag is necessary

- Use "workflow resource instead"  ( e.g., ServiceRequest. Task)


### How to provisioning a patient observation through ServiceRequest

- This is a precondition and not detailed
- Whether unsolicited data is out of scope - we have concluded it is not

### Registration of a Device

- This is a precondition and not detailed

### Using backend services to write data to EHR

```plantuml
cloud "DeviceServer" 
node Device
node EHR 
node "ClientApp"
DeviceServer <-left- Device: upload\ndata
EHR <-up- DeviceServer: provide\ndevice\ndata
EHR <-left- ClientApp: query\nfor\ndata
```

- common for devices to send measurements to a dedicated server
- instead of client app write to the ehr the service writes to it.
(or the ehr/provider fetches it)
- Out of scope?




Questions... PM me on Zulip  @**Eric Haas** 

{%hackmd 8ffOHhmtSKuk035ydksKiA %}
