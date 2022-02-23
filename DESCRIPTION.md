# Pattern Matcher

In this project we will build a pattern matcher "by hand". We will not be using
existing libraries like regular expression matching.

## Examples

The pattern "x = (?is ?n numberp)" from [PAIP 6.2](https://github.com/norvig/paip-lisp/blob/main/docs/chapter6.md)
will get written in Python as:

	[ "x", "=", ["?is", "?n", numberp] ]

Notice that the `numberp` is a function in a list, not a string.

	def numberp(x): return type(x) in [int, float]

### Example 1

Here is a short table of the results of matching this pattern:

| target            | match result    |
|-------------------|-----------------|
| `["x", "=", 36]`  | `{ '?n' : 36 }` |
| `["y", "=", 48]`  | `None`          |
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

### Example 4

Variables should be required to have the same value throughout the match.
The pattern

	["?x", "good", "and", "?x", "wins"]

should match

	["pizza", "good", "and", "pizza", "wins"]

but not

	["pizza", "good", "and", "pasta", "wins"]

## Segment Patterns

A segment pattern is a created by a list within a list.
A segment pattern can match a variable number of items.

### Example 5

The `?if` matcher takes in a function and then 0 or more variables to
use as the arguments to the function. In first example below, the
function `sum_is_six(5,1)` is called.

```python
def sum_is_six(a,b): return a+b==6
pat = ["?x","plus","?y","=",6,["?if",sum_is_six,"?x","?y"]]
tgt = [5,"plus",1,"=",6] # match
tgt = [7,"plus",2,"=",6] # no match
```

Unlike some matchers, we are _not_ going to attempt to allow functions
to be called by matching.

	# NOT EXPECTED TO WORK
	pat = ["?operation","on","?y","is","true",["?if","?operation","?y"]]
	tgt = ["numberp","on",5,"is","true"]

Python skills: in order to get this to work you need to know how to
[unpack a list into arguments to a function](https://docs.python.org/3/tutorial/controlflow.html#tut-unpacking-arguments).

	args = [5,1]
	if sum_is_six(*args) then: print("GOOD")


### Example 6

There are three related repetition matchers: `??` (0 or 1 match), `?*`
(0 or more matches), and `?+` (1 or more match).

    pat = [["??","A"],"A","B"]
    tgt1 = ["A","B"]  # yes
    tgt2 = ["A","A","B"] # yes
    tgt3 = ["A","A","A","B"] # no
    tgt4 = ["B","A"] # no
    tgt5 = ["B","A","B"] # no, must match whole pattern

## Example 7

The 0 or more matches pattern `?*` should be a small step above `??`.

    pat = [["?*","A"],"B"]
    # Matches tgt1, tgt2, tgt3 above and also
    tgt6 = ["B"]


## Outline

Pattern:
* Variable
* Constant
* Single pattern
* Segment pattern (TBD)

Single pattern:
* `[["?is", var, predicate] ...]`: two things
  * (1) bind the variable - is this necessary? - what happens if the variable is already bound? - and
  * (2) test the binding
* `?or`
* `?and`
* `?not`

Segment pattern:
* `[["?*", var] ...]`


## Links

* Github Repository
* ReplIt main project
