# JSON-Criteria

JSON-Criteria is a JSON document describing criteria, a tree of boolean
expressions. These can be used to evaluate against context data.

Implementations provide access to context data via any string. This document
recommends using JSON-Path.

## Operators

### `#equals`

```javascript
let expression = {
    "#equals": {
        "$.person.firstname": "Bob"
    }
}
let context = {
    "person": {
        "firstname": "Alice"
    }
}
JSONCriteria(expression, context) === false
```

```javascript
let expression = {
    "#equals": {
        "$.person.firstname": "alice",
        "#caseIgnore": true
    }
}
let context = {
    "person": {
        "firstname": "Alice"
    }
}
JSONCriteria(expression, context) === false
```

### `#regex`

```javascript
let expression = {
    "#regex": {
        "$.person.firstname": "bob|alice",
        "#caseIgnore": true
    }
}
let context = {
    "person": {
        "firstname": "Alice"
    }
}
JSONCriteria(expression, context) === true
```

### `#and`

Equivalent to (chaining) `and` expressions, for example in JavaScript
`expr && expr && expr`. Each array item must be a valid JSON-Criteria
expression.


```javascript
let expression = {
    "#and": [
        {
            "$.person.firstname": "Alice"
        }
    ]
}
let context = {
    "person": {
        "firstname": "Alice"
    }
}
JSONCriteria(expression, context) === true
```

```javascript
let expression = {
    "#and": [
        {
            "$.person.firstname": "Alice"
        },
        {
            "$.person.lastname": "Example"
        }
    ]
}
let context = {
    "person": {
        "firstname": "Alice",
        "lastname": "Tester",
    }
}
JSONCriteria(expression, context) === false
```

### `#or`

Equivalent to (chaining) `or` expressions, for example in JavaScript
`expr || expr || expr`. Each array item must be a valid JSON-Criteria
expression.

```javascript
let expression = {
    "#or": [
        {
            "$.person.firstname": "Alice"
        }
    ]
}
let context = {
    "person": {
        "firstname": "Alice"
    }
}
JSONCriteria(expression, context) === true
```

```javascript
let expression = {
    "#or": [
        {
            "$.person.firstname": "Alice"
        },
        {
            "$.person.lastname": "Example"
        }
    ]
}
let context = {
    "person": {
        "firstname": "Alice",
        "lastname": "Tester",
    }
}
JSONCriteria(expression, context) === true
```

### `#not`

```javascript
let expression = {
    "#not": true
}
let context = {
    "person": {
        "firstname": "Alice"
    }
}
JSONCriteria(expression, context) === false
```

```javascript
let expression = {
    "#not": false
}
let context = {
    "person": {
        "firstname": "Alice"
    }
}
JSONCriteria(expression, context) === true
```

```javascript
let expression = {
    "#not": {
        "#equals": {
            "$.person.firstname": "Alice"
        }
    }
}
let context = {
    "person": {
        "firstname": "Alice"
    }
}
JSONCriteria(expression, context) === false
```
