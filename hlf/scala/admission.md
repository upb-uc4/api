# Hyperledger Scala API Admission

In the following, the hlf-api for the "admission" handling will be explained.
For insight in the General hlf-api, please refer to it's .Readme - File.
> [!NOTE]
Any Method can throw these [Exceptions](errors.md#Exceptions).

For "admission" handling we offer the following interface. 
Additionally, as described in [General Communication](general-communication.md), different proposal calls can be made.


## AddAdmission(enrollmentId: String, courseId: String, moduleId: String, timestamp: Option[String])
- Returns
    - Admission :: Json (refer to: [Admission](../chaincode/admission.md#Admission))
        - => Success
    - TransactionError :: Json (refer to: [DetailedError](../chaincode/errors.md#DetailedError))
        - => error is returned
          - If the given parameters could not be parsed.
    - TransactionError :: Json (refer to: [SemanticError](../chaincode/errors.md#SemanticError))
        - => error is returned
          - If the given transaction could not be performed. ValidationRules were violated.

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

## GetAdmissionsForUser(enrollmentId: String)
- Returns
    - AdmissionsList :: List\<String Json (refer to: [Admission](../chaincode/admission.md#Admission))\> 
        - => Exhaustive List of all Admissions referencing the enrollmentId. 
             Empty if none exist.
    - TransactionError :: Json (refer to: [GenericError](../chaincode/errors.md#GenericError))
        - => error is returned,
          - If the state of data on the ledger is not consistent with the curent model.
            - This error should only occurr if the model changes while the old ledger state remains without modification.
    - TransactionError :: Json (refer to: [DetailedError](../chaincode/errors.md#DetailedError))
        - => error is returned,
          - The enrollmentId is null or empty

## GetAdmissionsForCourse(courseId: String)
- Returns
    - AdmissionsList :: List\<String Json (refer to: [Admission](../chaincode/admission.md#Admission))\> 
        - => Exhaustive List of all Admissions referencing the course. 
             Empty if none exist.
    - TransactionError :: Json (refer to: [GenericError](../chaincode/errors.md#GenericError))
        - => error is returned,
          - If the state of data on the ledger is not consistent with the curent model.
            - This error should only occurr if the model changes while the old ledger state remains without modification.
    - TransactionError :: Json (refer to: [DetailedError](../chaincode/errors.md#DetailedError))
        - => error is returned,
          - The courseId is null or empty


## GetAdmissionForModule(moduleId: String)
- Returns
    - AdmissionsList :: List\<String Json (refer to: [Admission](../chaincode/admission.md#Admission))\> 
        - => Exhaustive List of all Admissions referencing the module. 
             Empty if none exist.
    - TransactionError :: Json (refer to: [GenericError](../chaincode/errors.md#GenericError))
        - => error is returned,
          - If the state of data on the ledger is not consistent with the curent model.
            - This error should only occurr if the model changes while the old ledger state remains without modification.
    - TransactionError :: Json (refer to: [DetailedError](../chaincode/errors.md#DetailedError))
        - => error is returned,
          - The moduleId is null or empty