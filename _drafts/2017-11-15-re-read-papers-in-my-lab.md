I re-read the three `DDPA`-related papers (Higher-Order DDPA, Implementing Higher-Order DDPA and DRSF).

One of the "scary" things is Leandro told me it took him one year to thoroughly understand some Matt Might's papers.

I skim through slides from [Abstract Interpretation Winter School], which Scott recommended to me before. (Not exercises, may do some in the coming holiday)

Sometimes we mention `static analysis` using "not execute the program". My slight disagreement on the terms is whether in `abstract interpretation` or `demand-driven`, it actually "runs" the program under a different semantics(interpreter). Borrowing a concept from cryptography and imaging an oracle knowing all truths of the program. The truths also exclude properties which are determined in run-time.




The type constrain is implemented in contracts.

```Python
# Example 1
def add(x: int, y: int): positive int
    return x + y

z1 = add( 1, 2)
z2 = add(-1, 2)
```

The oracle know constrains in `add` must satisfy. So we hope our analysis can infer this and we can remove the contract check in run-time. In some scenarios, we, even the oracle, don't what the run-time code will be, e.g. `module add` is compiled separate or we load code in run-time (`eval "some_code_in_string"`)

```Python
# Example 2

# module arith (compiled):
def add(x: int, y: int): positive int
    return x + y

# module main:
z1 = add(1,  2)
z3 = add(1, -2) # contract violation
```

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
