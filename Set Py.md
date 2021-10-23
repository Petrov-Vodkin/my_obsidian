2021-09-27 20:37
[doc en](https://docs.python.org/3/library/stdtypes.html#set)
# Множества Set
Return a new set or frozenset object whose elements are taken from _iterable_. The elements of a set must be [hashable](https://docs.python.org/3/glossary.html#term-hashable). To represent sets of sets, the inner sets must be [`frozenset`](https://docs.python.org/3/library/stdtypes.html#frozenset "frozenset") objects. If _iterable_ is not specified, a new empty set is returned.
Sets can be created by several means:
-   Use a comma-separated list of elements within braces: `{'jack', 'sjoerd'}`
-   Use a set comprehension: `{c for c in 'abracadabra' if c not in 'abc'}`
-   Use the type constructor: `set()`, `set('foobar')`, `set(['a', 'b', 'foo'])`

Instances of [`set`](https://docs.python.org/3/library/stdtypes.html#set "set") and [`frozenset`](https://docs.python.org/3/library/stdtypes.html#frozenset "frozenset") provide the following operations:
![[Pasted image 20210927204302.png]]
`len(s)` Return the number of elements in set _s_ (cardinality of _s_).
`x in s` Test _x_ for membership in _s_.
`x not in s` Test _x_ for non-membership in _s_.
`isdisjoint` (_other_)[](https://docs.python.org/3/library/stdtypes.html#frozenset.isdisjoint "Permalink to this definition")
Return `True` if the set has no elements in common with _other_. Sets are disjoint if and only if their intersection is the empty set.
`issubset` (_other_)[](https://docs.python.org/3/library/stdtypes.html#frozenset.issubset "Permalink to this definition")
`set <= other` Test whether every element in the set is in _other_.
`set < other` Test whether the set is a proper subset of _other_, that is, `set <= other and set != other`.
`issuperset` (_other_)[](https://docs.python.org/3/library/stdtypes.html#frozenset.issuperset "Permalink to this definition")
`set >= other` Test whether every element in _other_ is in the set.
`set > other` Test whether the set is a proper superset of _other_, that is, `set >= other and set != other`.
`union`(_*others_)[](https://docs.python.org/3/library/stdtypes.html#frozenset.union "Permalink to this definition")
`set | other | ...` Return a new set with elements from the set and all others.
`intersection`(_*others_)[](https://docs.python.org/3/library/stdtypes.html#frozenset.intersection "Permalink to this definition")
`set & other & ...` Return a new set with elements common to the set and all others.
`difference`(_*others_)[](https://docs.python.org/3/library/stdtypes.html#frozenset.difference "Permalink to this definition")
`set - other - ...` Return a new set with elements in the set that are not in the others.
`symmetric_difference`(_other_)[](https://docs.python.org/3/library/stdtypes.html#frozenset.symmetric_difference "Permalink to this definition")
`set ^ other` Return a new set with elements in either the set or _other_ but not both.
`copy`()[](https://docs.python.org/3/library/stdtypes.html#frozenset.copy "Permalink to this definition") Return a shallow copy of the set.
Note, the non-operator versions of [`union()`](https://docs.python.org/3/library/stdtypes.html#frozenset.union "frozenset.union"), [`intersection()`](https://docs.python.org/3/library/stdtypes.html#frozenset.intersection "frozenset.intersection"), [`difference()`](https://docs.python.org/3/library/stdtypes.html#frozenset.difference "frozenset.difference"), and [`symmetric_difference()`](https://docs.python.org/3/library/stdtypes.html#frozenset.symmetric_difference "frozenset.symmetric_difference"), [`issubset()`](https://docs.python.org/3/library/stdtypes.html#frozenset.issubset "frozenset.issubset"), and [`issuperset()`](https://docs.python.org/3/library/stdtypes.html#frozenset.issuperset "frozenset.issuperset") methods will accept any iterable as an argument. In contrast, their operator based counterparts require their arguments to be sets. This precludes error-prone constructions like `set('abc') & 'cbs'` in favor of the more readable `set('abc').intersection('cbs')`.

Both [`set`](https://docs.python.org/3/library/stdtypes.html#set "set") and [`frozenset`](https://docs.python.org/3/library/stdtypes.html#frozenset "frozenset") support set to set comparisons. Two sets are equal if and only if every element of each set is contained in the other (each is a subset of the other). A set is less than another set if and only if the first set is a proper subset of the second set (is a subset, but is not equal). A set is greater than another set if and only if the first set is a proper superset of the second set (is a superset, but is not equal).

Instances of [`set`](https://docs.python.org/3/library/stdtypes.html#set "set") are compared to instances of [`frozenset`](https://docs.python.org/3/library/stdtypes.html#frozenset "frozenset") based on their members. For example, `set('abc') == frozenset('abc')` returns `True` and so does `set('abc') in set([frozenset('abc')])`.

The subset and equality comparisons do not generalize to a total ordering function. For example, any two nonempty disjoint sets are not equal and are not subsets of each other, so _all_ of the following return `False`: `a<b`, `a==b`, or `a>b`.

Since sets only define partial ordering (subset relationships), the output of the [`list.sort()`](https://docs.python.org/3/library/stdtypes.html#list.sort "list.sort") method is undefined for lists of sets.

Set elements, like dictionary keys, must be [hashable](https://docs.python.org/3/glossary.html#term-hashable).

Binary operations that mix [`set`](https://docs.python.org/3/library/stdtypes.html#set "set") instances with [`frozenset`](https://docs.python.org/3/library/stdtypes.html#frozenset "frozenset") return the type of the first operand. For example: `frozenset('ab') | set('bc')` returns an instance of [`frozenset`](https://docs.python.org/3/library/stdtypes.html#frozenset "frozenset").

The following table lists operations available for [`set`](https://docs.python.org/3/library/stdtypes.html#set "set") that do not apply to immutable instances of [`frozenset`](https://docs.python.org/3/library/stdtypes.html#frozenset "frozenset"):
`update`(_*others_)[](https://docs.python.org/3/library/stdtypes.html#frozenset.update "Permalink to this definition")
`set |= other | ...` Update the set, adding elements from all others.
`intersection_update`(_*others_)[](https://docs.python.org/3/library/stdtypes.html#frozenset.intersection_update "Permalink to this definition")
`set &= other & ...` Update the set, keeping only elements found in it and all others.
`difference_update`(_*others_)[](https://docs.python.org/3/library/stdtypes.html#frozenset.difference_update "Permalink to this definition")
`set -= other | ...` Update the set, removing elements found in others.
`symmetric_difference_update`(_other_)[](https://docs.python.org/3/library/stdtypes.html#frozenset.symmetric_difference_update "Permalink to this definition")
`set ^= other` Update the set, keeping only elements found in either set, but not in both.
`add`(_elem_)[](https://docs.python.org/3/library/stdtypes.html#frozenset.add "Permalink to this definition") Add element _elem_ to the set.
`remove`(_elem_)[](https://docs.python.org/3/library/stdtypes.html#frozenset.remove "Permalink to this definition") Remove element _elem_ from the set. Raises [`KeyError`](https://docs.python.org/3/library/exceptions.html#KeyError "KeyError") if _elem_ is not contained in the set.
`discard`(_elem_)[](https://docs.python.org/3/library/stdtypes.html#frozenset.discard "Permalink to this definition") Remove element _elem_ from the set if it is present.
`pop`()[](https://docs.python.org/3/library/stdtypes.html#frozenset.pop "Permalink to this definition") Remove and return an arbitrary element from the set. Raises [`KeyError`](https://docs.python.org/3/library/exceptions.html#KeyError "KeyError") if the set is empty.
`clear`()[](https://docs.python.org/3/library/stdtypes.html#frozenset.clear "Permalink to this definition") Remove all elements from the set.
Note, the non-operator versions of the [`update()`](https://docs.python.org/3/library/stdtypes.html#frozenset.update "frozenset.update"), [`intersection_update()`](https://docs.python.org/3/library/stdtypes.html#frozenset.intersection_update "frozenset.intersection_update"), [`difference_update()`](https://docs.python.org/3/library/stdtypes.html#frozenset.difference_update "frozenset.difference_update"), and [`symmetric_difference_update()`](https://docs.python.org/3/library/stdtypes.html#frozenset.symmetric_difference_update "frozenset.symmetric_difference_update") methods will accept any iterable as an argument.

Note, the _elem_ argument to the [`__contains__()`](https://docs.python.org/3/reference/datamodel.html#object.__contains__ "object.__contains__"), [`remove()`](https://docs.python.org/3/library/stdtypes.html#frozenset.remove "frozenset.remove"), and [`discard()`](https://docs.python.org/3/library/stdtypes.html#frozenset.discard "frozenset.discard") methods may be a set. To support searching for an equivalent frozenset, a temporary one is created from _elem_.

_____________
#### Links
