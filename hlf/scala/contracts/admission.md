# Hyperledger Scala API Admission

In the following, the hlf-api for the "admission" handling will be explained.
For insight in the General hlf-api, please refer to it's .Readme - File.
> [!NOTE]
Any Method can throw these [Exceptions](../errors.md#Exceptions).

For "admission" handling we offer the following interface. 
Additionally, as described in [General Communication](../general-communication.md), different proposal calls can be made.


## AddAdmission(admission: Json([Admission](../../chaincode/contracts/admission.md#AbstractAdmission)))
- Returns
    - Admission :: Json (refer to: [Admission](../../chaincode/contracts/admission.md#AbstractAdmission))
        - => Success
    - TransactionError :: Json (refer to: [DetailedError](../../chaincode/errors.md#DetailedError))
        - => error is returned
          - If the given parameters could not be parsed.
          - If the given transaction could not be performed due to semantic reasons.
    - TransactionError :: Json (refer to: [GenericError](../../chaincode/errors.md#GenericError))
        - => error is returned, 
          - If the required approvals are not present on the ledger.

## DropAdmission(admissionId: String)
- Returns
    - "": String
        - => Success
    - TransactionError :: Json (refer to: [GenericError](../../chaincode/errors.md#GenericError))
        - => error is returned, 
          - If the admissionId specified is not present on the ledger.
          - If the required approvals are not present on the ledger.
    - TransactionError :: Json (refer to: [DetailedError](../../chaincode/errors.md#DetailedError))
        - => error is returned
          - If the given parameters could not be parsed.

## GetCourseAdmissions(enrollmentId: String, courseId: String, moduleId: String)
Gets the full List of existing Admissions.
Applies filters to match enrollmendId, courseId, moduleId.
If any of theses parameter is empty, its filter will not be applied.
- Returns
    - AdmissionsList :: List\<String Json (refer to: [Admission](../../chaincode/contracts/admission.md#Admission))\> 
        - => Exhaustive List of all Admissions, filtered by
            inputs "enrollmentId", "courseId", "moduleId".
            Empty if no match could be found.

## GetExamAdmissions(admissionIds: List\<String\>, enrollmentId: String, examIds: List\<String\>)
Gets the full List of existing Admissions.
Applies filters to match admissionIds, enrollmentId, examIds.
If any of theses parameter is empty, its filter will not be applied.
- Returns
    - AdmissionsList :: List\<String Json (refer to: [Admission](../../chaincode/contracts/admission.md#Admission))\> 
        - => Exhaustive List of all Admissions, filtered by
            inputs "admissionIds", "enrollmentId", "examIds".
            Empty if no match could be found.

> Note! Invalid States on the ledger get ignored.
> We return all valid Admissions that match the filters.
