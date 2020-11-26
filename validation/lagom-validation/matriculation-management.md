# MatriculationData Validation

## SubjectMatriculation

### fieldOfStudy (String)
*validFieldsOfStudies* are names of active examination regulations
- vFieldOfStudy1: fieldOfStudy ∈ *validFieldsOfStudies*
- vFieldOfStudy1: fieldOfStudy ∉ *validFieldsOfStudies*

### semesters (Array[String])
Each semester in the list is validated as follows:  
Valid semesters are of the form "SS2016" and "WS2017/18" (with consecutive years for WS)
- vSemester1: semester is of a valid format for semesters
- iSemester1: semester is the empty String
- iSemester2: semester is not the empty String AND semester is not in a valid format for semesters
