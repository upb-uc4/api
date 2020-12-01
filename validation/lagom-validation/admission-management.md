# CourseAdmission Validation

## CourseAdmission

### enrollmentId (String)
- vEnrollmentId1: enrollmentId is the empty String
- iEnrollmentId1: enrollmentId is not the empty String

### courseId (String)
- vCourseId1: courseId.length ∈ [1, 10000] and courseId exists in a course, with moduleId ∈ course.moduleIds
- iCourseId1: courseId is the empty String
- iCourseId2: courseId.length > 10000
- iCourseId3: courseId does not exist
- iCourseId4: moduleId ∉ course.moduleIds

### moduleId (String)
- vModuleId1: moduleId.length ∈ [1, 10000] and moduleId exists in an examination regulation, with moduleId ∈ course.moduleIds
- iModuleId1: moduleId is the empty String
- iModuleId2: moduleId.length > 10000
- iModuleId3: moduleId does not exist
- iModuleId4: moduleId ∉ course.moduleIds

### admissionId (String)
- vAdmissionId1: admissionId is the empty String
- iAdmissionId1: admissionId is not the empty String

### timestamp (String)
- vTimestamp1: timestamp is the empty String
- iTimestamp1: timestamp is not the empty String

## DropAdmission

### admissionId (String)
- vAdmissionId1: admissionId.length ∈ [1, 10000]
- iAdmissionId1: admissionId is the empty String
- iAdmissionId2: admissionId.length > 10000