As it's impossible to have cases with arguments in enums with raw types:

```
enum FieldId: String {
    case Name = "Name_Field"
    case Phone = "Phone_Field"
    case EMail = "Email_Field"
    
    // Enum with raw type cannot have cases with arguments
    //    case Custom(String)
}
```

It can be implemented with using additional adapter:

```
enum FieldIdBox {
    case SpecifiedFieldId(FieldId)
    case Custom(String)
    
    init(fieldId: String) {
        if let fieldId = FieldId(rawValue: fieldId) {
            self = .SpecifiedFieldId(fieldId)
        }
        else {
            self = .Custom(fieldId)
        }
    }
    
    func rawValue() -> String {
        switch self {
        case .SpecifiedFieldId(let fieldId):
            return fieldId.rawValue
        case .Custom(let fieldIdValue):
            return fieldIdValue
        }
    }
}
```

Sample:

```
let fields = [
    FieldIdBox(fieldId: "Name_Field"),
    FieldIdBox(fieldId: "Phone_Field"),
    FieldIdBox(fieldId: "Email_Field"),
    FieldIdBox(fieldId: "Address_Field"),       // custom field
]

print(fields.map { $0.rawValue() })
```
