SymPEP XXXX Equation class
=========================================

**Author** Jonathan Gutow, <gutow@uwosh.edu>

**Status** Draft

**Type** Standards Track

**Created** 2021-01-25

**Resolution** url to discussion (required for Accepted | Rejected | Withdrawn)


## Abstract

Fundamentally this is an object with two SymPy expressions connected by an equals sign.
Potentially this could be extended to a general relational expression, but that is not
part of this proposal. This class facilitates manipulating mathematical expressions
in a manner as similar to what is done on paper as possible. For operations/methods where
it makes sense they would be applied to both sides of the equation. As much as possible,
operations on the equations should be initiated using syntax mimicking standard
mathematical notation. This class also aims to provide output in standard mathematical
notation when used within tools such as Jupyter, that support typeset LaTex.

## Motivation and Scope
The original impetus for development of the `Equation` class came from discussions J. Gutow 
had with Physical Science Educators using SymPy in their classes. They wanted this on-paper-like
behavior to facilitate demonstrating algebraic manipulations in physical science classes.

There are a number of specific motivations for the development of the `Equation` class:
1. Make interactive manipulation of equations as similar to doing the work on paper as
possible and make the results easier to read by having results that look like
`x = 2*y + 1` rather than `2*y + 1`, with what it is equal to on a separate line.
1. Have equations on which manual algebra can be done by having SymPy perform the actual
symbolic manipulation. This allows people to perform messy manipulations with fewer errors, much
as calculators, spreadsheets and other numerical computation tools facilitate arithmetic.
1. From an educational and new user perspective this increases the ease of use of SymPy
by decreasing the cognitive load of using SymPy. This would be acheived by maintaining as
close to standard mathematical notation for operations as possible (see 
[Detailed Description](#detailed-description)). Making SymPy easier to begin using will
increase its adoption and encourage people to learn to use symbolic math tools.
1. The `Equality` class does not reliably support general SymPy operations on both sides
of the equality without collapsing to a Boolean value of `True` or `False`. Parts of SymPy
depend on this Boolean behavior; thus this proposal for a separate `Equation` class rather
than an extension of `Equality`.
1. Currently cycling between expressions and the results of the `solve` operation is quite
messy. This proposal includes extending the `solve` operation so that if passed an equation
or set of equations it would return the solutions as a set of equations.
1. `.subs()` could also be made more natural to use if it could take a list of equations.
1. As an equation with an `=` connecting the two sides can be thought of as a specific 
example of a general relation (`=`, `>`, `<`, etc...) as much as possible the `Equation`
class should draw upon the underlying logic in the `relational` class.

## Usage and Impact

The class would have the name `Equation` and `Eqn` would be a synonym. The expectation is
that this would be used primarily interactively for symbolic algebra. Some examples
(__Note__ that for illustration purposes results of operations are written with the two
sides connected by `=`, however this will only be the case in environments with Latex
output):
```
>>> a, b, c, x = var('a b c x')
>>> Equation(a,b/c)
a = b/c
>>> t=Eqn(a,b/c)
>>> t
a = b/c
>>> t*c
a*c = b
>>> c*t
a*c = b
>>> exp(t)
exp(a) = exp(b/c)
>>> exp(log(t))
a = b/c

Simplification and Expansion
>>> f = Eqn(x**2 - 1, c)
>>> f
x**2 - 1 = c
>>> f/(x+1)
(x**2 - 1)/(x + 1) = c/(x + 1)
>>> (f/(x+1)).simplify()
x - 1 = c/(x + 1)
>>> simplify(f/(x+1))
x - 1 = c/(x + 1)
>>> (f/(x+1)).expand()
x**2/(x + 1) - 1/(x + 1) = c/(x + 1)
>>> expand(f/(x+1))
x**2/(x + 1) - 1/(x + 1) = c/(x + 1)
>>> factor(f)
(x - 1)*(x + 1) = c
>>> f.factor()
(x - 1)*(x + 1) = c
>>> f2 = f+a*x**2+b*x +c
>>> f2
a*x**2 + b*x + c + x**2 - 1 = a*x**2 + b*x + 2*c
>>> collect(f2,x)
b*x + c + x**2*(a + 1) - 1 = a*x**2 + b*x + 2*c

Apply operation to only one side
>>> poly = Eqn(a*x**2 + b*x + c*x**2, a*x**3 + b*x**3 + c*x)
>>> poly.applyrhs(factor,x)
a*x**2 + b*x + c*x**2 = x*(c + x**2*(a + b))
>>> poly.applylhs(factor)
x*(a*x + b + c*x) = a*x**3 + b*x**3 + c*x
>>> poly.applylhs(collect,x)
b*x + x**2*(a + c) = a*x**3 + b*x**3 + c*x

``.apply...`` also works with user defined python functions
>>> def addsquare(eqn):
...     return eqn+eqn**2
...
>>> t.apply(addsquare)
a**2 + a = b**2/c**2 + b/c
>>> t.applyrhs(addsquare)
a = b**2/c**2 + b/c
>>> t.apply(addsquare, side = 'rhs')
a = b**2/c**2 + b/c
>>> t.applylhs(addsquare)
a**2 + a = b/c
>>> addsquare(t)
a**2 + a = b**2/c**2 + b/c

Inaddition to ``.apply...`` there is also the less general ``.do``,
``.dolhs``, ``.dorhs``, which only works for operations defined on the
``Expr`` class (e.g.``.collect(), .factor(), .expand()``, etc...).
>>> poly.dolhs.collect(x)
b*x + x**2*(a + c) = a*x**3 + b*x**3 + c*x
>>> poly.dorhs.collect(x)
a*x**2 + b*x + c*x**2 = c*x + x**3*(a + b)
>>> poly.do.collect(x)
b*x + x**2*(a + c) = c*x + x**3*(a + b)
>>> poly.dorhs.factor()
a*x**2 + b*x + c*x**2 = x*(a*x**2 + b*x**2 + c)

``poly.do.exp()`` or other sympy math functions will raise an error.

Rearranging an equation (simple example made complicated as illustration)
>>> p, V, n, R, T = var('p V n R T')
>>> eq1=Eqn(p*V,n*R*T)
>>> eq1
V*p = R*T*n
>>> eq2 =eq1/V
>>> eq2
p = R*T*n/V
>>> eq3 = eq2/R/T
>>> eq3
p/(R*T) = n/V
>>> eq4 = eq3*R/p
>>> eq4
1/T = R*n/(V*p)
>>> 1/eq4
T = V*p/(R*n)
>>> eq5 = 1/eq4 - T
>>> eq5
0 = -T + V*p/(R*n)

Substitution (#'s and units)
>>> L, atm, mol, K = var('L atm mol K', positive=True, real=True) # units
>>> eq2.subs({R:0.08206*L*atm/mol/K,T:273*K,n:1.00*mol,V:24.0*L})
p = 0.9334325*atm
>>> eq2.subs({R:0.08206*L*atm/mol/K,T:273*K,n:1.00*mol,V:24.0*L}).evalf(4)
p = 0.9334*atm

Combining equations (Math with equations: lhs with lhs and rhs with rhs)
>>> q = Eqn(a*c, b/c**2)
>>> q
a*c = b/c**2
>>> t
a = b/c
>>> q+t
a*c + a = b/c + b/c**2
>>> q/t
c = 1/c
>>> t**q
a**(a*c) = (b/c)**(b/c**2)

``solve()``
>>> t=Eqn(a,b/c)
>>> t
a = b/c
>>> solve(t,c)
c = b/a
```

## Backwards compatibility

No backwards compatiblity breaks are necessary. However, it has been suggested that the
`Equality` class might eventually be replaced and then `Eq` and `Equality` could become
synomyms for `Equation`.

## Detailed Description

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
* Functions will apply to both sides. Functions that take more than one parameter will
only accept an equation as one of the parameters. It will be up to the user to make
sure the equation is used appropriately.
```
>>> cos(eq1)
cos(a) = cos(b/c)
```
* Keywords and additional parameters will be passed through to the function.

_Simplification_
* Behavior should mimic the behavior for expressions and default to applying to both sides.

_Differentiation and Integration_
* No special support although they could be done on the separate sides of the equation
  or utilizing the [_Applications of methods_](#Applications-of-methods)

_Solve_
* Solve should accept equations and lists of equations and return equations or lists of
equations as solutions. To avoid breaking past behavior this should only occur when solve
is used on equations or lists of equations.

#### _Applications of methods_
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

## Related Work

Similar functionality is found in [SageMath](https://www.sagemath.org/), [Maple](https://maplesoft.com),
and other symbolic math packages. The SageMath implementation has essentially no limitations
on the lhs and rhs making it possible to set objects that are incompatible equal to each other.
The proposed implementation here currently requires lhs and rhs to be valid sympy expressions.
A python module that implements much of the proposed capabilities is available via pip:
`pip install -U Algebra_with_SymPy`. This module requires SymPy.

## Implementation
1. All the proposed behaviors except for those associated with `solve` have been implemented in
[PR 21333](https://github.com/sympy/sympy/pull/21333). A smaller subset that begins building
`Equation` as a specific case of `relational` has been implemented in [PR 21325](https://github.com/sympy/sympy/pull/21325)
2. The `solve` features will be implemented after step 1 is completed.
3. Acceptance of lists of equations by `.subs()` will be implemented in parallel with or after the
solve features.
4. A later potential extension might be to allow the user to set parameters to control whether human readable,code or
both forms of the expression are produced as output.

## Alternatives

An alternative would be to implement this as part of the work being done on polyadic predicate
[PR 20656](https://github.com/sympy/sympy/pull/20656). This option was discussed in both PR 19749
and PR 20656. The concensus was that until the predicate work reached the stage of being able to
replace the assumptions module it would be better to keep the two pieces separate. At that point
if combination still appeared straightforward the `Equation` class could become a subclass of the general
class of relations that the predicate work is moving towards.

## Discussions

In decending order of volume discussions within the SymPy community have occured on:
* [PR 19749](https://github.com/sympy/sympy/pull/19479)
* [PR 20656](https://github.com/sympy/sympy/pull/20656)
* [SymPy Google Group](https://groups.google.com/g/sympy/c/rSi_I42i35I)
* Non-public discussions by J. Gutow with Physical Science Educators using SymPy in their classes.
These discussions were the original impetus for development of this class to facilitate demonstrating
algebraic manipulations in physical science classes.
* [PR 21325](https://github.com/sympy/sympy/pull/21325)
* [PR 21333](https://github.com/sympy/sympy/pull/21333)
* [Issue 4986](https://github.com/sympy/sympy/issues/4986)

## References

## Copyright

This document has been placed in the public domain.
