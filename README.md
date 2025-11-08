# Desmos Black Magic & Undocumented Features

Linked graph: [https://www.desmos.com/calculator/tlwypgbi04](https://www.desmos.com/calculator/tlwypgbi04)

### Table of Contents
* [Desmos Black Magic & Undocumented Features](#desmos-black-magic--undocumented-features)
  * [Table of Contents](#table-of-contents)
  * [Help](#help)
  * [Magic Levels](#magic-levels)
* [Types in Desmos](#types-in-desmos)
  * [All Types](#all-types)
  * [General Restrictions](#general-restrictions)
  * [Attribute Notation](#attribute-notation)
  * [Method Notation](#method-notation)
* [Numbers](#numbers)
  * [Types of Numbers](#types-of-numbers)
  * [Producing Undefined Numbers](#producing-undefined-numbers)
  * [Negative 0](#negative-0)
  * [Differentiating Undefined Numbers](#differentiating-undefined-numbers) ![Magic Level: Easy](https://img.shields.io/badge/Magic_Level:-Easy-green?style=flat-square)
  * [Differentiating Negative 0](#differentiating-negative-0) ![Magic Level: Easy](https://img.shields.io/badge/Magic_Level:-Easy-green?style=flat-square)
* [Lists](#lists)
  * [List Restrictions](#list-restrictions)
  * [List Substitution](#list-substitution)
  * [Differentiating Lists of Numbers From Numbers](#differentiating-lists-of-numbers-from-numbers) ![Magic Level: Difficult](https://img.shields.io/badge/Magic_Level:-Difficult-orange?style=flat-square)

### Help
If you need help related to anything in this document, you can [create a GitHub issue](https://github.com/the-can-of-soup/desmos_black_magic/issues).

### Magic Levels
In this document, sections that are describing black magic will have a difficulty rating:
| Difficulty                                                                                                         | Description                                                               |
|--------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------|
| ![Magic Level: Easy](https://img.shields.io/badge/Magic_Level:-Easy-green?style=flat-square)                       | Easy to understand, fairly believable                                     |
| ![Magic Level: Difficult](https://img.shields.io/badge/Magic_Level:-Difficult-orange?style=flat-square)            | More complex, mildly mind-blowing                                         |
| ![Magic Level: Insane](https://img.shields.io/badge/Magic_Level:-Insane-magenta?style=flat-square) (none made yet) | Obscure workarounds, you may develop a strong urge to star the repository |

## Types in Desmos

### All Types
There are a lot of different types in Desmos. While most of the time we only use numbers, some other types can sometimes be very useful. Here is the list of types I know of, each with an example expression to produce them, sorted from easiest to hardest to manipulate:

| Type         | Example                             | Can be stored in lists?  | Can use [method notation](#method-notation)? | Supported [Attributes](#attribute-notation) | Notes |
|--------------|-------------------------------------|--------------------------|----------------------------------------------|---------------------------------------------|-------|
| Number       | `1 + 2`                             | <ul><li>- [x] </li></ul> | <ul><li>- [ ] </li></ul>                     |                                             |       |
| List         | `[1, 2, 3]`                         | <ul><li>- [ ] </li></ul> | <ul><li>- [x] </li></ul>                     |                                             |       |
| Point        | `(1, 2)`                            | <ul><li>- [x] </li></ul> | <ul><li>- [ ] </li></ul>                     | `.x`, `.y`                                  |       |
| 3D Point     | `(1, 2, 3)`                         | <ul><li>- [x] </li></ul> | <ul><li>- [ ] </li></ul>                     | `.x`, `.y`, `.z`                            |       |
| Distribution | `normaldist(0, 1)`                  | <ul><li>- [x] </li></ul> | <ul><li>- [x] </li></ul>                     |                                             |       |
| Tone         | `tone(440)`                         | <ul><li>- [x] </li></ul> | <ul><li>- [x] </li></ul>                     |                                             |       |
| Polygon      | `polygon([(1, 2), (3, 4), (5, 6)])` | <ul><li>- [x] </li></ul> | <ul><li>- [x] </li></ul>                     |                                             |       |
| Color        | `rgb(255, 0, 0)`                    | <ul><li>- [x] </li></ul> | <ul><li>- [ ] </li></ul>                     |                                             | When displaying a color or list of colors, Desmos will show a fake `undefined` text and error. To prevent this, simply assign the color or list of colors to a variable. |
| Action       | `a -> 1`                            | <ul><li>- [ ] </li></ul> | N/A                                          |                                             | It is unknown whether method notation is supported for actions because there are no known functions that accept an action to test it with. |

### General Restrictions
* Attempting to use a non-number value in any comparison results in an error (before applying [list substitution](#list-substitution)).
* Attempting to pass a value of the wrong type to a built-in function results in an error, although some built-in functions accept multiple different types.
* Attempting to use two different types as the branches of a piecewise or the elements of a list results in an error.

### Attribute Notation
Some types have attributes that can be accessed with the `.attribute` syntax. Here is the list of attributes that I know of:
* `point.x` and `point.y` retrieve the X and Y coordinates of the point.
* `point3.x`, `point3.y`, and `point3.z` retrieve the X, Y, and Z coordinates of the 3D point.

### Method Notation
Some types can be used to call built-in functions with `value.method(...)` syntax, which is equivalent to `method(value, ...)`. Methods that only take one argument may optionally be called without parentheses like `value.method`, which is equivalent to `method(value)`. See [the table above](#all-types) for the list of types that support this. Here are some examples:
* `list.length` = `length(list)`
* `list.max()` = `max(list)`
* `list.join(5, 6)` = `join(list, 5, 6)`
* `distribution.pdf(0.5)` = `pdf(distribution, 0.5)`
* `tone(220).join(tone(440), tone(880))` = `join(tone(220), tone(440), tone(880))`



## Numbers

### Types of Numbers
There are generally four types of numbers: defined, `Infinity`, `-Infinity`, and `NaN`. All types except defined show as `undefined` in vanilla Desmos, but their true type can be seen with the "Visual > Better Evaluation View > Advanced floating point" option in the [DesModder extension](https://www.desmodder.com/).

### Producing Undefined Numbers
`NaN` is what most undefined values are, but a couple special cases produce infinite values instead. `n / 0` produces `Infinity` for `n > 0`, and `-Infinity` for `n < 0`. For `n = 0`, this produces `NaN`. `log(0)` produces `-Infinity`. You can also create infinite values with the symbol `∞`, which can be typed with the shortcut `infty` or `infinity`. Finally, any number with a magnitude too large to be represented will be converted to `Infinity` or `-Infinity`.

### Negative 0
There is also a cursed number that exists called `-0`, which behaves exactly like `0`, except when you put it in the denominator, it flips the sign of the infinite number produced. This means that `n / -0` produces `-Infinity` for `n > 0`. `-0` shows up as `0` in vanilla Desmos unless you enable the "Advanced floating point" option in [DesModder](https://www.desmodder.com/) that I mentioned before. Here are some ways to create `-0`:
* `-0`
* `0 * -1`
* `0 / -1`
* `-1 / ∞`

Interestingly, `sgn(n)`, `sign(n)`, and `signum(n)` will return `n` when `n = 0`. This means that `sgn(-0)` is `-0`.

### Differentiating Undefined Numbers
![Magic Level: Easy](https://img.shields.io/badge/Magic_Level:-Easy-green?style=flat-square)

There are a few properties that can be used to differentiate between defined and undefined numbers. One very useful property is that `NaN` makes any conditional return false; i.e. `n = n` is true for any `n` except `NaN`. Infinite values can be identified by checking `|n| = ∞`, which is only true for infinite values. To check the sign of the infinite value, you can just check `n > 0`, which is true for `Infinity`, but false for `-Infinity`.

### Differentiating Negative 0
![Magic Level: Easy](https://img.shields.io/badge/Magic_Level:-Easy-green?style=flat-square)

The only method I know to differentiate `0` and `-0` is by dividing them from a nonzero `n`, which will produce either `Infinity` or `-Infinity`. The easiest check is `1 / n > 0`, which will be true for `0` and false for `-0`, because `1 / -0 = -Infinity`.



## Lists

### List Restrictions
There are a few restrictions on lists in Desmos:
* Lists may not have greater than 10,000 elements.
* Lists must be of all the same type; i.e. you may not mix and match types.
* Lists may not contain lists or actions.
* Variables may not be set to lists containing `NaN` via actions.

### List Substitution
Lists can be used to substitute other values in expressions (as long as the result would obey [list restrictions](#list-restrictions)). For example, the expression `a^3 + 1` could be replaced with `[1, 2, 3]^3 + [4, 5]` and still be valid syntax. Whenever a type error would be produced due to an argument not supporting lists (e.g. taking the logarithm of a list), instead Desmos will perform a list substitution.

The way Desmos evaluates list substitutions is by iterating through all lists simulatenously until any of the lists are exhausted, and placing the results in a list. In the example, exponentiation comes before addition in the order of operations, so Desmos would first try to evaluate `[1, 2, 3]^3`. It will first evaluate `1^3`, then `2^3`, and finally `3^3`. The resulting list is `[1, 8, 27]`. Then, Desmos would see `[1, 8, 27] + [4, 5]`. Because Desmos iterates through all lists simultaneously, it will first calculate `1 + 4`, then `8 + 5`, but then the second list is exhausted, so it will stop. The final result is the list `[5, 13]`.

> [!IMPORTANT]
> List substitution can sometimes lead to accidental and weird results. One of the biggest instances of this is when using piecewise expressions. [Piecewise branches must all be of the same type](#general-restrictions), so when a list is used as a branch alongside another type, list substitution will trigger, causing *all branches* to become lists. For example, the piecewise `{a < 5: 24, [39, 54]}` will output correctly as `[39, 54]` for `a >= 5`, but for `a < 5`, the output is actually `[24, 24]`. This is because Desmos first evaluates `{a < 5: 24, 39}` and then `{a < 5: 24, 54}`, and both resulted in `24`, making the output `[24, 24]`.

Another example of undesired list substitution is the expression `{[1, 2, 3] = 1: 4, 5}`, which appears to be comparing the list `[1, 2, 3]` to the number `1` to see if they are equal. However, what is actually happening is because [non-numbers cannot be compared](#general-restrictions), this is actually checking each item in the list to see if it's equal to `1`, and so the expression evaluates to `[4, 5, 5]` instead of the expected `5`. This is even easier to accidentally encounter if using variables, for example `{x = 1: 4, 5}` where `x = [1, 2, 3]` would produce the same result.

### Differentiating Lists of Numbers From Numbers
![Magic Level: Difficult](https://img.shields.io/badge/Magic_Level:-Difficult-orange?style=flat-square)

It is very difficult to differentiate types because passing the wrong type to a function anywhere, even in branches that are never used, immediately makes the entire equation result in an error. There are two key properties that I used to achieve this:
* The `join()` function will accept *any type* except actions.
* Due to list substitution, it is valid syntax to compare a number to a list, and it is also valid to compare a list to a list.

The first step is to detect any lists that don't have exactly 1 item, which is fairly easy. We can use `join([], x)` to convert the value to a list. After converting to a list, if `x` is a number, we will get the list `[x]`. If `x` is a list, we will just get `x` back again. We then check the length of this list. If it is anything but 1, we immediately know that `x` is a list. So our function so far is: `isListOfNumbers(x) = {join([], x).length = 1: ..., 1}`.

The next step is to address the `...` in our function. If the length of `join([], x)` is 1, we know that `x` is either a number or a list of length 1. Differentiating the two is the hardest part of this function. For this, I use list substitution. If `x` is a number, evaluating `{x = [0, 0]: 0, 0}` will substitute each element of the list `[0, 0]` into the condition, and end up with two responses. However, if `x` is a list of length 1, `x` will now be *shorter than `[0, 0]`*, and so the Desmos list substitution algorithm will only substitute values until `x` is exhausted. Therefore, we only get one response, checking if the first element in `x` equals `0`. So the final step is to take this result and check its length. If it is `1`, we know `x` is a list, and if it is `2`, we know `x` is a number.

So our final function is: `isListOfNumbers(x) = {join([], x).length = 1: {{x = [0, 0]: 0, 0}.length = 2: 0, 1}, 1}`.
