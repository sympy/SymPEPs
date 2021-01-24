SymPEP for behavior of the Equation class
=========================================
Fundamentally this is an object with two SymPy expressions connected by an equals sign.
Potentially this could be extended to a general relational expression, but that is not
part of this proposal.

The initial motivation for this class is to facilitate manipulating mathematical expressions
in a manner as similar to what is done on paper as possible. For operations/methods where
it makes sense they should be applied to both sides of the equation. As much as possible,
operations on the equations should be initiated using syntax mimicking standard
mathematical notation.

__Proposed Behaviors__
_Binary Operations (+,-, *, /, %, **)_
* If combining an equation with a SymPy expression the expression will be applied to both
sides of the equation as specified by the binary operator.
```
>>> a, b, c = Symbol('a b c')
>>> eq1 = Equation(a, b/c)
>>> eq1
a = b/c
>>> eq1*c
a*c = b
```
* If combining two equations the lhs will combine with the lhs and the rhs with the rhs.
```
>>> eq2 = Equation(b, c**2)
>>> eq2
b = c**2
>>> eq1 + eq2
a + b = b/c + c**2
```
