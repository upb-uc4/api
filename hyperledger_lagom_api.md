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
- ID = immatriculateStudent
- Send
    - ImmatriculationData
- Receive
    - Done

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