---
mathjax: true
---

I really like puzzles. One great source of puzzles is interview questions for knowledge work. The first kind I was exposed to was consulting-style Fermi questions: how many M&Ms fit on a jumbo jet? But when I got deeper into math and computer science, I discovered that software engineering interview questions were also enjoyable.

There are some that rely on knowledge I don't have: I have no formal training in CS, and there are concepts (especially "closer to the metal") that elude me. But when I hang out with engineers I like talking through their interview problems.

I heard a good one recently from [JP](http://www.janpaulposma.nl/), I'll share the problem and how it went.

## The problem

> Write a function that generates a minesweeper board.

This is normally given as a programming challenge but we just talked through it.

## The solution path

First, clarify the inputs and outputs.
* The inputs are $$(m,n,x)$$, where $$m$$ and $$n$$ represent the board dimensions, and $$x$$ is the number of mines in the board (and $$ x <=  m \times n $$).
* The output is an $$ m \times n $$ array where each value in the array is either:
	* $$ -1 $$, indicating the location of a mine, or
	* An integer $$0<=i<=8$$ indicating a non-mine, where the value is how many mines it borders.

Okay. Now we know what is needed, we can split up the solution into two sequential steps:
1. Determine the mine locations
1. Fill in the non-mine values

### Mine locations

For now we don't need to worry about the dimensionality of the board, we can just think about picking $$x$$ positions randomly, without replacement, out of $$ N = m \times n $$ options. Easy!

JP stopped me: "how would you pick without replacement?" Python has a [built-in library function](https://docs.python.org/3/library/random.html#random.sample) for this, but he wanted me to implement it, assuming I only have access to a function like [`randint`](https://docs.python.org/3/library/random.html#random.randint), which selects one number randomly from a range.

#### Naive approach

Here I pulled out a really useful trick, that everyone who solves these kinds of problems should know: **give a really dumb answer first**. Actually this is a good strategy when solving these problems for real. Sometimes the dumb approach is good enough, and anyway it lets you modularize the solution and move on, then revisit when there's enough time.

"Okay, I'm going to give a really naive approach first. You just keep calling `randint(0, N)`, and throw away any values you've seen before."

Great, that solves the problem, but it's pretty bad for $$x$$ close to $$N$$. We didn't try to quantify how bad, but it's bad.

Can we do better?

#### Second attempt: offsets
Well, once we pick the first value $$v_1$$, there are only $$ N - 1$$ options to choose from. So we should be able to call `randint(0, N-1)` the second time, giving us a candidate value $$v_2c$$. If $$v_2c<v_1$$, set $$v_2 = v_2c$$; otherwise, $$v_2 = v_2c + 1$$.

Essentially, we just shift pieces of our $$(0, N-i)$$ range around to cover the actual candidate values (the ones we haven't selected yet).

This solves the problem, but I think it's quadratic time with respect to $$x$$. Imagine $$x = N-1$$. Then to pick $$v_x$$ you need to consider all the $$x-1$$ values you've already chosen.

Can we go faster? JP told me we can actually get *linear* time on this. Hmm....

#### Third attempt: shuffle

I couldn't think of it at first. I got a hint: think of every square as a candidate, and then how would you pick $$x$$ squares at random?

That helped! Okay, what we're going to do is shuffle the integers $$0...N$$ and then take the first $$x$$! Elegance itself.

How do you shuffle those, assuming you don't have a library function? 

My answer: assign them each a random value, and then sort the list. This assumes you have access to a `random(0,1) -> float` function, or you can hack with `randint(0, L)`, or `randint(0, X) / randint(0, Y)` for suitably large `L, X, Y`.

However, sorting a list is $$O(n\log{}n)$$ time, and we want to get $$O(n)$$. How do you get to linear?

I didn't get the answer. It turns out there's an algorithm, the [Fisher-Yates shuffle](https://en.wikipedia.org/wiki/Fisher%E2%80%93Yates_shuffle), for shuffling a list in linear time. This was more "did you know it or not". I didn't know it, oh well.

But anyway, now that we have our mine locations, we can move on.

### Assigning non-mine values
This was pretty simple: 
1. Build your $$m \times n$$ array (eg as a list of lists) with mines in the appropriate location.
1. For each value:
	* if it's a non-mine, continue.
	* If it's a mine, consider all the bordering squares. For each one that isn't a mine, increment it by 1.


For a bonus speedup, you can write an alternate to step 2 where you skip the mines and act only on the non-mines, and then pick that path if more than half of the squares are mines.

I thought there was going to be the naive solution, and there would be a better way to do it, but it turns out this was right. Apparently this section is more about coding chops: there are a lot of ways to implement this, some much more elegant and [DRY](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself) than others. For example, do you write 8 conditionals? Or a nested for-loop over the two dimensions? There's also some trickiness around mines that are on the border, you have to not get `IndexErrors`. But since we were just talking through it, we didn't go deeper here.


## Testing
Finally, JP asked: how would you test this function?

Well, because there's randomness, you can't do a straightforward unit test where you pass in a known input and check if the output is what you expected. And you wouldn't want to pass in a random input and then check if the output is correct, since you'd end up just re-implementing the same logic -- and then you wouldn't know which was wrong, the test logic or the logic under test!

What you can do instead is freeze the randomness somehow, either by setting the seed outside the test, or dependency injection, or passing a random number generator in the pure-functional style, depending on the language.

You could also do something like [property-based testing]({{site.baseurl}}{% post_url 2020-07-11-hypothesis-property-testing %}). Generate a random board and test properties:
* Output is an $$m \times n$$ array
* Output has $$x$$ mines (values of `-1`).
* All non-mine squares are an integer between 0 and 8.
* I thought for a minute you could do some kind of checksum on the non-mine values, but that wouldn't work: the sum is going to be different depending on whether mines border each other.

Now, final bonus question from JP: How would you check that it's uniform?

Again, I thought my answer was pretty dumb, but he didn't have a better one: run it a bunch of times and look at the resulting distribution. I threw some fancy stats in there but simple comparisons or use of standard deviations would probably be fine. This would make your test suite take much longer, so you could run this offline and not make it part of the test suite. For double bonus points, I mentioned that in pytest (and likely other test frameworks) you can mark tests as long-running, so you can run the "full" test suite only occasionally and not during a development loop.

## Overall

I liked this puzzle. It involved a little bit of algorithms and big-O knowledge, but you could make a lot of progress without that. And there were a lot of opportunities for experienced programmers without deep CS background to demonstrate their value.

It also reminded me that there's an art to answering these kinds of questions: thinking out loud, questioning assumptions, giving good-enough answers to move on, building rapport with your interviewer, etc.
