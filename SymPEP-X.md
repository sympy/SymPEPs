# SymPEP X — Adoption of Final Qualifier in SymPy Classes

**Author:** Sangyub Lee

**Status:** Draft

**Type:** Standards Track

**Created:** 2023-08-25

**Resolution:** [Link to Discussion]

## Abstract

This SymPEP proposes the adoption of the `final` qualifier in appropriate SymPy classes to enhance code design, maintainability, and performance. The `final` qualifier restricts subclassing of certain classes, promoting better code structure and reducing potential sources of bugs and inefficiencies.

## Motivation and Scope

In object-oriented programming, the `final` keyword is used to indicate that a class or method cannot be subclassed or overridden in subclasses. Introducing the `final` qualifier in selected SymPy classes will provide several benefits:

- Prevent unintentional subclassing that could lead to fragile inheritance hierarchies.
- Enable the SymPy developers to better control the behavior and design of critical classes.

The scope of this proposal includes identifying classes in SymPy that should be marked as `final` and adding the `final` keyword to their class definitions.

## Usage and Impact

By marking certain classes as `final`, the SymPy developers will signal that these classes are intended to be used as-is and not extended. This will guide users and contributors to understand the intended usage of these classes and avoid subclassing when it's not appropriate. This change will have a positive impact on code stability and reliability, as well as the overall maintenance of the library.

For example:

```python
from typing import final

class Basic:
    @final
    def doit(self, deep=True):
        if deep:
            expr = self.func(*(arg.doit(deep=deep) for arg in args))
            return expr._eval_doit()
        return self._eval_doit()

    def _eval_doit(self):
        return self
```

In this scenario, the `Basic` class is marked as `final` to prevent subclasses from overriding the `doit` method. This is because the `doit` method is designed to recursively call itself, and overriding this method in subclasses could break the recursion and lead to unexpected behavior.

## Backwards Compatibility

Adding the final qualifier to existing classes may break backward compatibility for users who were relying on subclassing those classes. However, this change can be introduced gradually, and the benefits of improved code design and maintainability outweigh the potential compatibility concerns.

## Detailed Description

The implementation will involve the following steps:

1. Identify classes within SymPy that are suitable candidates for the final qualifier.
2. Add the final keyword to the class definitions of the identified classes.
3. Update the documentation to highlight the classes that are now marked as final.
4. Classes that are candidates for the final qualifier are those that are not designed to be subclassed and are crucial to the core functionality of SymPy.

## Alternatives

An alternative approach would be to rely solely on documentation and conventions to discourage subclassing and overriding, without using the final qualifier. However, relying solely on documentation can lead to misunderstandings and unintended subclassing and overriding.

## Discussion

- [Link to Mailing List Discussion]

## References

- [PEP 591](https://peps.python.org/pep-0591/)

## Copyright

This document has been placed in the public domain.
