---
title: "Operators"
---

## Binary Operators
These are all the operations supposorted by Yatta's binary operators. Combinations of other data types than listed here will result in a `TypeError`.

| Operator | Left operand | Right operand | Description |
| -------- | ------------ | ------------- | ----------- |
| `&` | integer | integer | bitwise and |
| `&` | set | set | set intersection |
| `&` | dict | dict | dict intersection |
| `|` | integer | integer | bitwise or |
| `|` | set | set | set union |
| `|` | dict | dict | dict union |
| `^` | integer | integer | bitwise xor |
| `^` | set | set | set symetric difference |
| `^` | dict | dict | dict symetric difference |
| `--` | set | any | remove element from a set |
| `--` | dict | any | remove key from a dict |
| `/` | integer | integer | division |
| `/` | double | double | division |
| `==` | integer | integer | equality |
| `==` | double | double | equality |
| `==` | byte | byte | equality |
| `==` | function | function | referential equality |
| `==` | `()` | `()` | equality - alwyays `true` |
| `==` | tuple | tuple | equality |
| `==` | module | module | equality |
| `==` | sequence | sequence | equality |
| `==` | dict | dict | equality |
| `==` | set | set | equality |
| `==` | native | native | equality |
| `>` | integer | integer | greater than |
| `>` | double | double | greater than |
| `>` | byte | byte | greater than |
| `>` | function | function | always `false` |
| `>` | `()` | `()` | greater than - alwyays `false` |
| `>` | dict | dict | left is a strict superset of the right |
| `>` | set | set | left is a strict superset of the right |
| `>=` | integer | integer | greater than or equals |
| `>=` | double | double | greater than or equals |
| `>=` | byte | byte | greater than or equals |
| `>=` | function | function | referential equality |
| `>=` | `()` | `()` | always `true` |
| `>=` | dict | dict | left is a superset of the right |
| `>=` | set | set | left is a superset of the right |
| `!=` | integer | integer | non-equality |
| `!=` | double | double | non-equality |
| `!=` | byte | byte | non-equality |
| `!=` | function | function | referential non-equality |
| `!=` | `()` | `()` | non-equality - alwyays `false` |
| `!=` | tuple | tuple | non-equality |
| `!=` | module | module | non-equality |
| `!=` | sequence | sequence | non-equality |
| `!=` | dict | dict | non-equality |
| `!=` | set | set | non-equality |
| `!=` | native | native | non-equality |
| `<` | integer | integer | lower than |
| `<` | double | double | lower than |
| `<` | byte | byte | lower than |
| `<` | function | function | always `false` |
| `<` | `()` | `()` | lower than - alwyays `false` |
| `<` | dict | dict | left is a strict subset of the right |
| `<` | set | set | left is a strict subset of the right |
| `<=` | integer | integer | lower than or equals |
| `<=` | double | double | lower than or equals |
| `<=` | byte | byte | lower than or equals |
| `<=` | function | function | referential equality |
| `<=` | `()` | `()` | always `true` |
| `<=` | dict | dict | left is a subset of the right |
| `<=` | set | set | left is a subset of the right |
| `in` | any | set | left is a member of the right |
| `in` | any | dict | left is a member of the right |
| `in` | any | seq | left is a member of the right |
| `++` | seq | seq | concatenation |
| `<<` | integer | integer | signed left shift |
| `>>` | integer | integer | signed right shift |
| `>>>` | integer | integer | zero fill right shift |
| `&&` | boolean | boolean | logical and |
| `||` | boolean | boolean | logical or |
| `-` | integer | integer | arithmetic subtraction |
| `-` | float | float | arithmetic subtraction |
| `-` | dict | any | TBD: review |
| `%` | integer | integer | arithmetic modulo |
| `%` | float | float | arithmetic modulo |
| `*` | integer | integer | arithmetic multiplication |
| `*` | float | float | arithmetic multiplication |
| `**` | float | float | first argument raised to the power of the second argument |
| `-` | integer | integer | arithmetic subtraction |
| `-` | float | float | arithmetic subtraction |
| `-` | set | any | remove element from the left |
| `-` | dict | tuple | remove element from the left |
| `+` | integer | integer | arithmetic addition |
| `+` | float | float | arithmetic addition |
| `+` | set | any | add element to the left |
| `+` | dict | tuple | add tuple `(key, value)` to the left |
| `-|` | any | seq | add element to the beginning of a sequence |
| `|-` | seq | any | add element to the end of a sequence |


## Unary operators
In addition to the binary operators in the table above, Yatta supports negation operator `~` which can be applied 
either to integer or boolean values and will result in bitwise complement, or boolean negation respectively.
