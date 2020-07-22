# Hyperledger Lagom Api

## Transactions

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

### Immatriculate student
- ID = addStudent
- Send
    - ImmatriculationData
- Receive
    - ""
      - Description: Done.
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
        "title": "There is already a student for the given immatriculationId.",
        "invalidParams": []
      }
      ```
    - Description: This error is returned, if a student with the given immatriculationId is already present on the ledger.

### Update immatriculated student
- ID = updateStudent
- Send
    - ImmatriculationData
- Receive
    - Done

### Get immatriculated student
- ID = getStudent
- Send
    - immatriculationId
- Receive
    - ImmatriculationData

### Delete immatriculated student
- ID = deleteStudent
- Send
    - immatriculationId
- Receive
    - Done

### Get immatriculation certificate
- ID = getImmatriculationCertificate
- Send
    - matriculationId
    - semester
- Receive
    - *signed transaction*

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

### ImmatriculationData
```json
{
  "matriculationId": "string",
  "firstName": "string",
  "lastName": "string",
  "birthDate": "2020-07-21",
  "immatriculationStatus": [
    {
      "fieldOfStudy": "Computer Science",
      "intervals": [
        {
          "firstSemester": "WS2018",
          "lastSemester": "SS2020"
        }
      ]
    }
  ]
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