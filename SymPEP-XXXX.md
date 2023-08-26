# SymPEP X — Adopting Static Typing in SymPy

**Author:** Sangyub Lee

**Status:** Draft

**Type:** Standards Track

**Created:** 2023-08-25

**Resolution:** `https://github.com/sympy/SymPEPs/pull/4`

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

There are no backwards compatibility issues with this SymPEP, for users
who don't use the type checkers like ``mypy`` or ``pyright``. However, for users who use the type checkers, they need to update their code to match the new type annotations.

## Detailed Description

This SymPEP proposes the gradual introduction of static type annotations using tools like Python's typing module and third-party type checkers such as ``mypy`` or ``pyright``. The process will involve identifying critical modules, functions, and classes to initiate the integration of static typing. Guidelines will be established for annotating function signatures, class attributes, and return types. The objective is to maintain compatibility with existing dynamically typed code while facilitating a seamless transition.

Authors of new classes, functions, or modules in SymPy should be encouraged to provide the ``.pyi`` for their implementations, unless they encounter a situation where achieving this is difficult or not possible.

It's important to note that tools like ``mypy`` and ``pyright`` are capable of inferring types, which simplifies the process of incorporating static typing without requiring a steep learning curve. By adding a few annotations, the code can be enhanced in terms of clarity, facilitating the transition to static typing across the entire codebase.

Encouraging the SymPy community to collaborate on creating ``.pyi`` files to provide type stubs for all exported functions and classes in SymPy is recommended for fostering public access to type information.

Although the core CPython is inherently dynamically typed, nearly all core CPython standard libraries are equipped with type annotations. Similarly, popular third-party libraries like ``numpy``, ``scipy``, or ``pandas`` provide users with type annotations.

SymPy holds a pivotal role as a library used by many other libraries and stands at the forefront of the supply chain of scientific computing libraries. However, it currently lags behind technical standards by lacking comprehensive type annotations.

## Implementation

1. Identify Key Modules for Static Typing

   Identify the modules or components in your codebase that are essential candidates for static typing. These could include frequently used public functions, classes, and methods.

2. Use `.pyi` Stub Files

   Stub files (`.pyi` files) are separate files that contain type hints for external modules or libraries that isn't implemented with static typing. These files serve as references for static type checking tools like mypy or pyright and enable them to understand the types and interfaces of external code.

   Create a .pyi stub file for each SymPy modules by naming it with the same name as the module you're stubbing, and place it in the same directory as your code. For example, if you're stubbing the `prime` module, create a file named `prime.pyi`.

3. Start by Adding Type Annotations

   Begin by adding type annotations to function signatures and class attributes. For functions, specify the argument types and return type using the `->` syntax. For classes, annotate attributes in the `__init__` method and methods within the class.

   ```python
   # Original code
   def add(a, b):
       ...

   # Annotated with type hints
   def add(a: Integer, b: Integer) -> Integer:
       ...
   ```

   ```python
   # Original class
    class Circle:
        def __init__(self, center, radius):
            ...

   # Annotated with type hints
    class Circle:
        def __init__(self, center: Point2D, radius: Expr):
            ...
   ```

4. Gradually Propagate Static Typing

   Gradually add type annotations to dependent modules and functions. As you do this, ensure that you're not only annotating your code but also updating any function calls or method invocations to adhere to the new type annotations.

5. Update Documentation

   As you add type annotations, remember to update your code documentation to reflect the new static typing conventions. This will help other developers understand the expected types and enhance the overall clarity of your codebase.

6. Use Strict Mode with mypy or pyright

   Both mypy and pyright offer a strict mode that enforces more comprehensive type checking. Enable strict mode in your static type checking tool to catch even subtle type-related issues.

Remember that static typing is an iterative process. Start with the most critical parts of your codebase, or most simple and easiest parts of the codebase, and gradually expand the coverage as you become more comfortable with the process. This approach helps maintain a balance between improving code quality and avoiding overwhelming upfront changes.

## Alternatives

### Keeping the status quo of static typing

An alternative perspective is to maintain the current dynamic typing approach. However, this decision could potentially give rise to persistent challenges in upholding code quality and mitigating runtime errors, particularly as the SymPy codebase continues to evolve.

### Conservatism and Community Implications

To conserve about not using any type hints in the SymPy, additional concern regarding its impact on the SymPy community. By adhering strictly to dynamic typing and favoring duck typing, there's a risk of creating a division between users who prefer static typing for its benefits and those who are more inclined towards the existing syntax. This division might inadvertently isolate those who seek the advantages of static typing and potentially alienate them from the community. Consequently, this could lead to a situation where users start exploring alternative of SymPy instead of contributing to SymPy's improvement.

### Challenges of Comprehensive Annotation

Another approach to consider is fully annotating the entire SymPy codebase with inline type hints. While this strategy offers a comprehensive solution, it's worth noting that it presents significant technical challenges. Certain SymPy functions, like ``solve``, inherently defy easy expression through type annotations due to their complexity. Attempting to annotate these functions might introduce more confusion than clarity, making this approach less feasible in practice.

### The Role of .pyi Stubs

In light of the aforementioned considerations, ``.pyi`` stub files emerge as an appealing middle ground to introduce static typing to the core of SymPy. The advantage of this approach lies in its non-intrusive nature. It doesn't necessitate altering the existing codebase, and it has no impact on the documentation. This means that the project can gradually adopt static typing without undergoing immediate massive changes, striking a balance between the advantages of static typing and the project's existing dynamics.

### Verification and Community Management

It's important to acknowledge that while ``.pyi`` files offer a promising route, they are not without their challenges. There's a potential for these stub files to define unsound types that don't accurately reflect the implementation. This underscores the need for human reviewers to meticulously assess their correctness ensure their alignment with the project's implementation. The community's involvement in managing these ``.pyi`` files becomes crucial for maintaining the integrity of the type information and promoting accurate static type checking.

### Using `S` for SymPy objects

LPython is currently engaged in the development of a Python compiler. This compiler harnesses the benefits of statically typed functions, classes, and symbolic expressions found in SymPy.

To enhance collaboration and foster compatibility, there's a proposal to initiate a discussion regarding the establishment of type annotations as the standardized technical interface for interacting with LPython. This approach would involve soliciting input not only from the SymPy community but also potentially from external stakeholders, including 3rd party computer algebra systems and compilers. Rather than making internal decisions and implementing changes solely within the SymPy community, this effort aims to gather broader perspectives and create a consensus-driven approach to drive the evolution of LPython and its interface standards.

## Discussion

- [Look into using type hints](https://github.com/sympy/sympy/issues/17945)

## References

- [Python Typing Documentation](https://docs.python.org/3/library/typing.html)
- [PEP-484](https://peps.python.org/pep-0484/)
- [PEP-561](https://peps.python.org/pep-0561/)
- [MyPy Documentation](https://mypy.readthedocs.io/en/stable/)
- [Pyright Documentation](https://microsoft.github.io/pyright/)

## Copyright

This document has been placed in the public domain.
