# Chapter 1

## Expressions

- Expressions
- Call Expressions

## Name

Name -- *bind* -- object

The name of a function is repeated twice, once in the frame and again as part of the function itself. The name appearing in the function is called the *intrinsic name*. The name in a frame is a *bound name*. 

There is a difference between the two: different names may refer to the same function, but that function itself has only one intrinsic name.

## Environment

The possibility of binding names to values and later retrieving those values by name means that the interpreter must maintain some sort of memory that keeps track of the names, values, and bindings. This memory is called an *environment*.

An environment in which an expression is evaluated consists of a sequence of *frames*.

 Each frame contains *bindings*, each of which associates a name with its corresponding value.

There is a single *global* frame. 

![environment-frame-1](./environment-frame-1.png)

Assignment and import statements add entries to the first frame of the current environment. 

## Evaluating Nested Expressions

*expression tree*: 

![expression_tree](./expression_tree.png)

## The Non-Pure Print Function

In addition to returning a value, applying a non-pure function can generate *side effects*, which make some change to the state of the interpreter or computer. A common side effect is to generate additional output beyond the return value, using the `print` function.

````python
>>> print(print(1), print(2))
1
2
None None
````

## Calling User-Defined Functions

To evaluate a call expression whose operator names a user-defined function, the Python interpreter follows a computational process. As with any call expression, the interpreter evaluates the operator and operand expressions, and then applies the named function to the resulting arguments.

Applying a user-defined function introduces a second *local* frame, which is only accessible to that function. To apply a user-defined function to some arguments:

1. Bind the arguments to the names of the function's formal parameters in a new *local* frame.
2. Execute the body of the function in the environment that starts with this frame.

The environment in which the body is evaluated consists of two frames: first the local frame that contains formal parameter bindings, then the global frame that contains everything else. Each instance of a function application has its own independent local frame.

![local-names-in-func](./local-names-in-func.png)

## Functions as Abstractions

**Aspects of a functional abstraction.** To master the use of a functional abstraction, it is often useful to consider its three core attributes. The *domain* of a function is the set of arguments it can take. The *range* of a function is the set of values it can return. The *intent* of a function is the relationship it computes between inputs and output (as well as any side effects it might generate). Understanding functional abstractions via their domain, range, and intent is critical to using them correctly in a complex program.

For example, any `square` function that we use to implement `sum_squares` should have these attributes:

- The *domain* is any single real number.
- The *range* is any non-negative real number.
- The *intent* is that the output is the square of the input.
