# Hyperledger Scala API Exam

In the following, the hlf-api for the "exam" handling will be explained.
For insight in the General hlf-api, please refer to it's .Readme - File.
> [!NOTE]
Any Method can throw these [Exceptions](../errors.md#Exceptions).

For "exam" handling we offer the following interface. 
Additionally, as described in [General Communication](../general-communication.md), different proposal calls can be made.


## AddExam(examJson: String)
- Returns
    Exam :: Json (refer to: [Exam](../../chaincode/contracts/exam.md#Exam))
        - => Success
    - TransactionError :: Json (refer to: [DetailedError](../../chaincode/errors.md#DetailedError))
        - => error is returned
          - If the given parameters could not be parsed.
          - If the given transaction could not be performed due to semantic reasons.
    - TransactionError :: Json (refer to: [GenericError](../../chaincode/errors.md#GenericError))
        - => error is returned, 
          - If the required approvals are not present on the ledger.
  
## GetExams(examIds: List\<String\>, courseIds: List<\String\>, lecturerIds: List<\String\>, moduleIds: List<\String\>, types: List<\String\>, admittableAt: String, droppableAt: String)
Gets the full List of existing Exams.
Applies filters to match examIds, courseIds, lecturerIds, moduleIds, types, admittableAt, droppableAt.
If any of theses parameter is empty, its filter will not be applied.
- Returns
    - ExamsList :: List\<String Json (refer to: [Exam](../../chaincode/contracts/exam.md#Exam))\> 
        - =>    Exhaustive List of all Exam, filtered by the parameters.
                All filters are applied consecutively (logical AND).
                Each single List\<String\> filter is treated as a logical OR.
                Empty if no match could be found.

> Note! Invalid States on the ledger get ignored.
> We return all valid Exams that match the filters.
