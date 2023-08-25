# SymPEP X — Adopting Static Typing in SymPy

**Author:** Sangyub Lee

**Status:** Draft

**Type:** Standards Track

**Created:** 2023-08-25

**Resolution:** [Link to Discussion]

## Abstract

This SymPEP proposes the adoption of static typing in the SymPy codebase. Static typing can enhance the development experience, improve code quality, and facilitate better collaboration among contributors.

## Motivation and Scope

The current SymPy codebase is predominantly dynamically typed, which can lead to runtime errors and make it harder to understand the codebase. The adoption of static typing will provide better tooling support, catch type-related errors at compile-time, and improve the overall robustness of the library. This SymPEP aims to define the scope and guidelines for introducing static typing to SymPy.

## Usage and Impact

With static typing, users of SymPy will benefit from improved code suggestions, better error messages, and increased confidence in the correctness of their code. For instance, when working with symbolic expressions, the static type system can help catch potential issues in function calls, attribute accesses, and type mismatches. This will lead to a more intuitive and reliable programming experience for SymPy users.

For example, the functions can be static typed as follows:

```python
from typing import List
from sympy import Expr, Symbol

def differentiate(expr: Expr, var: Symbol) -> Expr:
    """
    Differentiate a SymPy expression with respect to a variable.
    """
    return expr.diff(var)

def simplify_expressions(expressions: List[Expr]) -> List[Expr]:
    """
    Simplify a list of SymPy expressions.
    """
    return [expr.simplify() for expr in expressions]
```

which is better than the dynamically typed versions:

```python
def differentiate(expr, var):
    """
    Differentiate a SymPy expression with respect to a variable.
    """
    return expr.diff(var)

def simplify_expressions(expressions):
    """
    Simplify a list of SymPy expressions.
    """
    return [expr.simplify() for expr in expressions]
```

which is not friendly for users because they can simplify expressions with a list of integers or strings. The static typed version can catch this error at compile-time.


```python
from sympy import Expr, Symbol

def differentiate(expr, var):
    """
    Differentiate a SymPy expression with respect to a variable.
    """
    if isinstance(expr, Expr) and isinstance(var, Symbol):
        return expr.diff(var)
    else:
        raise TypeError("expr must be a SymPy expression and var must be a SymPy symbol")

def simplify_expressions(expressions):
    """
    Simplify a list of SymPy expressions.
    """
    if isinstance(expressions, list):
        if all(isinstance(expr, Expr) for expr in expressions):
            return [expr.simplify() for expr in expressions]
    else:
        raise TypeError("expressions must be a list of SymPy expressions")
```

which is more verbose and less efficient than the static typed versions, because of added runtime type checks.

## Backwards Compatibility

Introducing static typing to SymPy may break backwards compatibility for code that relies on dynamically typed behavior. Users who heavily depend on runtime type inference might need to update their code to match the new type annotations.

## Detailed Description

This SymPEP proposes the gradual introduction of static type annotations using tools like Python's `typing` module and third-party type checkers such as `mypy` or `pyright`. The process will involve identifying critical modules, functions, and classes to begin the static typing integration. We will define guidelines for annotating function signatures, class attributes, and return types. The goal is to maintain compatibility with existing dynamically typed code while allowing for a smooth transition.

## Implementation

1. Identify key modules for static typing.
2. Start by adding type annotations to function signatures and class attributes.
3. Use `mypy` or `pyright` to perform static type checking and address any issues.
4. Gradually propagate static typing to dependent modules and functions.
5. Update documentation to reflect the new static typing conventions.
6. Use `mypy` or `pyright` with strict mode.

## Alternatives

An alternative approach would be to maintain the status quo of dynamic typing. However, this could lead to ongoing challenges in maintaining code quality and preventing runtime errors, especially as the SymPy codebase continues to evolve.

## Discussion

- [Link to Mailing List Discussion]

## References

- [Python Typing Documentation](https://docs.python.org/3/library/typing.html)

## Copyright

This document has been placed in the public domain.
