# dataweave-queries
- The best repo of dataweave queries ever and ever.
> Stackoverflow the hottest about Dataweave:
> https://stackoverflow.com/tags/dataweave/hot?filter=year

<br />

## Map | Mapping | FromTo
* https://docs.mulesoft.com/mule-runtime/4.3/dw-core-functions-map#map1

- Structure:
```
map(Array<T>, (item: T, index: Number) -> R): Array<R>
```

- Example:
``` java
["victor", "jose", "pedro"] map (value, index) -> { (index) : value}
```

- Result:
``` json
[ { "0": "victor" }, { "1": "jose" }, { "2": "pedro" } ]
```

- Other example:
``` java
['banana', 'apple', 'orange'] map ( "$$" : $)
```
- Other result:
``` json
[{ "0": "banana" }, { "1": "apple" }, { "2": "orange" }]  
```

<br />

## Interpolation of strings | Interpolate
* https://stackoverflow.com/questions/64991150/assemble-string-in-dataweave-2-x-efficiently/64991736#64991736

- Structure:
```
"$variableString"
```

- Example:
``` java
%dw 2.0
output text/plain

import repeat from dw::core::Strings
var msg = "Read the Release Notes!"

fun banner(in) =
    do {
        var width = sizeOf(in) + 4
        var standout = repeat("*", width)
        ---
        "$(standout)\n* $(in) *\n$(standout)"       
    }
---
banner(msg)
```

<br />

## Conditions | Conditionals
* https://docs.mulesoft.com/mule-runtime/4.3/dw-operators#scope-and-flow-control-operators

- Structure:
``` 
if (condition) (expression return)
else if (condition) (expression return)
else (expression return)
```

- Example:
``` java
if (fruit == "banana") (1.23)
else if (fruit == "apple") (2.31)
else (1.00)
```

- Result:
``` json
1.23
```

<br />

## Update Field
* https://docs.mulesoft.com/mule-runtime/4.3/dw-operators#update-operator

- Structure:
```
<value_to_update> update {
    case <variable_name> at <update_expression>[!]? [if(<conditional_expression>)]? -> <new value>
    ...
}
```

- Example:
``` java
payload update {
    case age at .age -> age + 1
}
```

- Other example:
``` java
payload map ((user) ->
    user update {
        case name at .name if(name == "Ken") -> name ++ " (Victor)"
        case name at .name if(name == "John") -> name ++ " (Me)"
    }
)
```

- Another best example:
``` java
payload  map ((user) ->
    user update {
        case name at .name! -> if(name == null) "JOHN" else upper(name)
    }
)
```