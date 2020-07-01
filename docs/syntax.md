---
title: "Syntax"
---
Programs in Yatta consist always of evaluation of a single expression. In fact, any Yatta program consists of exactly one expression.

The syntax is intentionally very minimalistic and inspired by languages such as SML or Haskell. There are only a handful of keywords, however it is not as flexible in naming as Haskell for example.

Yatta programs have the ambition to be easily readable. Custom operators with names consisting of [symbols](data-types.md) alone are not that useful when reading a program for the first time. This is why Yatta does not support custom operators named by [symbols](data-types.md) only. Supported operators are listed [here](operators.md).

Yatta does not have indentation specific parsing rules, but it does require new line at certain locations, which are noted in each individual expression descriptions.

Comments are denoted by the `#` character and everything that follows this character until the end of line (`\n` or `\r\n`) is considered a comment and ignored.

The source code of a Yatta program must be a valid UTF-8 text file.

## **Terminology**
These are some common terms and phrases used throughout these texts explained to a user unfamiliar with functional concepts:

* **Function application**: simply function call.

* **[Currying](https://javascript.info/currying-partials)**: a transformation of functions that translates a function from callable as `f(a, b, c)` into callable as `f(a)(b)(c)`. Currying doesn’t call a function. It just transforms it. The example uses JavaScript syntax for clarity.

* **Alias**: what would be a variable in other languages, but cannot have its value modified during program execution. Once set, this __alias__, or a __name__, always contains the same original value.

* **Pattern**: an expected shape of a value. If the value "matches" this pattern, then this value can also be deconstructed onto it. For instance a pattern can be a first element of a list and the rest. The first element can be also assigned to an alias (or the rest), provided that the value matched this pattern. Patterns can be nested and serve as a builidng blocks for describing data structure for matching and extracting values. Patterns may be as simple as an alias - that is also a pattern in a way. An alias will be matched if an only it is a new name, that was not previously assigned, or if it was, then only if the value is the same as the one previously assigned to it.

* **Guard**: guards are additional conditions that may be applied to patterns, that otherwise couldn't be expressed using pattern syntax. Typical example might be a function call to determine a type of a value for example.

* **String interpolation**: process of substituting values of variables into placeholders in a string. For instance, if there a template for saying hello to a person like "Hello {parson}, nice to meet you!", where the placeholder should be replaced with an actual person's name. This process is called string interpolation.

## **Functions**: definition and application
Functions in Yatta are defined in a very short and concise form. They may take arguments, which the function can pattern match on, and one function can be defined using multiple arguments. Function names must start with a lowercase letter, followed by any letters, numbers or underscores.

A simple function to calculate a factorial can be written for example this way:

```haskell
factorial 1 = 1
factorial n = n * factorial (n - 1)
```

Each function case must be on a new line. More complex conditions in patterns can be specified in this way:

```haskell
factorial 1 = 1
factorial n 
  | n > 1 = n * factorial (n - 1)
```
Which means that there is an additional condition for the `n` value to be greater than 0. There may be multiple guards for each pattern and they must each be on a new line. A guard starts with the `|` character, followed an expression that must evaluate to a boolean and finally an `=` and the expression to evaluate if the pattern matches and the guard is `true`. Guards are used to express conditions that must be met in order for the pattern to match. A pattern may have none, one or multiple guards, and the first one matching will cause the pattern to match.

Note that function arguments may actually be full patterns, not just argument names. Patterns are described in the section named *Pattern Matching*.

Yatta additionally supports non-linear patterns, meaning that if a pattern contains the same name multiple times, than this name is required to match the same value so that the pattern would match. This can be handy when checking for one value to be present in multiple places/arguments without having to explicitly write a guard that would ensure the equality.

### **Anonymous functions**: aka lambdas
Since functions are first-class citizens in Yatta, it is necessary to provide means of passing functions as arguments, and also to define them without giving them a name. The following syntax is used in this case:

```bash
\argument_one argument_two -> argument_one argument_two  # lambda function for summing its arguments
```

A lambda function with no arguments is simply: `\-> :something`.

### **Function application**: calling functions
Calling a function simply means writing the name of the function and then specifying its arguments. If fewer arguments are provided than the function expects, this is considered a [curried](https://en.wikipedia.org/wiki/Currying) call, and the result of such function is a partially applied function, that can be called with the remaining arguments at a later point.

So for example calling a `factorial` would look simply like this:

```haskell
factorial 5
```

Since Yatta is a strictly evaluated language, meaning that arguments are evaluated before calling the function (as opposed to a lazy language, where arguments are only evaluated when actually used by the called function), there is one situation to be careful about and that is passing lambda functions of zero arguments as arguments to functions. This would be evaluated before actually calling the function. If there are no side-effects happening in the lambda, it might be perfectly fine, however if the lambda must be passed as a lambda, it needs to be wrapped into another lambda at the call-site, such as for example:

```bash
let
    lambda = \-> println :hello
in
    do_something_with \-> lambda  # instead of do_something_with lambda
```

Then the `do_something_with` function will obtain its argument as a function and not a result of `println :hello` function (that would be `:hello` btw).

### Pipes and operator precedence
Since Yatta is an ML-style language and, unlike many C-like languages, it does not use parentheses to denote a function application, it can become unclear which expressions are arguments of which function. Take for the following example:

```haskell
Seq::take 5 Seq::random 10
```

Is this a function call to the `Seq::take` function (that is a function `take` in the module `Seq` as explained later) with 3 arguments (5, `Seq::random` and 10) or is it `Seq::random 10` supposed to be computed first and then its result used as a second argument to `Seq::take`?

If the latter, then it can be written this way:

```haskell
Seq::take 5 (Seq::random 10)
```

Since any expression can be put into parentheses and be given precedence in evaluation. Another way to write this would be using *pipes*:

```haskell
Seq::take 5 <| Seq::random 10
```

or

```haskell
Seq::random 10 |> Seq::take 5
```

These are all equivalent expression and it is up to the programmer's preference to decide which one to use. One nice feature that pipes have is that they can be used on multiple lines, such as:

```haskell
Seq::random 10
|> Seq::take 5
|> println
```

## **`let` expression**: defining local aliases / pattern matching in the scope of evaluated expression {: #let-expression}
The `let` expression allows defining aliases in the scope of the executed expressions. Alias represents a name for some value, like a variable, unlike a variable though, its value may not be changed over time. This expression allows evaluating patterns as well, so it is possible to deconstruct a value from a sequence, tuple, set, dictionary or a record directly, for example:

```haskell
let
    (1, second) = (1, 2)
    pattern     = expression1
in
    expression2
```

As shown in this example, the `let` expression consists of two parts. Each line consists of an alias or a pattern on the left side, and the expression to evaluate on the right side. The result of this expression is then matched to the pattern on the left side, or simply assigned to an alias, depending on what is on the left side. The result of this `let` expression is the result of the `expression2` expression. If a pattern on the left is not matched on any of the lines, the whole `let` expression raises a `:nomatch` exception. One line can use names defined in previous lines.
Every alias/pattern must be defined on a new line.

Note that the order of execution of the alias/pattern lines is not strictly sequential. Considering the example in the [documentation homepage](index.md#example), some aliases can be executed as a single batch, in parallel, provided that two conditions are met:
* the do not depend on each other (do not use names provided by other aliases in the same batch)
* they return a runtime Promise (IO operations, or results of the [`async`](stdlib/functions/async.md) function)

When both conditions are met, Yatta can safely execute them concurrently, avoiding unnecessary blocking and effectively speeding up the program execution.

## **`do` expression**: sequencing side effects {: #do-expression}
The `do` expression is used to define a sequence of side effecting expressions. This expression is very similar to the `let` expression in the sense that it allows defining aliases and patterns, however, it doesn't have a separate expression that would be used as a result of this expression. Instead the result of the last line is used as the result.

```haskell
do
    start_time  = Time::now
    (:ok, line) = File::read_line f
    end_time    = Time::now
    printf line
    printf (end_time - start_time)
end
```

Note that unlike the case with the `let` expression, the order of executed expressions is guaranteed to be exactly the same as the order in which they are written. This is the main use-case of the `do` expression: to allow strict ordering of execution. It does not matter whether an expression returns a run-time level Promise (such as one that can be created by a IO call or by using the [`async`](stdlib/functions/async.md) function), the order defined in this expression is always guaranteed to be maintained.

## **`case` expression**: pattern matching
`case` expression is used for pattern matching on an expression. Each line of this expression contains a pattern followed by an arrow `->` and an expression that is evaluated in case of a successful pattern match. Patterns are tried in the order in which they are specified. The default case can be denoted by an underscore `_` pattern that always evaluates as true.

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

Same as with function definition patterns, patterns in the `case` expression can contain guard expressions, as is the case in the *tuple* pattern in the example above. Patterns with a guard expression will be matched only the guard evaluated to `true`. Note that each pattern may have multiple guard expression, in which case the first one evaluated to `true` will match. Guards are tried in the order they are specified in.

## **`if` expression**: conditions
`if` is a conditional expression that takes form:

```haskell
if expression
then
    expression
else
    expression
```

Both the `then` and the `else` parts must be defined. The expression after `if` must evaluate to a boolean value. Empty sequences, dictionaries or similar will not work! New lines are allowed, but not required.

## **`with` expression**: resource management {: #with-expression}
`with` expression handles resource management within its scope. It initilizes the scope with a [context manager](resource-management.md#context-managers) that is available within the scope its body.

The syntax looks like this (`as alias` part is optional):
```haskell
with contextManager as alias
    bodyExpression
end
```

The `contextManager` expression must return a [context manager](resource-management.md#context-managers). If no alias is used, then the context manager is not meant to be interacted with directly. An example for this would be an [STM](stdlib/stm.md) transaction, that just needs to be present but there is no need to directly refer to it.

Alternatively, if the alias is specified, as usually case with files, then the file creted in the `contextManager` part of this expression can be directly accessed in `bodyExpression`.

Example that reads all lines using [`File::read_lines`](stdlib/file.md#opening-closing) function:

```haskell
with File::open "File.txt" {:read} as file
    File::read_lines file
end
```

## **module expression**
`module` is an expression representing a set of records (optional) and functions. A module must export at least one function, others may be private - usable only from functions defined within the same module. Records are always visible only within the same module and may not be exported. A module may be defined as a file - in this case, the file must take the name of the module + `.yatta`. Also, see section about [module loader](module-loader.md) for details regarding loading modules.

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

Modules must have capitalized names, while packages are expected to start with a lowercase letter.

Module may also be defined dynamically, for example assigned to a name in a `let` expression:

```haskell
let some_module = module TestModule exports test_function as
        test_function = 5
    end
in
    some_module::test_function
```

In this case the name of the module does not matter, as the module is assigned to the `test_module` value.

## **`with` expression**: resource management {: #with-expression}
`with` expression handles resource management within its scope. It initilizes the scope with a [context manager]({{< relref "resource-management#context-managers" >}}) that is available within the scope its body.

The syntax lookes like this (`as alias` part is optional):
```haskell
with contextManager as alias
    bodyExpression
end
```

The `contextManager` expression must return a [context manager](resource-management#context-managers). If no alias is used, then the context manager is not meant to be interacted with directly. An example for this would be an [STM](stdlib/stm) transaction, that just needs to be present but there is no need to directly refer to it.

Alternatively, if the alias is specified, as usually case with files, then the file creted in the `contextManager` part of this expression can be directly accessed in `bodyExpression`.

Example that reads all lines using [`File::read_lines`](stdlib/file#opening-closing) function:

```haskell
with File::open "File.txt" {:read} as file
    File::read_lines file
end
```

### Packages
Packages are logical units for organizing modules. Modules stored in packages must follow a folder structure which is exactly the same as the package path.

### Records
New data structures in Yatta can be implemented simply by using tuples. However, as tuples grow in number of elements, it may become useful to name those elements rather than always matching on a particular n-th element. To do so, Yatta provides `records`.

Records are essentially named tuples with names for each element and can be used to refer to a particular element by that name. Records exist within the scope of a module and cannot be imported by or exported to other modules. Modules are meant to provide an interface via functions alone.

A record is defined by its name and a list of field names. Here's an example of a record definition:

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

Note that not all fields are required when initializing record. Uninitialized fields are then initialized with value of `()` (unit). At least one field must be provided during initialization, though (its value may be unit as well).

Updating an existing record instance (actually creating a new one with some fields changed) can be done using this syntax:

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
Normally, it is not necessary to import modules, as it is often the case in many other languages. Functions from another modules can be called without explicitly declaring them as imported. However, Yatta has a special `import` expression (and as such it returns the value of the expression followed by the `in` keyword) that allows importing functions from modules and in that way create aliases for otherwise fully qualified names.

```bash
import
    funone as one_fun, otherfun from package\SimpleModule
    funtwo from other\package\NotSimpleModule
in
    onefun :something  # expression that is the return value of the whole import expression
```

Note that importing functions from multiple modules is possible, they just have to be put on new lines. Functions can be renamed using the `as` keyword.

See the section about [module loader](module-loader.md) for more details regarding loading modules.

## **`raise`, `try`/`catch` expressions**: raising and catching exceptions
Yatta is not a pure language, therefore it allows raising exceptions. Exceptions in Yatta are represented as a tuple of a symbol and a message. Message can be empty, if not provided as an argument to the keyword/function `raise`.

Exceptions in Yatta consist of three components. First component, is the type of the exception, represented as a symbol. The second component is a string description of the exception - an error message. Last component is the stacktrace which is appended by the runtime automatically.

Raising an exception can be accomplished by the `raise` expression:

```bash
raise :badarg "Error message"  # where :badarg is a symbol denoting type of exception
```

Catching an exception is done with `try`/`catch`. Catching an exception is essentially pattern matching on an exception triple that consists of all three exception components.

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

Note that this function is actually not tail-recursive. This is because `factorial (n - 1)` is evaluated before `n * (factorial (n - 1))`, so the multiplication is the last expression here and thus there is a potential for stack overflow. That might often not be an issue, just something to consider when writing recursive functions.

It would actually not be very difficult to rewrite this function to be tail-recursive, such as:

```haskell
factTR 0 a = a
factTR n a = factTR (n - 1) (n * a)

factorial n = factTR n 1
```

In this case there is a helper function that is tail-recursive, since the last expression in that function is the call to itself.

### Generators
Another way to iterate over a collection (sequence, set or dictionary) are generator expressions. They allow transforming an existing collection into another one (of possibly different type, such as sequence to dictionary). Generators consist of three or four components:

Syntax for a generator generating a sequence from a set:
```haskell
[x * 2 | x <- {1, 2, 3}]  # the source collection is a set of 1, 2, 3, so the result is [2, 4, 6]
[x * 2 | x <- {1, 2, 3} if x % 2 == 0]  # generator with a condition, so the result is [4]
```

Syntax for a generator generating a set from a sequence:
```haskell
[x * 2 | x <- [1, 2, 3]]  # the source collection is a set of 1, 2, 3, so the result is {2, 4, 6}
[x * 2 | x <- [1, 2, 3] if x % 2 == 0]  # generator with a condition, so the result is {4}
```

Syntax for a generator generating a dictionary:
```haskell
{key = val * 2 | key = val <- {:a = 1, :b = 2, :c = 3}}  # the source collection is a set of 1, 2, 3, so the result is {:a = 2, :b = 4, :c = 6}
{key = val * 2 | key = val <- {:a = 1, :b = 2, :c = 3} if val % 2 == 0}  # generator with a condition, so the result is {:b = 4}
```

Generators are an easy an convenient way to transform built-in collections. They are, however, themselves implemented using the reusable [Transducers](stdlib/transducers.md) module. For example, a set generator without using the above mentioned syntax "sugar" could look like:

```haskell
Transducers::filter \val -> val < 0 (\-> 0, \acc val -> acc + val, \acc -> acc * 2)
|> Set::reduce [1, 2, 3]
```

The description of the Transducer functions can be found in the module itself. Transducers may be combined in order to create more complex transformations. Also, custom collection may implement their version of a `reduce` function that accepts a transducer and reduces the collection as desired.

## Strings
Strings in Yatta are technically sequences of UTF-8 characters. Character in Yatta can be written between apostrophes. Working with strings is then no different than working with any other sequence. String literals can be multi-line as well. There is no special syntax for multi-line strings and a single pair of quotes is used to denote all string literals.

### String Interpolation
Yatta supports string interpolation for convenience in formatting strings. The syntax is as follows:

```
"this string contains an interpolated {variable}"
```

In this string the `{variable}` part is replaced with whatever contents of the `variable`. Technically,
`variable` is a name of a bound variable, or an expression in parentheses.

It is also possible to use string interpolation with alignment option, which can be used for formatting tabular outputs:

```
"{column1,10}|{column2, 10}"
```

The alignment number will make the value align to the right and filled with spaces to the left, if positive. If the number is negative, the opposite applies and the text is aligned to the left with spaces on the right. The number can be either a literal value or any expression that returns an integer.
