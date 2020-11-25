# <a id="Errors" /> Hyperledger Chaincode API Errors

In the following, the error returned by the uc4-chaincode transactions is described.

## <a id="GenericError" />GenericError
```json
{
  "type": "string",
  "title": "string"
}
```

## <a id="DetailedError" />DetailedError
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

## <a id="SemanticError" />SemanticError
```json
{
  "type": "HLInvalid",
  "title": "Semantic errors were found. The following Validation Rules were violated:",
  "validationRules": [
    {
      "name": "string",
      "reason": "string"
    }
  ]
}
```

### <a id="InvalidParameter" />InvalidParameter
```json
{
  "name": "string",
  "reason": "string"
}
```
name:
- name of the invalid parameter in simple dot- and array notation, i.e. if the second semester entry in the ```semesters``` field of the first ```SubjectMatriculation``` object in the ```matriculationStatus``` field of a ```MatriculationData``` object passed as a parameter named ```data``` is invalid, the ```name``` field of the ```InvalidParameter``` object would be ```"data.matriculationStatus[0].semesters[1]"```

reason:
- a textual representation of why the parameter is invalid

### <a id="ValidationRules" />ValidationRules
```json
{
  "name": "string",
  "reason": "string"
}
```
name:
- identifier of the validation rule that has been violated,
    i.e. if a student wants to matriculate into a non existing examinationRegulation, 
    the ```name``` field of the ```ValidationRules``` object would be ```NonExistingExaminationRegulation```

reason:
- a textual representation of how the ValidationRule has been violated.
