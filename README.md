Rank-biased overlap
===================

Overview
--------

A small Python module for calculating **rank-biased overlap**, a measure of
similarity between ragged, possibly infinite ranked lists which may or may not
contain the same items (up to the actually evaluated depth or at all). See "A
similarity measure for indefinite rankings" by W. Webber, A. Moffat and J. Zobel
(2011), <http://dx.doi.org/10.1145/1852102.1852106>.

The definition of overlap has been modified to account for ties. Without this,
results for lists with tied items were being inflated. The modification itself
is not mentioned in the paper but seems to be reasonable, see function
`overlap()`. Places in the code which diverge from the spec in the paper
because of this are highlighted with comments (search for "NOTE").

The functions intended for external use are `rbo()` and `rbo_dict()`, plus
possibly `average_overlap()` (for comparison purposes). `rbo()` receives two
sorted lists where each individual item is a hashable object or a set of
hashable objects (to represent ties):

```python
>>> rbo([{"a", "c"}, "b", "d"], ["a", {"b", "c"}, "d"], p=.9)
RBO(min=0.48919503099801515, res=0.47747163566865164, ext=0.9666666666666667)
```

The function returns a `namedtuple` with three fields whose values correspond
to three RBO estimates (all defined in the paper):

- `min` is a lower-bound estimate
- `res` is the corresponding *residual*; `min` + `res` constitutes an upper
  bound estimate
- `ext` is an *ext*rapolated point estimate

By contrast, `rbo_dict` takes a dict mapping the items to sort to the scores
according to which they should be sorted:

```python
>>> rbo_dict(dict(a=1, b=2, c=1, d=3), dict(a=1, b=2, c=2, d=3), p=.9, sort_ascending=True)
RBO(min=0.48919503099801515, res=0.47747163566865164, ext=0.9666666666666667)
```

Scores are typically the higher the better, so the sort is descending by
default. You can specify `sort_ascending=True` to override this if you have
some rank-like score (i.e. the lower the better).

Conceptually, the `p` parameter of both functions represents the probability
that a person doing a manual comparison of the ranked lists would stop (i.e.
decide she has seen enough in order to hazard a conclusion) at each transition
to a lower rank. In other words, it "models the user's *persistence*" (Webber
et. al, p. 17). Formally, it's the parameter of the geometric progression which
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
