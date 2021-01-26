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
_Functions (sin, cos, exp, etc...)_
* Functions will apply to both sides.
```
>>> cos(eq1)
cos(a) = cos(b/c)
```
* Keywords and additional parameters will be passed through to the function.

_Simplification_
* Behavior should mimic the behavior for expressions and default to applying to both sides.

_Differentiation_

_Integration_
* Support integration of each side through `.doXXX.integrate(variable-of-integration)`
and `.doXXX.Integral(variable-of-integration)`, where `XXX` is either `rhs` or `lhs`.

Integration of each side with respect to different variables
```
>>> eq1.dorhs.integrate(b).dolhs.integrate(a)
a**2/2 = b**2/(2*c)
>>> eq1.dorhs.integrate(b).dolhs.Integral(a)
Integral(a,a) = b**2/(2*c)
```

_Solve_
* Solve should accept equations and lists of equations and return equations or lists of
equations as solutions. To avoid breaking past behavior this should only occur when solve
is used on equations or lists of equations.

_Applications of methods_
* Support `.method()` calls for any method that applies to expressions. Apply the method
to both sides of the equation.
* Support `.apply()`, `.applylhs()`, `.applyrhs()`, `.do.`, `.dolhs.`,`.dorhs.`.
* Methods that can be applied to a SymPy expression as a Python method should also work
on an Equation:
```
>>> collect(a*x + b*x**2 + c*x, x)
(a + c)*x + b*x**2
>>> collect(Equation(a*x + b*x**2 + c*x,a*x**2 + b*x + c*x, x)
(a + c)*x + b*x**2 = a*x**2 + (b + c)*x
```
_Miscellaneous_
* `.as_Boolean()` returns the equation cast as an `Equality`.
* `.lhs` returns the expression on the left hand side.
* `.rhs` returns the expression on the right hand side.
* `.swap` returns the equation with the lhs and rhs swapped.
* `.check()` shortcut for `.as_Boolean().simplify()`.
