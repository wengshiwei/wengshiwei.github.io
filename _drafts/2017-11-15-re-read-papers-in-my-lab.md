The reason to re-read DDPA papers is more or less due to I felt I am not familiar (and confident) enough to explain to guys who don't do (high-order) (demand-driven) program analysis as well as guys who just do this area e.g. "what's your lab doing" "what are you doing" "what's the hard and novel part of your work". It seems sometimes I have to start from scratch.

Sometimes we mention `static analysis` using "not execute the program". My slight disagreement on the terms is whether in `forward analysis` or `demand-driven analysis`, it actually "runs"(abstract interprets) the program under a different semantics(interpreter). Borrowing the concept `oracle` from cryptography and imaging an oracle knowing all truths of the program. If the oracle know something just from the program, our analysis should know. (Should the oracle obey `Rice's theorem`?). If some properties are determined at run-time, even the oracle can never know, nor do us.



I do this because I skim through slides from [Abstract Interpretation Winter School], which Scott recommended to me before (Not exercises, may do some in the coming holiday)

I also read some materials from `big-bang-doc` in our repo. The slides for SAS 2017 is helpful.

The aim to introduce `PDS` is for call-return alignment and the efficiency of look-up. Some forward analysis also use `PDA` but not the same purpose here.









Assume we have compiled `module arith` before, it seems having to put contract checks inside `add`. Given `module main`, before running it, oracle knows it will trigger contract violation. The point here is if we still know source code from the compiled module, it behaves no difference to (un-compiled) source code. The downside is I am not sure is it feasible: either you need the source code (which means the previous compiling is useless) or you have to re-compile (which means the previous compiling is useless).

```Python
# Example 3

# module arith:
def add(x: int, y: int): positive int
    return x + y

# module main:
    x = user_input_int()
    y = user_input_int()
    z = add(x, y)
```

In this case oracle can only remove the `int` checks for `x` `y` `x+y`, but it has to check `x+y` is `positive`.

```Python
# Example 4 - high order

# module arith:
def apply(f: int int -> int, x: int, y: int): int
    return f(x, y)

def add(x: int, y: int): positive int
    return x + y

# module main:
    x = user_input_int()
    y = user_input_int()
    z = apply (add x y)
```

The general idea is the same to high-order function. If we know the `module main`, we can do lots of optimization ahead of run-time

```Python
# Example 5 - high order

# module arith:
def apply(f, x, y)
    return f(x, y)

def add(x, y): positive # only positive need to check
    return x + y

# module main:
    x = user_input_int()
    y = user_input_int()
    z = apply (add x y)
```
