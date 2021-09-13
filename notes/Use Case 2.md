---
tags: argo-write
title: 'Use Case 2: Patient Submitted Wound Photo'
---

#### Description

This simple example uses the Argo Write API to submit a patient supplied wound photo to her provider using the FHIR DocumentReference resource.

#### Assumptions/Preconditions:

- Provider requests the patient submit a photo through a provisioned app
- Patient is provided an app (gateway) and instructed how to use
- Patient app has proper authorization and patient write scopes to the EHR
- Patient is provided a *Data Submission Key*. It represents fulfillment of a data request and can be referenced from the DocumentReference.
- Patient app has proper authorization and patient write scopes to the EHR
- Patient app directly writes to EHR ("unsupervised" data)
- The server accepts all the data for review based on established policy
- The healthcare providers  explain to patients at authorization time, "hey, you want to allow this app to write data? That's fine but keep in mind we don't guarantee we'll review it in a timely fashion..."
- **Resources are submitted one at a time and not as a Bundle** (see [Questions](/UG_Lai1iRaC2posiQzl0zw#Questions))
- Patient's can access data (see [FRC-2 and 3]([/WwsA0bNWSQ2OS5zbJFM_rw?view](https://hackmd.io/WwsA0bNWSQ2OS5zbJFM_rw?view)))
- Argo Write rate limits in place as needed

---

#### Sequence Diagram

```plantuml
@startuml
 
Participant PatientApp 
Participant EHR
Participant ProviderClient 
    Note over PatientApp: 1. Patient takes photo and "uploads" to app
    PatientApp-->PatientApp: 2. App creates DocumentReference
    PatientApp->EHR: 2. POST DocumentReference
    EHR->PatientApp: Http response 201

    Opt "EHR can react"
        EHR-->EHR:5. EHR 'Processes Data'
        EHR-->ProviderClient:6. !Notify (not standardized)
    End
    Opt "Clients can search"
        PatientApp -> EHR: 4. GET DocumentReference
        EHR->PatientApp: Http response 200\nresponse body: DocumentReference
    End   
@enduml
```

##### Steps

1. Patient takes photo of wound and "uploads" to app
2. The App creates a FHIR DocumentReference Example resource with a uploaded-data tag and submission key, performer and image data as inline base64 data.
    - See below
3. The patient instructs the connected App to POST the image to her EHR. The app uses the FHIR RESTful API to do this.
4. The patient may decide to fetch her weight data to review and instructs the app to fetch it.  The app uses the FHIR RESTful API to do this.
5. Based on its policy, the EHR 'Processes' the patient submitted wts.  For example it may store, delete summarize or 6. may notify the provider (not standardized)

##### Example resource with uploaded-data tag and submission key, performer and image data as inline base64 data:

{%gist Healthedata1/c8849ec01b692dafd57610a8b74dd139 %}
