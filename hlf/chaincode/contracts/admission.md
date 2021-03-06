# Hyperledger Admission Api

In the following, the chaincode api for the "Admission"-Chaincode is described.
This contains all Transactions and Models provided for this domain.
The Errors returned are defined [here](../errors.md#Errors).

## ContractName "UC4.Admission"

## Transactions

### Add Admission
- ID = addAdmission
- Required Approvals
  - Users
    - jsonAdmission.enrollmentId
  - Groups
    - System
- Send
    - jsonAdmission :: [AbstractAdmission](#AbstractAdmission)
- Receive
    - Subclass of [AbstractAdmission](#AbstractAdmission)
      -  Description: Done, returns the submitted data, decorated with an admissionId.

    - [DetailedError](../errors.md#DetailedError) 
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
         - if the given parameters produce some [semantic error](#semanticChecks).
    
    - additionally all [operation errors](../errors.md#AllOperations) can be thrown


### Drop Admission
- ID = dropAdmission
- Required Approvals
  - Users
    - related enrollmentId
  - Groups
    - System
- Send
    - admissionId :: String
- Receive
    - ""
      -  Description: Done, returns the submitted data.

    - [GenericError](../errors.md#GenericError) 
      ```json
      {
        "type": "HLNotFound",
        "title": "There is no Admission for the given admissionId"
      }
      ```
      - Description: This error is returned, if the admissionId is not present on the ledger.

    - [DetailedError](../errors.md#DetailedError) 
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
      - Description: This error is returned, if the given parameters are not well formatted, they are listed in invalidParams.  
      For detailed informations see [parameter checks](#parameterChecks).
      (admissionId)
    
    - additionally all [operation errors](../errors.md#AllOperations) can be thrown



### Get Course Admissions
- ID = getCourseAdmissions
- Send
    - enrollmentId :: String
    - courseId :: String
    - moduleId :: String
- Receive
    - admissionList :: List\<[Admission](#Admission)\>
      - Description: Exhaustive List of all Admissions, filtered by
      inputs "enrollmentId", "courseId", "moduleId".
      Empty if no match could be found.

    - [GenericError](../errors.md#GenericError) 
      ```json
      {
        "type": "HLUnprocessableLedgerState",
        "title": "The state on the ledger does not conform to the specified format"
      }
      ```
      - Description: This error is returned, if the state of data on the ledger is not consistent with the curent model. This error should only occurr if the model changes while the old ledger state remains without modification.

    - [DetailedError](../errors.md#DetailedError) 
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
      - Description: This error is returned, if the given parameters are not well formatted, they are listed in invalidParams.  
      For detailed informations see [parameter checks](#parameterChecks).

### Get Exam Admissions
- ID = getExamAdmissions
- Send
    - admissionIds :: List\<String\>
    - enrollmentId :: String
    - examIds :: List\<String\>
- Receive
    - admissionList :: List\<[Admission](#Admission)\>
      - Description: Exhaustive List of all Admissions, filtered by
      inputs "admissionIds", "enrollmentId", "examIds".
      Empty if no match could be found.

    - [DetailedError](../errors.md#DetailedError) 
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
      - Description: This error is returned, if the given parameters are not well formatted, they are listed in invalidParams.  
      For detailed informations see [parameter checks](#parameterChecks).

## <a id="Models" />Models

### <a id="Admission" />AbstractAdmission
```json
{
  "admissionId": "0123456:ExampleCourse",
  "enrollmentId": "0123456",
  "timestamp": "2004-06-14T23:34:30.123Z",
  "type": "Course"
}
```
with date conforming to regular datetime format specified in [Date & Time](./general.md#Date&Time)

### <a id="CourseAdmission" />CourseAdmission
```json
{
  "admissionId": "0123456:ExampleCourse",
  "enrollmentId": "0123456",
  "courseId": "ExampleCourse",
  "moduleId": "M.1",
  "timestamp": "2004-06-14T23:34:30.123Z",
  "type": "Course"
}
```
with timestamp conforming to regular datetime format specified in [Date & Time](./general.md#Date&Time)


### <a id="ExamAdmission" />ExamAdmission
```json
{
  "admissionId": "0123456:ExampleCourse:M.1:WrittenExam:2021-02-16T10:00:00.123Z",
  "enrollmentId": "0123456",
  "examId": "ExampleCourse:M.1:WrittenExam:2021-02-16T10:00:00.123Z",
  "timestamp": "2004-06-14T23:34:30.123Z",
  "type": "Exam"
}
```
with timestamp conforming to regular datetime format specified in [Date & Time](./general.md#Date&Time)

## <a id="Checks" />Input Checks
### <a id="parameterChecks" />Parameters
- Checks, if parseable Json.
- **admissionId**
  - Check, if not null or an empty String.
- **enrollmentId**
  - Check, if not null or an empty String.
- **courseId**
  - Check, if not null or an empty String.
- **moduleId**
  - Check, if not null or an empty String.
- **examId**
  - Check, if not null or an empty String.
- **type**
  - Check, if one of valid Inputs ("Course", "Exam")
- **admissionIds**
  - Check, if valid jsonList of String.
- **examIds**
  - Check, if valid jsonList of String.

### <a id="semanticChecks" />SemanticChecks

#### CourseAdmission: 

- **addAdmission**
  - Check, if **enrollmentId** is matriculated in an examinationRegulation containing the **moduleId**.

#### ExamAdmission: 

- **addAdmission**
  - Check, if **enrollmentId** is matriculated in an examinationRegulation containing the **moduleId**, referenced in the exam behind the examId.
  - Check, if **enrollmentId** is not admitted to the same exam already.
  - Check, if **enrollmentId** is admitted to the **couseId**, referenced in the exam behind the examId.
  - Check, if the **examId** is currently admittable
  - Check, if the **timestamp** is not too old (max 60 seconds from now)

- **dropAdmission**
  - Check, if the **examId** is currently droppable
  - Check, if **enrollmentId** is currently admitted to **examId** 
