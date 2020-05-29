---
title: "Syntax"
---

Program in Yatta consists always of evaluation of a single expression. In fact, any Yatta program consist of exactly one expression.

Syntax is intentionally very minimalistic and inspired in languages such as SML or Haskell. There is only a handful of keywords, however, it is not as flexible in naming as Haskell for example.

Yatta programs have ambition to be easily readable and custom operators with names consisting of symbols alone are not that useful when reading programs for the first time. Therefore Yatta does not support custom operators named by symbols only. Supported operators are listed [here]({{< ref "operators" >}}).

Yatta does not have indentation specific parsing rules, it does require new line at certain locations, which are noted in each individual expression descriptions.

Comments are denoted by `#` character and everything that follows this character until the end of line `\n` or `\r\n` is considered a comment and ignored.

Source code of Yatta program must be a valid UTF-8 text file.

## **Functions**: definition and application
Functions in Yatta are defined in a very short and concise form. They make take arguments, which function can pattern match on, and one function can be defined using multiple such arguments. Function names must start with a lowercase letter, followed by any letters, numbers or underscores.

A simple function to calculate factorial can be written for example this way:

```haskell
factorial 1 = 1
factorial n = n * factorial (n - 1)
```

Each function case must be on a new line. A more complex conditions in patterns can be specified in this way:

```haskell
factorial 1 = 1
factorial n 
  | n > 1 = n * factorial (n - 1)
```
Which means that there is an additional condition for the `n` value to be greater than 0. There may be multiple guards for each pattern and they must be each be on a new line. Guard starts with a `|` character, follows an expression that must evaluate to a boolean and finally an `=` and the expression to evaluate if the pattern matches and the guard is `true`.

Note that function arguments may actually be full patterns, not just names of arguments. Patterns are described in the section named *Pattern Matching*.

Yatta additionally supports non-linear patterns, meaning that if a pattern contains the same name multiple times, than this name is required to match the same value so that the pattern would match. This can be handy when checking for one value to be present in multiple places/arguments without having to explicitly write a guard that would ensure the equality.

### **Anonymous functions**: aka lambdas
Since functions are first-class citizens in Yatta, it is necessary to provide means of passing functions as arguments, and also to define them without giving them a name. Following syntax is used in this case:

```bash
\argument_one argument_two -> argument_one argument_two  # lambda function for summing its arguments
```

Lambda function with no arguments is simply: `\-> :something`.

### **Function application**: calling functions
Calling function simply means writing a name of the function and then specifying its arguments. If fewer arguments are provided than function expects, this is considered a [curried](https://en.wikipedia.org/wiki/Currying) call, and the result of such function is a partially applied function, that can be called with remaining arguments at a later point.

So for example calling a `factorial` would look simply like this:

```haskell
factorial 5
```

Since Yatta is strictly evaluated language, meaning that arguments are evaluated before calling the function, as opposed to lazy language, where arguments are only evaluated when actually used by the called function, there is one situation to be careful about and that is passing lambda functions of zero arguments as arguments to functions. This would be evaluated before actually calling the function and if there are no side-effects happening in the lambda, it might be perfectly fine. However, if the lambda must be passed as a lambda, it needs to be wrapped into another lambda at the call-site, such as for example:

```bash
let
    lambda = \-> println :hello
in
    do_something_with \-> lambda  # instead of do_something_with lambda
```

Then `do_something_with` function will obtain its argument that is a function and not a result of `println :hello` function (that would be `:hello` btw).

### Pipes and operator precedence
Since Yatta is an ML-style language, and unlike many C-like languages it does not use parentheses to denote a function application, it can become unclear which expressions are arguments of which function. Take for example the following example:

```haskell
Seq::take 5 Seq::random 10
```

Is this a function call to the `Seq::take` function (that is a function `take` in the module `Seq` as explained later) with 3 arguments (5, `Seq::random` and 10) or is it `Seq::random 10` supposed to be computed first and then its result used as a second argument to `Seq::take`?

If the latter, then it can be written this way:

```haskell
Seq::take 5 (Seq::random 10)
```

Since any expression can be put into parentheses and be given precendece in evaluation. Another way to write this would be using *pipes*:

```haskell
Seq::take 5 <| Seq::random 10
```

or

```haskell
Seq::random 10 |> Seq::take 5
```

These are all equivalent expression and it is up to the programmers preference to decide which one to use. One nice feature that pipes have is that they can be used on multiple lines, such as:

```haskell
Seq::random 10
|> Seq::take 5
|> println
```

## **`let` expression**: defining local aliases / pattern matching in the scope of evaluated expression {#let-expression}
`let` expression allows defining aliases in the scope of the executed expressions. This expressions allows evaluating patterns as well, so it is possible to deconstruct a value from a sequence, tuple, set, dictionary or a record directly, for example:

```haskell
let
    (1, second) = (1, 2)
    pattern     = expression1
in
    expression2
```

As shown in this example, `let` expression consists of two parts. First is used for definition of aliases and patterns using and the second one which contains the expression that is evaluated with these aliases on the stack. The result of this `let` expression is the result of the `expression2` expression. The `let` expression allows defining patterns which are on the left side of the first section. If a pattern is not matched, the whole expression throws a `:nomatch` exception. One alias line can use names defined in previous lines.
Every alias/pattern must be defined on a new line.

Note that the order of execution of the alias/pattern lines is not strictly sequential. Considering the example in the [documentation homepage]({{< ref "docs#example" >}}), some aliases can be executed as a single batch, in parallel, provided that two conditions are met:
* the do not depend on each other (do not use names provided by other aliases in the same batch)
* they return runtime Promise

When both conditions are met, Yatta can safely execute them conurrently, speeding up the program execution.

## **`do` expression**: sequencing side effects {#do-expression}
`do` expressions is used for definition of a sequence of side effecting expressions. This expression is pretty similar to the `let` expression in the sense that it allows defining aliases and patterns, however, it doesn't have a separate expression that would be used as a result of this expression. Instead the result of the last line is used as the result.

```haskell
do
    start_time  = Time::now
    (:ok, line) = File::read_line f
    end_time    = Time::now
    printf line
    printf (end_time - start_time)
end
```

Note that unlike with `let` expression, the order of executed expressions is guaranteed to be exactly the same as the order in which they are written. This is the main usecase of the `do` expression, to allow strict ordering of execution. It does not matter whether an expression returns a run-time level promise (such as one can be created by a IO call or by using the `async` function), the order is still maintained.

## **`case` expression**: pattern matching
`case` expression is used for pattern matching on an expression. Each line of this expression contains a pattern followed by an arrow `->` and an expression that is evaulated in case of a successful pattern match. Patterns are tried in the order in which they are specified. The default case can be denoted by an underscore `_` pattern that always evaluates as true.

```haskell
case File::read_line f of
    (:ok, line)       -> line
    (:ok, :eof)       -> :eof
    err@(:error, _)   -> err
    tuple   # guard expressions below
        | tuple_size tuple == 3 -> :ok
        | true                  -> :ok
    _                 -> (:error, :unknown)
end
```

Same as with function definition patterns, patterns in the `case` expression can contain guard expressions, as is the case in the *tuple* pattern in the example above. Pattern with a guard expression will match only a guard evaluated to `true` can be found. Guards are tried in the order they are specified in.

## **`if` expression**: conditions
`if` is a conditional expression that takes form:

```haskell
if expression
then
    expression
else
    expression
```

Both `then` and `else` parts must be defined. The expression after `if` must evaluate to a boolean value. Empty sequence, dictionary or similar will not work! New lines are allowed, but not required.

## **module expression**
`module` is an expression representing a set of records (optional) and functions. A module must export at least one function, others may be private - usable only from functions defined within the same module. Records are always visible only within the same module and may not be exported. A module may be defined as a file - then the file must take name of the module + `.yatta` suffix. Also, se section about [module loader]({{< ref "module-loader" >}}) for details regarding loading modules.

```haskell
module package\DemoMmodule 
    exports function1, function2
as

record DataStructure = (field_one, field_two)

function1 = :something

function2 = :something_else
end
```

Calling functions from a module is denoted by a double colon:
```
package\DemoModule::function1
````

Modules must have capitalized name, while packages are expected to start with a lowercase letter.

Module may also be defined dynamically, for example assigned to a name in a `let` expression, for example:

```haskell
let some_module = module TestModule exports test_function as
        test_function = 5
    end
in
    some_module::test_function
```

In this case the name of the module does not matter, as the module is assigned to the `test_module` value.

### Packages
Packages are logical units for organizing modules. Modules stored in packages must follow a folder structure exactly the same as the package path.

### Records
New data structures in Yatta can be implemented as simply as by using tuples. However, as tuples grow in number of elements, it may become useful to name those elements rather than always matching on a particular n-th element. To do so, Yatta provides `records`.

Records are essentially named tuples with names for each element and can be used to refer to a particular element by that name. Records exist within the scope of a module and cannot be imported or exported to other modules. Modules are meant to provide interface via functions alone.

Record is defined by its name and a list of field names. An example of a record definition:

```haskell
record Car = (brand, model, engine_type)
```

To initialize a new record instance, following syntax should be used:

```haskell
let
    car = Car(brand="Audi", model="A4", engine_type="TDI")
in
    ...
```

Note that not all fields are required when initializing record. Unitialized fields are then initalized with value of `()` (unit). At least one field must be provided during initialization, though, its value may be unit as well.

Updating an existing record instance (actually creating a new one with some fields changed) can be performed using this syntax:

```bash
let
    new_car = car(model="A6")  # this is same as Car(brand="Audi", model="A4", engine_type="TDI")
in
    ...
```

To access a field from a record instance a dot syntax is used:

```bash
println new_car.model  # will print A6
```

Pattern matching on records is very easy as well:

```bash
order_car Car(brand="Audi", model=model) = order_audi_model model
order_car Car(brand="Lexus", model=model) = order_lexus_model model
order_car any_car@Car = order_elsewhere any_car  # the whole record_instance is available under name any_car
```

## **`import` expression**: importing functions from other modules
Normally, it is not necessary to import modules, as it is often the case in many other languages. Functions from another modules can be called without explicitly declaring them as imported. However, Yatta has a special `import` expression (and as such it returns the value of the expression followed the `in` keyword) that allows importing functions from modules and in that way create aliases for otherwise fully qualified names.

```bash
import
    funone as one_fun, otherfun from package\SimpleModule
    funtwo from other\package\NotSimpleModule
in
    onefun :something  # expression that is the return value of the whole import expression
```

Note that importing functions from multiple modules is possibly, they just have to be put on new lines. Functions can be renamed using the `as` keyword.

See section about [module loader]({{< ref "module-loader" >}}) for details regarding loading modules.

## **`raise`, `try`/`catch` expressions**: raising and catching exceptions
Yatta is not a pure language, therefore it allows raising exceptions. Exceptions in Yatta are represented as a tuple of a symbol and a message. Message can be empty, if not provided as an argument to the keyword/function `raise`.

Exceptions in Yatta consist of three components. Type of exception is of type of symbol. The second component is a string description of the exception - an error message. Last component is the stacktrace which is appended by the runtime automatically.

Raising an exception can be accomplished by the `raise` expression:

```bash
raise :badarg "Error message"  # where :badarg is a symbol denoting type of exception
```

Cachting exceptions is done via the `try`/`catch`. Catching an exception is essentially pattern matching on an exception triple that consists of all three exception components.

```
try
    expression
catch
    (:badarg, error_msg, stacktrace)  -> :error
    (:io_error, error_msg, stacktrace) -> :error
end
```

## **Loops**: recursion and generators
Yatta is a functional language with immutable data types and no variables. This means that imperative constructs for loops, such as `while` or `for` cannot be used.
Instead, iteration is normally achieved via recursion. Yatta is able to optimize tail-recursive function calls, so they would not stack overflow.

A typical Python solution using mutation might look like this:

```python
def factorial(n):
    i = n
    while i > 1:
        i -= 1
        n *= i
    return n
```

However, this solution requires mutable variables, which are not present in Yatta. So, an example of a recursive function to calculate factorial in Yatta would be:

```haskell
factorial 1 = 1
factorial n = n * factorial (n - 1)
```

Note that this function is actually not tail-recursive. This is because `factorial (n - 1)` is evaluated before `n * (factorial (n - 1))`, so the multiplication is the last expression here, and thus there is a potential for stack overflow. That might often not be an issue, just something to consider when writing recursive functions.

It would actually not be verydifficult to rewrite this function to be tail-recursive, such as:

```haskell
factTR 0 a = a
factTR n a = factTR (n - 1) (n * a)

factorial n = factTR n 1
```

In this case there is a helper function that is tail-recursive, since the last epxression in that function is the call to itself.

### Generators
Another way to iterate over a collection (sequence, set or dictionary) are generator expressions. They allow transforming an existing collection into another one (of possibly different type, such as sequence to dictionary). Generator consists of three or four components:

The syntax for a generator generating a sequence from a set:
```haskell
[x * 2 | x <- {1, 2, 3}]  # the source collection is a set of 1, 2, 3, so the result is [2, 4, 6]
[x * 2 | x <- {1, 2, 3} if x % 2 == 0]  # generator with a condition, so the result is [4]
```

The syntax for a generator generating a set from a sequence:
```haskell
[x * 2 | x <- [1, 2, 3]]  # the source collection is a set of 1, 2, 3, so the result is {2, 4, 6}
[x * 2 | x <- [1, 2, 3] if x % 2 == 0]  # generator with a condition, so the result is {4}
```

The syntax for a generator generating a dictionary:
```haskell
{key = val * 2 | key = val <- {:a = 1, :b = 2, :c = 3}}  # the source collection is a set of 1, 2, 3, so the result is {:a = 2, :b = 4, :c = 6}
{key = val * 2 | key = val <- {:a = 1, :b = 2, :c = 3} if val % 2 == 0}  # generator with a condition, so the result is {:b = 4}
```

Generators are an easy an conveniet way to transform built-in collections. They are, however, themselves implemented using reusable [Transducers]({{< ref "stdlib/transducers" >}}) module, for example a set generator without using the above mentioned syntax "sugar" could look like:

```haskell
Transducers::filter \val -> val < 0 (\-> 0, \acc val -> acc + val, \acc -> acc * 2)
|> Set::reduce [1, 2, 3]
```

The description of Transducer functions can be found in the module itself. Transducers may be combined in order to create more complex transformations. Also, custom collection may implement their version of a `reduce` function that accepts a transducer and reduces the collection as desired.

## Strings
Strings in Yatta are technically sequences of UTF-8 characters. Character in Yatta can be written between apostrophes. Working with strings is then no different than working with any other sequence. String literals can be multi-line as well. There is no special syntax for multi-line strings and a single pair of quotes is used to denote all string literals.

### String Interpolation
Yatta supports string interpolation for convenience of formatting strings. The syntax is as follows:

```
"this string contains an interpolated {variable}"
```

In this string the `{variable}` part is replaced with whatever contents of the `variable`. Technically,
`variable` is a name of a bound variable, or an expression in parentheses.

It is also possible to use string interpolation with alignment option, which can be used for formatting tabular outputs:

```
"{column1,10}|{column2, 10}"
```

The alignment number will make the value align to the right and filled with spaces to the left, if positive. If the number is negative, the opposite applies and the text is aligned to the left with spaces on the right. The number can be either a literal value or any expression that return an integer.
