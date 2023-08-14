# Ch.2.2 & Optional Courses

## Ch.2.2

The general technique of isolating the parts of a program that deal with how data are represented from the parts that deal with how data are manipulated is a powerful design methodology called *data abstraction*.

### Example: Rational Numbers

A rational number is a ratio of integers, and rational numbers constitute an important sub-class of real numbers. A rational number such as `1/3` or `17/29` is typically written as:

```python
<numerator>/<denominator>
```

where both the `<numerator>` and `<denominator>` are placeholders for integer values.

Actually dividing integers produces a `float` approximation, losing the exact precision of integers. However, we can create an exact representation for rational numbers by combining together the numerator and denominator.

- `rational(n, d)` returns the rational number with numerator `n` and denominator `d`.
- `numer(x)` returns the numerator of the rational number `x`.
- `denom(x)` returns the denominator of the rational number `x`.

We are using here a powerful strategy for designing programs: *wishful thinking*. We haven't yet said how a rational number is represented, or how the functions `numer`, `denom`, and `rational` should be implemented. Even so, if we did define these three functions, we could then add, multiply, print, and test equality of rational numbers:

```python
>>> def add_rationals(x, y):
        nx, dx = numer(x), denom(x)
        ny, dy = numer(y), denom(y)
        return rational(nx * dy + ny * dx, dx * dy)
>>> def mul_rationals(x, y):
        return rational(numer(x) * numer(y), denom(x) * denom(y))
>>> def print_rational(x):
        print(numer(x), '/', denom(x))
>>> def rationals_are_equal(x, y):
        return numer(x) * denom(y) == numer(y) * denom(x)
```

Now we have the operations on rational numbers defined in terms of the selector functions `numer` and `denom`, and the constructor function `rational`, but we haven't yet defined these functions. What we need is some way to glue together a numerator and a denominator into a compound value.

### Pairs

Unpack a list into its elements and binds each element to a different name

```python
>>> pair = [10, 20]
>>> x, y = pair
>>> x
10
>>> y
20
```

Two-element lists are not the only method of representing pairs in Python. Any way of bundling two values together into one can be considered a pair. Lists are a common method to do so.

We can now represent a rational number as a pair of two integers: a numerator and a denominator.

```python
>>> from fractions import gcd
>>> def rational(n, d):
        g = gcd(n, d)
        return (n//g, d//g)
>>> def numer(x):
        return x[0]
>>> def denom(x):
        return x[1]
```

Together with the arithmetic operations we defined earlier, we can manipulate rational numbers with the functions we have defined.

This improvement was accomplished by changing the constructor without changing any of the functions that implement the actual arithmetic operations.

### Abstraction Barriers

An abstraction barrier violation occurs whenever a part of the program that can use a higher level function instead uses a function in a lower level. For example, a function that computes the square of a rational number is best implemented in terms of `mul_rational`, which does not assume anything about the implementation of a rational number.

```python
>>> def square_rational(x):
        return mul_rational(x, x)
```

Referring directly to numerators and denominators would violate one abstraction barrier.

```python
>>> def square_rational_violating_once(x):
        return rational(numer(x) * numer(x), denom(x) * denom(x))
```

Assuming that rationals are represented as two-element lists would violate two abstraction barriers.

```python
>>> def square_rational_violating_twice(x):
        return [x[0] * x[0], x[1] * x[1]]
```

Abstraction barriers make programs easier to maintain and to modify. The fewer functions that depend on a particular representation, the fewer changes are required when one wants to change that representation. All of these implementations of `square_rational` have the correct behavior, but only the first is robust to future changes. The `square_rational` function would not require updating even if we altered the representation of rational numbers. By contrast, `square_rational_violating_once` would need to be changed whenever the selector or constructor signatures changed, and `square_rational_violating_twice` would require updating whenever the implementation of rational numbers changed.

### The Properties of Data

Abstraction barriers shape the way in which we think about data. A valid representation of a rational number is not restricted to any particular implementation (such as a two-element list); it is a value returned by `rational` that can be passed to `numer`, and `denom`. In addition, the appropriate relationship must hold among the constructor and selectors. That is, if we construct a rational number `x` from integers `n` and `d`, then it should be the case that `numer(x)/denom(x)` is equal to `n/d`.

This point of view can be applied broadly, including to the pair values that we used to implement rational numbers. We never actually said much about what a pair was, only that the language supplied the means to create and manipulate lists with two elements. The behavior we require to implement a pair is that it glues two values together. Stated as a behavior condition,

- If a pair `p` was constructed from values `x` and `y`, then `select(p, 0)` returns `x`, and `select(p, 1)` returns `y`.

We don't actually need the `list` type to create pairs. Instead, we can implement two functions `pair` and `select` that fulfill this description just as well as a two-element list.

```python
>>> def pair(x, y):
        """Return a function that represents a pair."""
        def get(index):
            if index == 0:
                return x
            elif index == 1:
                return y
        return get
>>> def select(p, i):
        """Return the element at index i of pair p."""
        return p(i)
```

With this implementation, we can create and manipulate pairs.

```python
>>> p = pair(20, 14)
>>> select(p, 0)
20
>>> select(p, 1)
14
```

## Optional Courses

### Four Steps to Draw Your Own Circuit

1. Draw a truth-table with all possible inputs & outputs
2. Build sub-expressions for each output column only with and/not
3. Combine, two at a time,sub-expression with an or
4. Draw circuit diagram
