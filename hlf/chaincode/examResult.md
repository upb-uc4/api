# Hyperledger ExamResult Api

In the following, the chaincode api for the "Exam"-Chaincode is described.
This contains all Transactions and Models provided for this domain.
The Errors returned are defined [here](errors.md#Errors).

## ContractName "UC4.ExamResult"

## Transactions

### Add Exam Result
- ID = addExamResult
- Required Approvals
  - Users
    - examResult.examId.lecturerEnrollmentId
  - Groups
    - System
- Send
    - examResultEntries :: List\<[ExamResultEntry](#ExamResultEntry)\>
- Receive
    - examResultEntries :: List\<[ExamResultEntry](#ExamResultEntry)\>
      -  Description: Done; returns final object on ledger.

    - [DetailedError](errors.md#DetailedError) 
      ```json
      {
        "type": "HLUnprocessableEntity",
        "title": "The following parameters do not conform to the specified format",
        "invalidParams": [
          {
            "name": "string",
            "reason": "string"
          }
        ]
      }
      ```
       - Description: This error is returned, 
         - if the given parameters could not be parsed. If some attributes within the json are not well formatted, they are listed in invalidParams.  
            For detailed informations see [parameter checks](#parameterChecks).
    
    - [GenericError](errors.md#GenericError) 
      ```json
      {
        "type": "HLInsufficientApprovals",
        "title": "The approvals present on the ledger do not suffice to execute this transaction"
      }
      ```
      - Description: This error is returned, if the required approvals are not present.

### Get Exam Result Entries
- ID = getExamResultEntries
- Send
    - enrollmentId :: String
    - examIds :: List<\String\>
- Receive
    - examList :: List\<[ExamResultEntry](#ExamResultEntry)\>
      - Description: Returns the full list of existing ExamResultEntries matching the filter parameters.
        All filters are applied consecutively (logical AND).
        If these parameters are empty, the exhaustive list of all existing ExamResultEntries is returned.
      - Empty List if none could be found.

    - [DetailedError](errors.md#DetailedError) 
      ```json
      {
        "type": "HLUnprocessableEntity",
        "title": "The following parameters do not conform to the specified format",
        "invalidParams": [
          {
            "name": "string",
            "reason": "string"
          }
        ]
      }
      ```
       - Description: This error is returned, if the given parameters could not be parsed. If some attributes are not well formatted, they are listed in invalidParams.  
       For detailed informations see [Input Checks](#Checks).

## <a id="Models" />Models

### <a id="ExamResultEntry" />ExamResultEntry
```json
{
  "enrollmentId": "0123456",
  "examId": "ExampleGroup:M.1:Written Exam:2021-02-12T10:00:00",
  "grade": "1.0"
}
```
### <a id="ExamResult" />ExamResult
{
"examResultEntries":
  [
    {
      "enrollmentId": "0123456",
      "examId": "ExampleGroup:M.1:Written Exam:2021-02-12T10:00:00",
      "grade": "1.0"
    }
  ]
}
## <a id="Checks" />Input Checks
### <a id="parameterChecks" />Parameters
- Checks, if parseable Json.
- **enrollmentId**
  - Check, if not null or an empty String.
  - Check, if user exists (Certificate exists)
- **examId**
  - Check, if not null or an empty String.
  - Check, if exam exists
- **moduleId**
  - Check, if not null or an empty String.
  - Check, if the exists an ExaminationRegulation which contains the module.
- **grade**
  - AnyOf("1.0", "1.3", "1.7", "2.0", "2.3", "2.7", "3.0", "3.3", "3.7", "4.0", "5.0")
- **examResultEntryList**
  - All entries must refer to the same **examId**
  - All students referenced must be admitted
  - All students admitted must be referenced
- **examIds**
  - Check, if valid jsonList of String.
