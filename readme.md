A catalogue of polynomial approximations
========================================

These text files report the polynomial approximations on the Pareto
front of a few computational efficiency metrics and of accuracy.  The
current version only considers single and double float polynomials,
with degree at most 16, for a few transcendentals: sin, cos, atan,
exp, log, log1px, and lg1px.

The efficiency metrics are: the polynomial's degree, the number of
non-zero multipliers, the number of non-{-1, 0, 1} multipliers, the
number of non-{-2, -1, 0, 1, 2} multipliers, and whether the constant
offset is equal to 0, 1, 2 or other (3).

The structure of the metrics for multipliers reflect that multiplying
by zero (i.e. doing nothing) is at least as fast as multiplying by
one, which is itself at least as fast as multiplying by two.  However,
multiplication by three or other integers doesn't seem exploitable: FP
multipliers on current x86 are nearly as fast as adders.

Accuracy is measured with the maximal absolute error over the
considered input range; a rounded base-2 logarithm (`lb_error`) is also
reported, to distribute accuracy in discrete buckets.

The `single/` directory reports values for single float approximations,
and `double/` for double float approximations.

For each directory and each function (e.g. exp), there are two files:

* `exp-degree-lb_error-non_zero-non_one-non_two-constant-error` sorts
  and reports the Pareto front by degree, then log-error, then number
  of non-zero multipliers, non-{-1, 0, 1}, non-{-2, -1, 0, 1, 2},
  constant offset and finally maximal error.  This file is likely most
  useful when looking for the most accurate approximation in a given
  computational budget.

* `exp-lb_error-degree-error-non_zero-non_one-non_two-constant` sorts
  by log-error, degree, maximal error, and reports the characteristics
  of the multipliers and of the constant offset.  This file is better
  suited to determining the most efficient approximation given an
  accuracy goal.

All the files have the same structure: first, a one-line summary of
its contents -- in particular the approximated function, the range
considered in the optimisation, and the maximal degree --, a header,
and, finally, the approximations, one approximation per (very long)
line.  Each approximation line first includes the performance and
accuracy metrics, then a pipe (|), the coefficients in float format
(in order, for `x**0`, `x**1`, `x**2`, etc.), a pipe, the same in
rational form, a pipe, and an unique identifier.

The unique identifier is made of the approximated function's name,
concatenated with the MD5 hash of the float coefficients, in a
contiguous vector, in X86 representation (sign-magnitude, little
endian).  The identifier is useful to refer to approximations, and to
make sure that your language implementation is parsing floats
correctly.

In theory, if an approximation isn't listed in these files, it's not
more interesting that at least one of the approximation that is in the
files.  An approximation is uninteresting if it has really bad
accuracy (more error than an approximation with a degree lower by
three), or is less accurate than an approximation with coefficients
that are at least as efficient to compute (this is conservatively
approximated with the number of 0, 0-or-1 and 0-or-1-or-2 multipliers,
and whether the constant offset is 0, 1, 2, or other).

Future versions will likely split the directories to report
approximations that optimise relative or absolute error, and to
include rational polynomial approximations as well.

More information is up at
http://pvk.ca/Blog/2012/10/07/the-eight-useful-polynomial-approximations-of-sinf-3/
