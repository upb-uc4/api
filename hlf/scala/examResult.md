# Hyperledger Scala API ExamResult

In the following, the hlf-api for the "examResult" handling will be explained.
For insight in the General hlf-api, please refer to it's .Readme - File.
> [!NOTE]
Any Method can throw these [Exceptions](errors.md#Exceptions).

For "examResult" handling we offer the following interface. 
Additionally, as described in [General Communication](general-communication.md), different proposal calls can be made.


## AddExamResult(examResultJson: String)
- Returns
    ExamResultJson :: Json (refer to: [ExamResultEntry](../chaincode/examResult.md#ExamResultEntry))
        - => Success
    - TransactionError :: Json (refer to: [DetailedError](../chaincode/errors.md#DetailedError))
        - => error is returned
          - If the given parameters could not be parsed.
          - If the given transaction could not be performed due to semantic reasons.
    - TransactionError :: Json (refer to: [GenericError](../chaincode/errors.md#GenericError))
        - => error is returned, 
          - If the required approvals are not present on the ledger.
  
## GetExamResultEntries(enrollmentId: String, examIds: List<\String\>)
Gets the full List of existing Exams.
Applies filters to match enrollmentId, examIds.
If any of theses parameter is empty, its filter will not be applied.
- Returns
    - ExamResultList :: List\<String Json (refer to: [ExamResultEntry](../chaincode/examResult.md#ExamResultEntry))\> 
        - =>    Exhaustive List of all ExamResultEntries, filtered by the parameters.
                All filters are applied consecutively (logical AND).
                Empty if no match could be found.

> Note! Invalid States on the ledger get ignored.
> We return all valid ExamResultEntries that match the filters.