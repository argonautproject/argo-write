---
tags: argo-write
title: Argo Write Direct Write API
---

<!-- Enter your content here -->

the Argo Write API follows the [FHIR RESTful *creates* and *reads*](http://hl7.org/fhir/http.html) as described in the FHIR specification.  <!--The pros and cons of this approach are summarized [here](http://hl7.org/fhir/workflow-ad-hoc.html#optiona)-->


### Create



```plantuml
@startuml

    App->>EHR: POST Observation
    alt Accept
    EHR-->>App: Http 201 Created +/- OperationOutcome
    else Reject due to invalid or absent authorization
    EHR-->>App: Http 401 Unauthorized +/- OperationOutcome
    else Reject due to insufficient scopes 
    EHR-->>App: Http 403 Forbidded +/- OperationOutcome 

    else Reject because EHR does not accept writes (Policy)
    EHR-->>App: Http 422 UnProcessable Entity +/- OperationOutcome 
    end
@end
```

<!--
Request:
~~~
POST [base]/Observation
{"resourceType": "Observation",
....
}
~~~
Response:
~~~
HTTP/1.1 201 Created
...
{"resourceType": "OperationOutcome",
....
}
~~~
-->

### Read / Search




```plantuml
@startuml

    App->>EHR: GET Observation/<fhir_id>
        EHR-->>App: Http response + Observation/<fhir_id>

@end
```

<!--
Request:
~~~
GET [base]/Observation/foo
~~~
Response:
~~~
HTTP/1.1 200 Success
...
{"resourceType": "Observation",
....
}
~~~
-->
