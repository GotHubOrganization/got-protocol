# got

## Introduction

The got protocol is a description language of typed objects which can be rendered automatically by any component based frontend technology. Documents written in got are usually populated and returned by an API that computes which linked subtype descriptions are needed for rendering a requested view. This protocol is under development so feel free to make suggestions and create pull requests.

## Types

Types have a name and properties. 

```json
[{

    "name": "Person",
    "properties": []
}]
```

Properties are defined by a type, the accessability (read/write), a view component as a key and its validators. Primitively typed properties have simple view components like `"text"` for type `string` or `"date"` for type `datetime` whereas properties with complex types (e.g. `BestFriend`) have a complex type as a view component which renders other primitve or complex properties itself. The standard view for a complex property is the actual type of the property (e.g. `Person` for type `Person`).

```json
"properties": [
    {
        "name": "Firstname",
        "type": "string",
        "view": "text",
        "access": "rw"
    },
    {
        "name": "Lastname",
        "type": "string",
        "view": "text",
        "access": "rw",
        "validators": [
            "required",
            {
                "minLength": 5
            }
        ]
    },
    {
        "name": "BestFriend",
        "type": "Person",
        "access": "r"
    }
]
```

However you can use any other type as a view component for a complex property and map its properties to the actual default type. Note that the view contains a complete type object itself. The only thing that is different is the `"map"` key that holds a mapping reference of the underlying type, that should be presented by this "view" type.

```json
"properties": [
    {
        "name": "BestFriend",
        "type": "Person",
        "view": {
            "name": "PersonView",
            "properties": [
                {
                    "name": "Lastname",
                    "type": "string",
                    "view": "text",
                    "map": "Person.Lastname",
                    "validators": [
                        "required",
                        {
                            "minLength": 5
                        }
                    ]
                }
            ]
        },
        "access": "r"
    }
]
```

With this in mind there is only one entity that you have to model which is the `Type` and you can use types as data model or as a view model which is shown to a user.