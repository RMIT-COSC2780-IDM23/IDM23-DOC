# Train Network Assignment FAQ

- [Train Network Assignment FAQ](#train-network-assignment-faq)
    - [Can I use built-in predicate `P/n`?](#can-i-use-built-in-predicate-pn)
    - [Is the empty list a path?](#is-the-empty-list-a-path)
    - [For `fastest_route/6`, can we assume there is only 1 fastest route?](#for-fastest_route6-can-we-assume-there-is-only-1-fastest-route)
    - [Can we take that `bounded_route/7` holds if `Duration` is _less-or-equal_ than `Limit`?](#can-we-take-that-bounded_route7-holds-if-duration-is-less-or-equal-than-limit)
    - [How can I compute \& carry "so-far" results?](#how-can-i-compute--carry-so-far-results)

### Can I use built-in predicate `P/n`?

This assignment can be solved neatly using plain basic Prolog built-in predicates. As the Language Restriction section in the specs states you are not to use advanced predicates or libraries. These include all-solution predicates like `findall/3` for example or the constraint libraries, none are needed anyways.

If in doubt always ask in the forum. Some other predicates that you shall not use are:

- `table/1`, `order_by/1`.

Some predicates that you can use as they are still basic are:

- `nth0/3`, `nth1/3`, `list_to_set/2`

### Is the empty list a path?

Since we are interested in paths with an origin and destination, we will assume that a path has at least one city. That is why we had this example for `route/6`:

```prolog
?- route(montreal, montreal, freight, Path, Length, Duration).
Path = [montreal]
Length = 0
Duration = 0
```

So, the empty list is not a path in our setting.

### For `fastest_route/6`, can we assume there is only 1 fastest route?

Yes and no. There could indeed be more than one fastest route because they are equally fast. An implementation that yields all fastest routes is of course better than those that return just one.

You may need a more "general" `bounded_route/7` though, check next question...

### Can we take that `bounded_route/7` holds if `Duration` is _less-or-equal_ than `Limit`?

Not really, the spec is clear in saying it must return paths that are less than the threshold duration, not less-or-equal. :-)

Said so, nothing precludes you to write a more general predicate (that `bounded_route/7` relies on) that does both, as needed: _less_ OR _less-or-equal_. You can configure what you need via a parameter. 

And if you want to go very sophisticated and meta-programming you can use that parameter to pass what kind of operation you should use!. Check this "hint":

![meta-op](imgs/meta-operators.png)

:-)

### How can I compute & carry "so-far" results?

A usual strategy for computing something is to carry intermediate results until the end, and once there, just equate the final result with the result so far.

For example, this is how I can sum a list of numbers:

```prolog
sumar(L, R) :- sumar(L, 0, R).

sumar([], SumSoFar, SumSoFar).
sumar([X|L], SumSoFar, Result) :-
    SumSoFar2 is  SumSoFar + X,
    sumar(L, SumSoFar2, Result).
```

Check a run:

```prolog
?- sumar([1,2,3,4], R).
R = 10.
```

