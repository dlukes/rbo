Rank-biased overlap
===================

Overview
--------

A small Python module for calculating **rank-biased overlap**, a measure of
similarity between ragged, possibly infinite ranked lists which may or may not
contain the same items (up to the actually evaluated depth or at all). See "A
similarity measure for indefinite rankings" by W. Webber, A. Moffat and J. Zobel
(2011), <http://dx.doi.org/10.1145/1852102.1852106>.

The functions intended for external use are `rbo()` and `rbo_dict()`, plus
possibly `average_overlap()` (for comparison purposes). `rbo()` receives two
sorted lists where each individual item is an immutables or a set of immutables
(to represent overlap):

```python
>>> rbo([{'c', 'a'}, 'b', 'd'], ['a', {'c', 'b'}, 'd'], p=.9)
{'res': 0.47747163566865164, 'min': 0.48919503099801515, 'ext': 0.9666666666666667}
```

By contrast, `rbo_dict` takes a dict mapping the items to sort to scores
according to which they should be sorted:

```python
>>> rbo_dict(dict(a=1, b=2, c=1, d=3), dict(a=1, b=2, c=2, d=3), p=.9)
{'ext': 0.9666666666666667, 'res': 0.47747163566865164, 'min': 0.48919503099801515}
```

Conceptually, the `p` parameter of both functions represents the probability
that a person doing a manual comparison of the ranked lists would stop (i.e.
decide she has seen enough in order to hazard a conclusion) at each transition
to a lower rank. Formally, it's the parameter of the geometric progression which
weights the contribution of overlaps at different depths.

The code is primarily optimized for correctness, not speed. Build your own
faster version and check it for correctness by comparing against this one!

Requirements
------------

Built and tested under Python 3.5.2. No external dependencies.

License
-------

Credits for the concept of the RBO measure are indicated above.

Copyright (this implementation) © 2016 [ÚČNK](http://korpus.cz)/David Lukeš

This implementation is distributed under the
[GNU General Public License v3](http://www.gnu.org/licenses/gpl-3.0.en.html).
