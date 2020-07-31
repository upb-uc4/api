# Hyperledger Course Api

## Transactions

In the following, the chaincode api is described. It is identical to the hyperledger-lagom api, except for th following points:
- If the chaincode api returns "", meaning Done, to lagom Unit is returned
- Each of the specified error json objects is sent to lagom in an exception, with a string parameter containing the json string
- Lagom can receive an error of the form:
  ```json
  {
    "type": "hl: unknown transactionId",
    "title": "The transaction is not defined."
  }
  ```
  - Description: This error is returned, if the transactionId does not exist.
  ```json
  {
    "type": "hl: invalid transaction call",
    "title": "The transaction was invoked with the wrong amount of parameters. Expected: ${expected} Actual: ${actual}",
    "transactionId": "addCourse"
  }
  ```
  - Description: This error is returned, if the transactionId does not exist.

  

### Get all courses
- ID = getAllCourses
- Receive
    - Sequence of Courses

### Add course
- ID = addCourse
- Send
    - Course
- Receive
    - Done
    
    - ```json
      {
        "type": "Unprocessable Entity",
        "title": "The given string does not conform to the specified json format.",
        "invalidParams": [
          {
            "name": "string",
            "reason": "string"
          }
        ]
      }
      ```
      - Description: This error is returned, if the given parameters could not be parsed. If the string could not be parsed as json, the list of invalidParams is empty. If some attributes within the json are not well formatted, they are listed in invalidParams.
  - ```json
      {
        "type": "Conflict",
        "title": "There is already a student for the given matriculationId.",
        "invalidParams": []
      }
      ```
    - Description: This error is returned, if a course with the same courseId is already present on the ledger.
### Get All Courses 
- ID = getAllCourses
- Send
    -  optional
        - courseName
        - lecturerId
-Receive
    - Course List (according to parameters)
    
### Find Course by course ID
- ID = findCourseByCourseId
- Send
    - courseId
- Receive
    - Course
    
    - ```json
        {
          "type": "Not Found",
          "title": "The given ID does not fit any existing course.",
          "invalidParams": [ ]
        }
      ```
### Delete a course
- ID = deleteCourseById
- Send
    - courseId
- Receive
    - ""
    - ```json
        {
          "type": "Not Found",
          "title": "The given ID does not fit any existing course.",
          "invalidParams": []
        }
      ```
### Update an existing course
- ID = updateExistingCourse
- Send 
    - ID
    - Course Object
- Receive 
    - ""
    - ```json
         {
           "type": "Not Found",
           "title": "The given ID does not fit any existing course.",
           "invalidParams": []
         }
      ```
   - ```json
        {
          "type": "Unprocessable Entity",
          "title": "The given string does not conform to the specified json format.",
          "invalidParams": [
          {
            "name": "string",
            "reason": "string"
          }
          ]
        }
      ```   
      - Description: This error is returned, if the given parameters could not be parsed. If the string could not be parsed as json, the list of invalidParams is empty. If some attributes within the json are not well formatted, they are listed in invalidParams.

## Models

### Course
```json
{
  "courseId": "string",
  "courseName": "string",
  "courseType": "Lecture",
  "startDate": "2020-07-21",
  "endDate": "2020-07-21",
  "ects": 0,
  "lecturerId": "string",
  "maxParticipants": 0,
  "currentParticipants": 0,
  "courseLanguage": "German",
  "courseDescription": "string"
}
```

### GenericError
```json
{
  "type": "string",
  "title": "string"
}
```

### DetailedError
```json
{
  "type": "string",
  "title": "string",
  "invalidParams": [
    {
      "name": "string",
      "reason": "string"
    }
  ]
}
```
