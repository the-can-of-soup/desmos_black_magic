# Desmos Black Magic & Undocumented Features

> [!NOTE]
>
> While Desmos has a few different calculator types, this document mainly focuses on the Desmos Graphing Calculator.

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
* [Complex Numbers](#complex-numbers)
  * [Overview](#overview)
  * [Differentiating real number R vs complex number R±0i](#differentiating-real-number-r-vs-complex-number-r0i) ![Magic Level: Easy](https://img.shields.io/badge/Magic_Level:-Easy-green?style=flat-square)
* [Lists](#lists)
  * [List Restrictions](#list-restrictions)
  * [List Substitution](#list-substitution)
  * [Differentiating Lists of Numbers From Numbers](#differentiating-lists-of-numbers-from-numbers) ![Magic Level: Difficult](https://img.shields.io/badge/Magic_Level:-Difficult-orange?style=flat-square)
* [Points & 3D Points](#points--3d-points)
  * [Converting Number/Point Inputs to Points](#converting-numberpoint-inputs-to-points) ![Magic Level: Insane](https://img.shields.io/badge/Magic_Level:-Insane-magenta?style=flat-square)
  * [Converting Number/Point Inputs to Numbers](#converting-numberpoint-inputs-to-numbers) ![Magic Level: Easy](https://img.shields.io/badge/Magic_Level:-Easy-green?style=flat-square)
* [Built-in Functions](#built-in-functions)
  * [Fragile Functions](#fragile-functions)
  * [List of Fragile Functions](#list-of-fragile-functions)

### Help
If you need help related to anything in this document, you can [create a GitHub issue](https://github.com/the-can-of-soup/desmos_black_magic/issues).

### Magic Levels
In this document, sections that are describing black magic will have a difficulty rating:
| Difficulty                                                                                              | Description                                      |
|---------------------------------------------------------------------------------------------------------|--------------------------------------------------|
| ![Magic Level: Easy](https://img.shields.io/badge/Magic_Level:-Easy-green?style=flat-square)            | Easy to understand, fairly believable            |
| ![Magic Level: Difficult](https://img.shields.io/badge/Magic_Level:-Difficult-orange?style=flat-square) | More complex, mildly mind-blowing                |
| ![Magic Level: Insane](https://img.shields.io/badge/Magic_Level:-Insane-magenta?style=flat-square)      | Obscure workarounds, why on earth does this work |

## Types in Desmos

### All Types
There are a lot of different types in Desmos. While most of the time we only use numbers, some other types can sometimes be very useful. Here is the list of types I know of, each with an example expression to produce them, sorted from easiest to hardest to manipulate:

| Type           | Example                             | Can be stored in lists?  | Can use [method notation](#method-notation)? | Supported [Attributes](#attribute-notation) | Notes |
|----------------|-------------------------------------|--------------------------|----------------------------------------------|---------------------------------------------|-------|
| Number         | `2`                                 | <ul><li>- [x] </li></ul> | <ul><li>- [ ] </li></ul>                     | `.real`, `.imag`                            |       |
| Complex Number | `3 - 2i`                            | <ul><li>- [x] </li></ul> | <ul><li>- [ ] </li></ul>                     | `.real`, `.imag`                            | These normally can only be created when "Complex Mode" is enabled in the graph's settings, however this can be bypassed with the use of [fragile functions](#fragile-functions). |
| List           | `[1, 2, 3]`                         | <ul><li>- [ ] </li></ul> | <ul><li>- [x] </li></ul>                     |                                             |       |
| Point          | `(1, 2)`                            | <ul><li>- [x] </li></ul> | <ul><li>- [ ] </li></ul>                     | `.x`, `.y`                                  |       |
| 3D Point       | `(1, 2, 3)`                         | <ul><li>- [x] </li></ul> | <ul><li>- [ ] </li></ul>                     | `.x`, `.y`, `.z`                            |       |
| Distribution   | `normaldist(0, 1)`                  | <ul><li>- [x] </li></ul> | <ul><li>- [x] </li></ul>                     |                                             |       |
| Tone           | `tone(440)`                         | <ul><li>- [x] </li></ul> | <ul><li>- [x] </li></ul>                     |                                             |       |
| Polygon        | `polygon([(1, 2), (3, 4), (5, 6)])` | <ul><li>- [x] </li></ul> | <ul><li>- [x] </li></ul>                     |                                             |       |
| Color          | `rgb(255, 0, 0)`                    | <ul><li>- [x] </li></ul> | <ul><li>- [ ] </li></ul>                     |                                             | When displaying a color or list of colors, Desmos will show a fake `undefined` text and error. To prevent this, simply assign the color or list of colors to a variable. |
| Action         | `a -> 1`                            | <ul><li>- [ ] </li></ul> | N/A                                          |                                             | It is unknown whether method notation is supported for actions because there are no known functions that accept an action to test it with. |

> [!TIP]
>
> There exist some more types that are not listed above that are only available via normal means in other calculator types. However, some of these other types may still be accessible in the Graphing Calculator via the use of [fragile functions](#fragile-functions).
>
> Maybe at some point in the future I will document all of these types here, but for now consider the above list of types unfinished.

### General Restrictions
* Attempting to use a non-number value in any comparison results in an error (before applying [list substitution](#list-substitution)).
* Attempting to pass a value of the wrong type to a built-in function results in an error, although some built-in functions accept multiple different types.
* Attempting to use two different types as the branches of a piecewise or the elements of a list results in an error.

### Attribute Notation
Some types have attributes that can be accessed with the `.attribute` syntax. Here is the list of attributes that I know of:
* `point.x` and `point.y` retrieve the X and Y coordinates of the point.
* `point3.x`, `point3.y`, and `point3.z` retrieve the X, Y, and Z coordinates of the 3D point.
* `complex.real` and `complex.imag` retrieve the real and imaginary components of a complex number. These also work for regular numbers if "Complex Mode" is enabled in the graph settings.

  This may look like [method notation](#method-notation) because there exist functions `real()` and `imag()` that do the same thing, but this is actually attribute notation because it doesn't follow the other rules of method notation.

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
`NaN` is what most undefined values are, but a couple special cases produce infinite values instead. `n / 0` produces `Infinity` for `n > 0`, and `-Infinity` for `n < 0`. For `n = 0`, this produces `NaN`. `0 * ∞` also produces `NaN`. `log(0)` produces `-Infinity`. You can also create infinite values with the symbol `∞`, which can be typed with the shortcut `infty` or `infinity`. Finally, any number with a magnitude too large to be represented will be converted to `Infinity` or `-Infinity`.

### Negative 0
There is also a cursed number that exists called `-0`, which behaves exactly like `0`, except when you put it in the denominator of a division, it flips the sign of the infinite number produced. This means that `n / -0` produces `-Infinity` for `n > 0`. `-0` shows up as `0` in vanilla Desmos unless you enable the "Advanced floating point" option in [DesModder](https://www.desmodder.com/) that I mentioned before. Here are some ways to create `-0`:
* `-0`
* `0 * -1`
* `0 / -1`
* `-1 / ∞`
* `1 / -∞`

Here are some more properties of `-0`:
* `-0 = 0` is true.
* `-0 + 0` returns `0`, but `-0 + -0` and `-0 - 0` return `-0`.
* `-0 * n` returns `-0` for all nonnegative `n` and `0` for all negative `n`. `-0` counts as negative here.
* Reading a variable that has a slider and is set as `-0` will return `0`.
* `sgn(n)`, `sign(n)`, and `signum(n)` will return `n` when `n = 0`. This means that `sgn(-0)` is `-0`.
* With "Complex Mode" enabled, `(-0).real` returns `-0`.
* With "Complex Mode" enabled, `-0 + ni` returns `0 + ni` for all nonnegative `n` and `-0 + ni` for all negative `n`. `-0` counts as negative here.
* With "Complex Mode" enabled, `-0 * i` returns `-0 - 0i`.

### Differentiating Undefined Numbers
![Magic Level: Easy](https://img.shields.io/badge/Magic_Level:-Easy-green?style=flat-square)

There are a few properties that can be used to differentiate between defined and undefined numbers. One very useful property is that `NaN` makes any conditional return false; i.e. `n = n` is true for any `n` except `NaN`. Infinite values can be identified by checking `|n| = ∞`, which is only true for infinite values. To check the sign of the infinite value, you can just check `n > 0`, which is true for `Infinity`, but false for `-Infinity`.

### Differentiating Negative 0
![Magic Level: Easy](https://img.shields.io/badge/Magic_Level:-Easy-green?style=flat-square)

The only method I know to differentiate `0` and `-0` is by dividing them from a nonzero `n`, which will produce either `Infinity` or `-Infinity`. The easiest check is `1 / n > 0`, which will be true for `0` and false for `-0`, because `1 / -0 = -Infinity`.



## Complex Numbers

### Overview
Complex numbers have a real and imaginary component, both of which are numbers. This means the real and imaginary components can have separate number types. However, there are some weird behaviors I have noticed that I can't fully explain. First, whenever any undefined number is expected to be in the imaginary component, or `NaN` is expected in the real component, the entire complex number becomes `NaN - NaN i`. This implies that the sign of the imaginary component is stored separately from the imaginary component itself, or the `-` sign is a graphical bug, as `-NaN` isn't an actual value that can exist. When `.imag` is used, `NaN` is returned, confirming that `-NaN` doesn't exist.

However, infinite real components appear to work properly. This means that `∞ + i` works as expected, but `0/0 + i` and `∞i` just collapse to `NaN - NaN i`.

Here are some properties of complex numbers:
* `0 * z` and `-0 * z` will return a complex number, even though it would make sense to return a regular number.
* `z / ∞` and `-z / -∞` will return the complex number `0`, but unlike regular numbers, `z / -∞` and `-z / ∞` will also return the complex number `0` (instead of `-0`).

### Differentiating real number R vs complex number R±0i
_Written by [@marralesfios](https://github.com/marralesfios)_

![Magic Level: Easy](https://img.shields.io/badge/Magic_Level:-Easy-green?style=flat-square)

The important property we will rely on is that `∞ × 1 = ∞` but `∞ × (1 ± 0i) = NaN - NaN i`. We have to be a bit careful because `∞ × 0 = NaN`, but if we ensure the real component is not zero, then we can just multiply by infinity and plug into a NaN detector to tell whether a number is of complex type. That is: `{u = u: 0, 1} with u = ∞{x.real = 0: x + 1, x}` 



## Lists

### List Restrictions
There are a few restrictions on lists in Desmos:
* Lists may not have greater than 10,000 elements.
* Lists must be of all the same type; i.e. you may not mix and match types.
* Lists may not contain lists or actions.
* Variables may not be set to lists containing `NaN` via actions.

### List Substitution
> [!NOTE]
>
> "List substitution" is also often referred to as "list broadcasting".

Lists can be used to substitute other values in expressions (as long as the result would obey [list restrictions](#list-restrictions)). For example, the expression `a^3 + 1` could be replaced with `[1, 2, 3]^3 + [4, 5]` and still be valid syntax. Whenever a type error would be produced due to an argument not supporting lists (e.g. taking the logarithm of a list), instead Desmos will perform a list substitution.

The way Desmos evaluates list substitutions is by iterating through all lists simulatenously until any of the lists are exhausted, and placing the results in a list. In the example, exponentiation comes before addition in the order of operations, so Desmos would first try to evaluate `[1, 2, 3]^3`. It will first evaluate `1^3`, then `2^3`, and finally `3^3`. The resulting list is `[1, 8, 27]`. Then, Desmos would see `[1, 8, 27] + [4, 5]`. Because Desmos iterates through all lists simultaneously, it will first calculate `1 + 4`, then `8 + 5`, but then the second list is exhausted, so it will stop. The final result is the list `[5, 13]`.

> [!IMPORTANT]
> List substitution can sometimes lead to accidental and weird results. One of the biggest instances of this is when using piecewise expressions. [Piecewise branches must all be of the same type](#general-restrictions), so when a list is used as a branch alongside another type, list substitution will trigger, causing *all branches* to become lists. For example, the piecewise `{a < 5: 24, [39, 54]}` will output correctly as `[39, 54]` for `a >= 5`, but for `a < 5`, the output is actually `[24, 24]`. This is because Desmos first evaluates `{a < 5: 24, 39}` and then `{a < 5: 24, 54}`, and both resulted in `24`, making the output `[24, 24]`.

Another example of undesired list substitution is the expression `{[1, 2, 3] = 1: 4, 5}`, which appears to be comparing the list `[1, 2, 3]` to the number `1` to see if they are equal. However, what is actually happening is because [non-numbers cannot be compared](#general-restrictions), this is actually checking each item in the list to see if it's equal to `1`, and so the expression evaluates to `[4, 5, 5]` instead of the expected `5`. This is even easier to accidentally encounter if using variables, for example `{x = 1: 4, 5}` where `x = [1, 2, 3]` would produce the same result.

### Differentiating Lists of Numbers From Numbers
![Magic Level: Difficult](https://img.shields.io/badge/Magic_Level:-Difficult-orange?style=flat-square)

We want to create a function `isListOfNumbers(x)` that returns `1` if `x` is a list of numbers, and returns `0` if `x` is a number. It is very difficult to differentiate types because passing the wrong type to a function anywhere, even in branches that are never used, immediately makes the entire equation result in an error. There are two key properties that I used to achieve this:
* The `join()` function will accept *any type* except actions.
* Due to list substitution, it is valid syntax to compare a number to a list, and it is also valid to compare a list to a list.

The first step is to detect any lists that don't have exactly 1 item, which is fairly easy. We can use `join([], x)` to convert the value to a list. After converting to a list, if `x` is a number, we will get the list `[x]`. If `x` is a list, we will just get `x` back again. We then check the length of this list. If it is anything but 1, we immediately know that `x` is a list. So our function so far is: `isListOfNumbers(x) = {join([], x).length = 1: ..., 1}`.

The next step is to address the `...` in our function. If the length of `join([], x)` is 1, we know that `x` is either a number or a list of length 1. Differentiating the two is the hardest part of this function. For this, I use list substitution. If `x` is a number, evaluating `{x = [0, 0]: 0, 0}` will substitute each element of the list `[0, 0]` into the condition, and end up with two responses. However, if `x` is a list of length 1, `x` will now be *shorter than `[0, 0]`*, and so the Desmos list substitution algorithm will only substitute values until `x` is exhausted. Therefore, we only get one response, checking if the first element in `x` equals `0`. So the final step is to take this result and check its length. If it is `1`, we know `x` is a list, and if it is `2`, we know `x` is a number.

So our final function is: `isListOfNumbers(x) = {join([], x).length = 1: {{x = [0, 0]: 0, 0}.length = 2: 0, 1}, 1}`.



## Points & 3D Points

### Converting Number/Point Inputs to Points
![Magic Level: Insane](https://img.shields.io/badge/Magic_Level:-Insane-magenta?style=flat-square)

We want to allow functions such as, for example, `getCoordinate(p,n) = {n=0:p.x, n=1:p.y, n=2:p.z}` to not error even with numerical inputs of `p`, to prevent errors in functions that could take either type. Even if functions do `{isPoint(x)=1:getCoordinate(x,n), ...}`, they would still error with numerical inputs, because Desmos does not care if the branch doesn't trigger; it will still error due to getting `.x` of a number. I ran into this problem myself working on my [video renderer](https://www.desmos.com/calculator/9jfamqnyic). Naturally, the solution would be a function `convertTo3DPoint(x)` that leaves points unchanged, but converts numbers into points; this would satisfy the type checker for numerical inputs by converting them to points. This entry will implement that function.

> [!NOTE]
> We do not care what the contents of the point result is if the input is a number, as we simply want to satisfy the type checker.
> Consequently, the result in this case will be garbage data.

> [!NOTE]
> There are two versions of this function; one for normal points, and one for 3D points.
> I will describe my process implementing only the 3D point version here, but the normal point version is also given at the end.

There are two key features that I exploit for this implementation. The first is the fact that the dot operator works on any combination of point/number inputs. This is because for points `a` and `b`, `a * b` gives the dot product of `a` and `b`, whereas for point `a` and number `n`, `a * n` or `n * a` gives `a` dilated by `n` (and the case with two numbers is trivial). Note that this is only true for the dot operator, and not the implicit multiplication operator; i.e., for points `a` and `b`, `ab` is not a valid operation, but `a * b` is.

We immediately exploit this by writing `(1,0,0) * x`, `(0,1,0) * x`, and `(0,0,1) * x`. For point `x`, this will calculate the dot product, thus yielding the X, Y, and Z components of `x` respectively. For number `x`, this will give the points `(x,0,0)`, `(0,x,0)`, and `(0,0,x)` respectively. Notably, we now have swapped the types; number inputs will yield points, and point inputs will yield numbers.

The second feature is that the absolute value operator works on both points and numbers. For numbers, it returns their magnitude, and for points, it returns the magnitude of the vector with their coordinates. We now will take our three results from before and take their absolute value; `|(1,0,0) * x|`, `|(0,1,0) * x|`, and `|(0,0,1) * x|`. If `x` is a point, these will be the absolute values of the components of `x`, but if `x` is a number, these will be the absolute value of that number.

Our obstacle to success now is determining the sign of the three components without causing errors for number inputs, and believe me, this is VERY difficult to solve, hence the Insane magic level.

My first breakthrough was that because `∞ - ∞` returns `NaN`, I could use `∞` in the dot products to determine difference in sign. For example, if we use `|(∞,∞,0) * x|`, for point `x`, this will return `NaN` if and only if the sign of the X and Y components of `x` are different; otherwise, it will return `∞` (excepting zero cases mentioned below). This is because we are effectively multiplying the X and Y components by `∞` and then adding them together. If they have the same sign, this becomes `∞ + ∞` or `-∞ - ∞`, both of which result in `∞` after the absolute value takes effect. However, and this is the key part, if they have a _different_ sign, this becomes `∞ - ∞` or `-∞ + ∞`, _both of which result in `NaN`_, and this of course propagates through the absolute value as well. If exactly one of the X or Y components are `0`, the result is `∞`, and if they both are `0`, the result is `0`. (For number `x`, the result after absolute value is `∞` for nonzero values or `0` for zero.)

> [!NOTE]
> The above trick is not used in the final solution, but I included it here for completeness.

Using this trick, we can deduce whether the X and Y components have the same sign, and whether the X and Z components have the same sign. Now, the final piece of the puzzle is identifying what the sign of the X component is. Literally 2 possible states. It couldn't _possibly_ be that hard to differentiate between the two. Right? **WRONG**. This problem took me a very long time to solve.

The solution is to use `-0`. Yes. You heard me right. MORE SHENANIGANS. If we use `(0,0,0) * x`, for point `x`, this will give `-0` if all three components are negative; otherwise, it will give `0`. This is because when each `0` is multiplied by a component of `x`, it becomes `0` if that component is positive, or `-0` if it is negative. When added, the only way addition can produce `-0` is by adding `-0 - 0`. All other combinations of signs of `0` being added result in `0`. This gives that behavior of `-0` if all three components are negative or `0` otherwise. (For number `x`, this gives a 3D point, and that's all that matters.)

Now, take that previous result and apply `* x` to get `((0,0,0) * x) * x`. For point `x`, if the previous result was `0` (as is always the case except when all components are negative), then this will result in a 3D point where each component is given by `0` times the component in `x`. This means that each resulting component is `0` if the original component was nonnegative, otherwise it is `-0`. We can use this to determine the sign!! But, remember that the previous result could be `-0` in the case that all components are negative. In this case, the result is entirely flipped by the negative there, so the final result is `(0,0,0)` instead of the desired `(-0,-0,-0)`. (For number `x`, the result is a 3D point because we just scale the previous result by `x`, and that's again all that matters.)

There is one final edge case. The only issue now is if `x` is a point with all three components negative, because that gets read as all three components positive (due to the second-to-last result being `-0` in that case instead of `0`). The solution to this is simple, yet ingeniously effective. I couldn't have found it without help from [`<div></div>`](https://github.com/Dicuo/). Show them support.

The solution is to detect this edge case by detecting when `(0,0,0) * x` is `-0` safely. The way this is done is with the expression `(x - x) * x`. For point `x`, this simplifies to `(0,0,0) * x`, giving the same result. But for number `x`, this simplifies to `0 * x`, _giving a number in this case as well!!_ This is the key part. The `(0,0,0) * x` expression would give a point for number `x`, so it wasn't safely usable, but the new `(x - x) * x` version returns a number _no matter what_. Now, we just simply multiply our expression from before `((0,0,0) * x) * x` by `(x - x) * x` (so that in the edge case, it is multiplied by `-0`, thus properly reversing the signs). This leaves us with `(((0,0,0) * x) * x) * ((x - x) * x)`, which **will properly return a point where each component is `0` or `-0` for a respective positive or negative component in `x` when `x` is a point**. (For `0` components in `x`, they are treated as positive, but `-0` is treated as negative, preserving this difference. This can be verified by testing zeroes in the expression.)

> [!IMPORTANT]
> Due to the `x - x` term, any components of `x` being infinite will result in a `NaN` value propagating, making the result `(NaN, NaN, NaN)`.
> Because of this, this function **will not properly handle or preserve the difference between different undefined types in point inputs**, as they cause the entire result become `NaN` in this term.
> This may be fixed in the future if a better solution or patch is found.

Finally, it is time to put it all together. First, we take the magnitudes of each component of `x` and put them in a point: `(|(1,0,0) * x|, |(0,1,0) * x|, |(0,0,1) * x|)`. Second, we get the sign of every component of `x` in the form of positive or negative zeroes: `(((0,0,0) * x) * x) * ((x - x) * x)`. Next, we divide each of these zeroes from 1 to get positive or negative `∞`, making detecting sign easier: `(1/s.x, 1/s.y, 1/s.z) with s = (((0,0,0) * x) * x) * ((x - x) * x)`. Then, we use the `sign` function to convert these entries to either `-1` or `1`: `(sign(1/s.x), sign(1/s.y), sign(1/s.z)) with s = (((0,0,0) * x) * x) * ((x - x) * x)`. And finally, we multiply these signs by the magnitudes we found at the start, giving:

Final function: `convertTo3DPoint(x) = (sign(1/s.x) * |(1,0,0) * x|, sign(1/s.y) * |(0,1,0) * x|, sign(1/s.z) * |(0,0,1) * x|) with s = (((0,0,0) * x) * x) * ((x - x) * x)`. For completeness, the normal point version of this is `convertToPoint(x) = (sign(1/s.x) * |(1,0) * x|, sign(1/s.y) * |(0,1) * x|) with s = (((0,0) * x) * x) * ((x - x) * x)`.

### Converting Number/Point Inputs to Numbers
![Magic Level: Easy](https://img.shields.io/badge/Magic_Level:-Easy-green?style=flat-square)

This is the same as the previous entry, except the reverse; converting to a number rather than a point or 3D point. Because of this, I will skip most of the explanation.

Basically we just take the same `(x - x) * x` expression from that entry, except now use it to get the sign of numbers. For number `x`, this evaluates to `0 * x`, or `0` for nonnegative `x` and `-0` for negative `x`. For point `x`, this evaluates to `(0,0,0) * x`, which is a dot product, so it results in some number. Either way, we divide from `1` and use the `sign` function, giving `sign(1 / ((x - x) * x))`. For number `x`, the inner part is `∞` or `-∞` based on sign, meaning after `sign`, it is `1` or `-1`; for point `x`, this is some number. Then, we use `|x|` to get the magnitude of `x`. For number `x`, this is the absolute value of `x`. For point `x`, this is the magnitude of the vector with its coordinates. Finally, we multiply the two, so that in the number case, the sign is restored after the absolute value. For the point case, this is multiplying two numbers, so it also results in a number.

Final function: `convertFromPoint(x) = sign(1 / ((x - x) * x)) * |x|`

> [!TIP]
> This function works for both normal points and 3D points, because nowhere in it does it use a feature limited to one of the two.



## Built-in Functions

### Fragile Functions
Fragile functions are hidden built-in functions that are not registered with the equation editor or renderer. This means that while they do exist and will function properly when in an equation, they do not appear in the list of built-in functions, they cannot be created by typing their name, nor do they render as functions; they instead show up as if you literally typed the name of the function as a bunch of variables multiplied together.

Fragile functions can be used to access types that are normally only available in other calculator types, access complex numbers with complex mode disabled, and more.

So how _do_ you use them?

In Desmos, while the equation renderer displays equations in fancy math notation, really all equations are stored as a bunch of special character sequences. You can see this for yourself by copying an equation or part of an equation to your clipboard and then pasting it into a text editor. Built-in functions specifically are notated by a backslash `\` followed by the name of the built-in function.

So to use a fragile function, you need to copy a backslash `\` followed by the name of the fragile function to your clipboard (this is the character sequence for the fragile function). However, simply pasting this into Desmos will not work, because after pasting, the fragile function will instantly dissolve (because the equation editor sees it as an invalid character sequence). To get around this, copy _multiple lines of text_ to your clipboard. For whatever reason, this bypasses the check, and the character sequence is directly pasted in.

Whenever an equation that contains a fragile function is edited, the function will dissolve. Because of this, make sure you have the entire equation that you want already copied when you are pasting in a fragile function.

### List of Fragile Functions

| Name                                | Notes |
|-------------------------------------|-------|
| `rtxsqpone`                         | |
| `rtxsqmone`                         | |
| `hypot`                             | |
| `logbase`                           | |
| `factorial`                         | |
| `polyGamma`                         | |
| `area`                              | |
| `perimeter`                         | |
| `pointDet`                          | |
| `pointDot`                          | |
| `pointPerp`                         | |
| `complexMultiply`                   | |
| `segment`                           | |
| `line`                              | |
| `ray`                               | |
| `vector`                            | |
| `vectorThreeD`                      | |
| `mathVector`                        | |
| `mathVectorThreeD`                  | |
| `vectorDisplacementAsPoint`         | |
| `vectorThreeDDisplacementAsPoint`   | |
| `basePointFromVector`               | |
| `basePointFromVectorThreeD`         | |
| `circle`                            | |
| `center`                            | |
| `radius`                            | |
| `arc`                               | |
| `arcCenter`                         | |
| `arcFirstPoint`                     | |
| `arcMiddlePoint`                    | |
| `arcThirdPoint`                     | |
| `arcOmega`                          | |
| `undirectedAngleMarker`             | |
| `directedAngleMarker`               | |
| `directedCoterminalAngle`           | |
| `undirectedCoterminalAngle`         | |
| `supplement`                        | |
| `directedAngleMarkerRawDelta`       | |
| `undirectedAngleMarkerRawDelta`     | |
| `directedAngleMarkerMultiplier`     | |
| `undirectedAngleMarkerMultiplier`   | |
| `polygonInteriorUndirectedAngles`   | |
| `polygonInteriorDirectedAngles`     | |
| `vertices`                          | |
| `segments`                          | |
| `scaleTangentTransformation`        | |
| `scaleTangentPolygon`               | |
| `scaleTangentSegment`               | |
| `scaleTangentLine`                  | |
| `scaleTangentRay`                   | |
| `scaleTangentCircle`                | |
| `scaleTangentArc`                   | |
| `scaleTangentDirectedAngleMarker`   | |
| `scaleTangentUndirectedAngleMarker` | |
| `addTangentPolygon`                 | |
| `addTangentSegment`                 | |
| `addTangentSegmentThreeD`           | |
| `addTangentLine`                    | |
| `addTangentRay`                     | |
| `addTangentVector`                  | |
| `addTangentCircle`                  | |
| `addTangentArc`                     | |
| `addTangentTransformation`          | |
| `addTangentDirectedAngleMarker`     | |
| `addTangentUndirectedAngleMarker`   | |
| `segmentGlider`                     | |
| `segmentThreeDGlider`               | |
| `lineGlider`                        | |
| `rayGlider`                         | |
| `circleGlider`                      | |
| `arcGlider`                         | |
| `polygonEdgeByParameter`            | |
| `polygonGlider`                     | |
| `chooseNonIncidentPoint`            | |
| `circleCircleIntersection`          | |
| `circleArcIntersection`             | |
| `circleLineIntersection`            | |
| `arcCircleIntersection`             | |
| `arcArcIntersection`                | |
| `arcLineIntersection`               | |
| `lineCircleIntersection`            | |
| `lineArcIntersection`               | |
| `lineLineIntersection`              | |
| `lineFromSegment`                   | |
| `lineFromRay`                       | |
| `parallel`                          | |
| `perpendicular`                     | |
| `rawTransform`                      | |
| `rawTransformConj`                  | |
| `transformWithoutTranslation`       | |
| `transformScaleFactor`              | |
| `translation`                       | |
| `dilation`                          | |
| `rotation`                          | |
| `reflection`                        | |
| `compose`                           | |
| `inverse`                           | |
| `transformPoint`                    | |
| `transformSegment`                  | |
| `transformLine`                     | |
| `transformRay`                      | |
| `transformVector`                   | |
| `transformCircle`                   | |
| `transformArc`                      | |
| `transformPolygon`                  | |
| `transformAngleMarker`              | |
| `transformDirectedAngleMarker`      | |
| `distanceThreeD`                    | |
| `segmentThreeD`                     | |
| `triangle`                          | |
| `sphere`                            | |
| `argmin`                            | |
| `argmax`                            | |
| `upperQuantileIndex`                | |
| `lowerQuantileIndex`                | |
| `quartileIndex`                     | |
| `upperQuartileIndex`                | |
| `lowerQuartileIndex`                | |
| `normalcdf`                         | |
| `normalpdf`                         | |
| `binomcdf`                          | |
| `binompdf`                          | |
| `poissoncdf`                        | |
| `poissonpdf`                        | |
| `uniformcdf`                        | |
| `uniformpdf`                        | |
| `invT`                              | |
| `invPoisson`                        | |
| `invBinom`                          | |
| `invUniform`                        | |
| `tpdf`                              | |
| `tcdf`                              | |
| `invNorm`                           | |
| `normalSample`                      | |
| `uniformSample`                     | |
| `tSample`                           | |
| `poissonSample`                     | |
| `binomSample`                       | |
| `validateRangeLength`               | |
| `validateSampleCount`               | |
| `select`                            | |
| `sortPerm`                          | |
| `elementsAt`                        | |
| `uniquePerm`                        | |
| `restriction`                       | |
| `restrictionToBoolean`              | |
