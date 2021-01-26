# Hyperledger Exam Api

In the following, the chaincode api for the "Exam"-Chaincode is described.
This contains all Transactions and Models provided for this domain.
The Errors returned are defined [here](errors.md#Errors).

## ContractName "UC4.Exam"

## Transactions

### Add Exam
- ID = addExam
- Required Approvals
  - Users
    - exam.lecturerEnrollmentId
  - Groups
    - Admin
    - System
- Send
    - [Exam](#Exam)
- Receive
    - [Exam](#Exam)
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

### Get Exams
- ID = getExams
- Send
    - examIds :: List\<String\>
    - courseIds :: List<\String\>
    - lecturerIds :: List<\String\>
    - moduleIds :: List<\String\>
    - types :: List<\String\>
    - admittableAt :: String (ISO DATE)
    - droppableAt :: String (ISO DATE)
- Receive
    - examList :: List\<[Exam](#Exam)\>
      - Description: Returns the full list of existing Exams matching the filter parameters.
        All filters are applied consecutively (logical AND).
        If these parameters are empty, the exhaustive list of all existing Exams is returned.
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

### <a id="Exam" />Exam
```json
{
  "examId": "ExampleGroup:M.1:Written Exam:2021-02-12T10:00:00",
  "courseId": "ExampleCourse"
  "lecturerEnrollmentId": "0123456"
  "moduleId": "M.1"
  "type": "Written Exam"
  "date": "2021-02-12T10:00:00" \<DATE ISO 8601 YYYY-MM-DDThh:mm:ss\>
  "ects": 6
  "admittableUntil": "2021-01-12T23:59:59" \<DATE ISO 8601 YYYY-MM-DDThh:mm:ss\>
  "droppableUntil": "2021-02-05T23:59:59" \<DATE ISO 8601 YYYY-MM-DDThh:mm:ss\>
}
```

## <a id="Checks" />Input Checks
### <a id="parameterChecks" />Parameters
- Checks, if parseable Json.
- **courseId**
  - Check, if not null or an empty String.
- **lecturerEnrollmentId**
  - Check, if not null or an empty String.
  - Check, if user exists (Certificate exists)
- **moduleId**
  - Check, if not null or an empty String.
  - Check, if the exists an ExaminationRegulation which contains the module.
- **type**
  - AnyOf("Written Exam")
- **date**
  - Check, if valid date-String YYYY-MM-DDThh:mm:ss
  - Check, if date lies in the future.
- **ects**
  - Check, if format is integer and value not negative
- **admittableUntil**
  - Check, if valid date-String YYYY-MM-DDThh:mm:ss
  - Check, if date lies in the future.
  - Check, if date lies in before **date**.
  - Check, if date lies in before **droppableUntil**.
- **droppableUntil**
  - Check, if valid date-String YYYY-MM-DDThh:mm:ss
  - Check, if date lies in the future.
  - Check, if date lies in before **date**.
  - Check, if date lies in after **admittableUntil**.
- **examIds**
  - Check, if valid jsonList of String.
- **courseIds**
  - Check, if valid jsonList of String.
- **lecturerIds**
  - Check, if valid jsonList of String.
- **moduleIds**
  - Check, if valid jsonList of String.
- **types**
  - Check, if valid jsonList of String.
- **admittableAt**
  - Check, if valid date-String YYYY-MM-DDThh:mm:ss
- **droppableAt**
  - Check, if valid date-String YYYY-MM-DDThh:mm:ss