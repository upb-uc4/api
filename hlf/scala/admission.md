# Hyperledger Scala API Admission

In the following, the hlf-api for the "admission" handling will be explained.
For insight in the General hlf-api, please refer to it's .Readme - File.
> [!NOTE]
Any Method can throw these [Exceptions](errors.md#Exceptions).

For "admission" handling we offer the following interface. 
Additionally, as described in [General Communication](general-communication.md), different proposal calls can be made.


## AddAdmission(enrollmentId: String, courseId: String, moduleId: String, timestamp: String)
- Returns
    - Admission :: Json (refer to: [Admission](../chaincode/admission.md#Admission))
        - => Success
    - TransactionError :: Json (refer to: [DetailedError](../chaincode/errors.md#DetailedError))
        - => error is returned
          - If the given parameters could not be parsed.
          - If the given transaction could not be performed due to semantic reasons.

## DropAdmission(admissionId: String)
- Returns
    - "": String
        - => Success
    - TransactionError :: Json (refer to: [GenericError](../chaincode/errors.md#GenericError))
        - => error is returned, 
          - If the admissionId specified is not present on the ledger.
          - If the required approvals are not present on the ledger.
    - TransactionError :: Json (refer to: [DetailedError](../chaincode/errors.md#DetailedError))
        - => error is returned
          - If the given parameters could not be parsed.

## GetAdmissions(enrollmentId: String, courseId: String, moduleId: String)
Gets the full List of existing Admissions.
Applies filters to match enrollmendId, courseId, moduleId.
If any of theses parameter is empty, its filter will not be applied.
- Returns
    - AdmissionsList :: List\<String Json (refer to: [Admission](../chaincode/admission.md#Admission))\> 
        - => Exhaustive List of all Admissions, filtered by
            inputs "enrollmentId", "courseId", "moduleId".
            Empty if no match could be found.

    - TransactionError :: Json (refer to: [GenericError](../chaincode/errors.md#GenericError))
        - => error is returned,
          - If the state of data on the ledger is not consistent with the curent model.
            - This error should only occurr if the model changes while the old ledger state remains without modification.
    - TransactionError :: Json (refer to: [DetailedError](../chaincode/errors.md#DetailedError))
        - => error is returned,
          - The enrollmentId is null or empty
