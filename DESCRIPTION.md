# Pattern Matcher

In this project we will build a pattern matcher "by hand". We will not be using
existing libraries like regular expression matching.

## Examples

The pattern "x = (?is ?n numberp)" from [PAIP 6.2](https://github.com/norvig/paip-lisp/blob/main/docs/chapter6.md) 
will get written in Python as:

    [ "x", "=", ["?is", "?n", numberp] ]

Notice that the `numberp` is a lambda in a list, not a string.

### Example 1

Here is a short table of the results of matching this pattern:

| target | match result    |
|--------|-----------------|
| `["x","=",36]` | `{ '?n' : 36 }` |
| `["x", "=", "x"]` | `None`          |

### Example 2
Another example is the pattern
    
    ["?x", ["?or", "<","=",">"], "?y" ]

which, when matched against

    [3, "<", 4]

produces the dictionary

    {'?x': 3, '?y': 4}

### Example 3

Given the existing functions `numberp` (true when the input is a number) and
`oddp` (true when the input is odd), the pattern

    [ "x", "=", ["?and", ["?is", "?n", numberp], ["?is","?n", oddp]]]

when matched against `["x","=",3]` returns

    {'?n': 3}

but when matched against `["x","=",10]` returns `None`.


## Outline

Pattern:
* Variable
* Constant
* Single pattern
* Segment pattern (TBD)

Single pattern:
* `[["?is", var] ...]`
* `?or`
* `?and`
* `?not`

Segment pattern:
* `[["?*", var] ...]`


## Links

* Github Repository
* ReplIt main project
