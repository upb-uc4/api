# Examination Regulation Validation

##  <a id="Module" /> Module
### id (String)
- vId1: id.length ∈ [1, 20]
- iId1: id is the empty String
- iId2: id.length > 20

### name (String)
- vName1: name.length ∈ [1, 100]
- iName1: name is the empty String
- iName2: name.length > 100

## Examination Regulation

### name (String)
- vName1: name.length ∈ [1, 100]
- iName1: name is the empty String
- iName2: name.length > 100

### active (Boolean)
- vActive1: active = true
- iActive1: active = false

### modules (Array[Modules])
- vModule1: At least one Module exists and all Modules are valid as defined in [Module](#Module)
- iModule1: The Array is empty
- iModule2: At one Module exists that is invalid as in the definition in [Module](#Module)
