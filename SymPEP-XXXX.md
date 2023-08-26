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

Authors of new classes, functions, or modules in SymPy should be encouraged to write their code with static typing, unless they encounter a situation where achieving this is difficult or not possible. In such cases, they are expected to include a comment explaining the reasons preventing static typing.

Introducing new dynamically typed code initially can lead to the accumulation of technical debt. It's recognized that migrating existing dynamically typed code later is significantly more challenging than initially writing code with static typing. Therefore, introducing new dynamically typed code should be approached cautiously.

It's important to note that tools like mypy and pyright are capable of inferring types, facilitating the incorporation of static typing without a steep learning curve. By adding a few annotations, code can be made cleaner and the transition to static typing across the entire codebase can be eased.

For functions, classes, and modules internally used by SymPy that currently feature unnecessary dynamic type checks, a shift towards static typing should be promoted. This transition can help eliminate these unnecessary checks and subsequently enhance the overall performance of the SymPy codebase.

Similarly, functions, classes, and modules within SymPy that intentionally avoided runtime type checks for performance reasons should consider embracing static typing. Static typing undoubtedly mitigates performance overhead while providing better bug detection related to type errors.

However, in the case of core and public functions and classes, such as ``sympify``, ``simplify``, or ``lambdify``, which have historically relied on dynamic typing for an extended period, the introduction of static typing should be approached judiciously. It should be implemented for these core components only if it doesn't disrupt backward compatibility.

It's worth noting that SymPy has devoted considerable effort over time to address type-related issues of Python objects within SymPy. This was especially relevant during periods when the Python type system was less mature or lacked type hinting capabilities.

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

- [Look into using type hints](https://github.com/sympy/sympy/issues/17945)

## References

- [Python Typing Documentation](https://docs.python.org/3/library/typing.html)
- [PEP-484](https://peps.python.org/pep-0484/)
- [MyPy Documentation](https://mypy.readthedocs.io/en/stable/)
- [Pyright Documentation](https://microsoft.github.io/pyright/)

## Copyright

This document has been placed in the public domain.
