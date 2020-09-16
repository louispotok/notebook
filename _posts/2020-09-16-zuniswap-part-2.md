---
mathjax: true
---

See also: [Part 1]({{site.baseurl}}{% link _posts/2020-08-31-generate-predictions.md %}).

We left off explaining Uniswap: there is a `buyer` and a `seller`, the `buyer` wants to make an `A<>B trade` where `A` and `B` are both ERC-20 tokens. The seller is providing `A<>B liquidity` by creating an `A<>B pool`, which removes the need to find a seller for each transaction.

Since the "sellers" are not operating per-transaction, that's actually not a great name. The Uniswap docs call them `LPs`, for "liquidity provider" -- let's go with that.

`LPs` deposit some amount of `A` and `B` tokens into a "pool", and receive "pool tokens" in return. The pool tokens can later be exchanged for the underlying assets (`A` and `B`).

Each pool has a simple rule: the "constant product invariant". Suppose that the LP deposited `x` amount of `A` and `y` amount of `B`. `x*y = k`, some number that is different for each pool but has to be the same over the pool's lifetime. This invariant is what sets the price of the exchange!

Let's look at an example. If you deposit 5 each of `A` and `B`, then `x=y=5` and `k=25`. If someone wants to buy 1 of `A`, then there will be 4 of `A` remaining in the pool, so the price `p` in units of `B` is set by:

$$
\displaylines{
\text{Original constant product:} \\
k = A \times B = 5 \times 5 = 25 \\
\text{Similarly for $k'$:} \\
k' = A' \times B' = 4 \times (5+p) \\
\text{By the invariant rule:}  \\
k = k' \\ 
\text{therefore:} \\
p = (25/4) - 5 = 1.25
}
$$

If you wanted to buy 2 of `A`, then your price would be instead $$(25/2) - 5 = 7.5$$, for a per-A price of 3.75. So we see that the price goes up with the transaction size (relative to the amount in the pool, or the "reserves"). This is considered a desirable property, presumably because you want the pools to stick around which leaves liquidity in the market.

LPs are incentivized to set `x` and `y` according to the current market price, because otherwise they can get arbitraged. There is an "oracle" that LPs can use to know the current market price, if they wish.

There is also a .3% fee on all transactions -- currently this goes to the LP, but eventually one-sixth of that (.05% of the transaction) will go to Uniswap itself, and the remaining part to the LP. 

Remaining things I don't understand:
* Does the pool stick around after a purchase, or does it dissipate?
* Do the seller's pool tokens get exchanged for what remains in their pool or some other way? Seems it must be the former for incentivization to work properly.
* How are pools discovered?

White paper: https://uniswap.org/whitepaper.pdf
