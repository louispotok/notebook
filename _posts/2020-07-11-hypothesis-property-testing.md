---
title: Property Testing With Hypothesis
---

One of my favorite tools for python development is the [Hypothesis](https://hypothesis.works/) package/testing framework. It brings "property testing" to python.

## Intro to software testing
It is hard to write good software. We often have a specific intent, and fail to implement it correctly. Or our intent is inadequate or wrong in some way. Or we get lazy, etc. We also want to make sure that when we modify software we don't break previous functionality.

The standard approach to this is "unit testing", which works as follows:

1. Run some part of your program (often a specific function/subroutine/method).
1. Compare the output to the expected output.

Another way to say this is that you feed the program an input/output pair, and confirm that, when fed the input, the program outputs the expected output. 

Example: suppose we have a function `multiply_complex` that multiplies two complex numbers. In python, we might test it like this:

```python
def test_multiply_complex():
    x = Complex(3,4)
    y = Complex(2,8)
    actual = multiply_complex(x, y)
    expected = Complex(-26, 32)
    assert actual == expected
```

Usually, for a given piece of code, you'd provide a few input/output pairs. Maybe one that is "typical" and few that are "interesting" or known edge cases. This generally works pretty well, and is usually considered to be better than no testing. However, it has a few main weaknesses:
* You have to know the expected result. If the code is doing something complicated, you may not have another way to calculate it -- often the code you write is your best attempt at how to calculate something! Even if you can calculate it another way, if the two methods yield different results, you may not know which is correct.
* It can be time consuming to think of the right examples, and to manually change them every time you find a bug.
* You are responsible for supplying "interesting" examples. Over time you get better at it, as in [this joke](https://twitter.com/brenankeller/status/1068615953989087232?lang=en). But overall, if your examples don't adequately cover the input space, future development could overfit to those examples, but be wrong elsewhere.

## Enter property testing

Property testing takes a different approach. Instead of providing input/output pairs, you provide:

1. *strategies* for generating inputs.
1. *properties* that hold for all input/output pairs.

Then your testing framework generates inputs using your strategies and checks whether the properties hold. If it finds any inputs where the properties don't hold, the test fails.

This involves a bit more thinking up front. What kinds of inputs do we need, and properties do we expect? You can get sophisticated, but even fairly naive properties will often turn up errors.

How would we use property-based testing to test our `multiply_complex` function? 

First, what do inputs look like? Any list of four numbers is a valid input.

Then, what properties would we expect? Here are a few:
1. The result is a complex number.
1. Multiplication should be associative commutative, and respect identity.
1. If we have a `complex_divide` function, then dividing the result by one of the factors should give back the other one.

Here's what that might look like in Hypothesis:

```python
# The `given` decorator is where we describe how to generate our inputs.
# Here we're generating a list of 6 floating point numbers, which will allow us to construct 3 complex numbers.
# We're making 3 so that we can use the associative property.
@given(st.lists(st.floats(allow_nan=False, allow_infinity=False), min_length=6, max_length=6))
def test_property_multiply_complex(inputs):

    # unpack the inputs
    x1, x2, x3, x4, x5, x6 = inputs
    c1 = Complex(x1, x2)
    c2 = Complex(x3, x4)
    c3 = Complex(x5, x6)
    
    # test that result is complex
    assert is_complex(complex_multiply(c1, c2))
    
    # test associative
    assert complex_multiply( complex_multiply(c1,c2), c3) == complex_multiply(c1, complex_multiply(c2, c3) )
    
    # test commutative
    assert complex_multiply(c1, c2) == complex_multiply(c2, c1)
    
    # test identity
    identity = Complex(1, 0)
    assert complex_multiply(identity, c1) == c1
    
    # test roundtrip with division
    assert complex_divide(complex_multiply(c1, c2), c2) == c1
```

Now, it's the program's responsibility to generate examples where any of the assertions are false. (If this was a real codebase, I'd probably break each assertion into a separate function.) If it isn't able to, then the test passes! Since it can't generate every possible input, in practice what you do is set a maximum number of inputs to try, or a maximum time to try.

This has lots of benefits. It means that on every test run you're trying many more input/output pairs than you could when you're manually writing every test. And by thinking about properties, you're actually encoding deeper truths about the objectives of the code, rather than just a few specific examples you thought of.

A few costs:
1. Property-based testing is usually not deterministic, so this can introduce flakiness to your test suite.
1. Time: because you're generating and testing hundreds of examples per test function, and then shrinking every failing example, property testing is definitely slower than unit testing. 

While this is all that a property testing framework *has* to do, a massive increase in usefulness comes from "test-case reduction", or "shrinking". In other words, it doesn't output the first failing example it finds. Instead it tries to simplify that example as much as possible and still have it fail, so that it's simpler for you to understand why it's failing. For example, when generating integers, smaller integers are "simpler" than bigger ones; when generating strings or lists, shorter ones are simpler.

What are the criteria by which to evaluate a property testing framework?
* Expressiveness of the generating strategies: in real codebases, you often have fairly complex inputs, and Hypothesis provides lots of flexibility to construct extremely specific input strategies.
* Quality of "shrinking": For more complex inputs, it's not always obvious how to shrink a failing example. Simpler results make it easier to figure out why your test is failing. Hypothesis has a cool approach to this, which I'll describe below.
* Speed: As above, this can be slow. A good framework should minimize that extra time.
* There are lots of additional nice-to-haves that can be added into this workflow. One important thing Hypothesis does is maintain a local database of previous test runs. So if a test fails, and then you try to fix the code, Hypothesis will deterministically re-run the failed example.

One common pattern in property testing is to exploit related functionality. In our example, we used the `complex_divide` function; these two functions are now tied together in our test suite. Another example of this pattern is the [encode/decode invariant](https://hypothesis.works/articles/encode-decode-invariant/). 

## Implementation

Hypothesis has a neat approach to test case generation and reduction. 
I haven't dug into the internals too much, but as best I understand the core idea is that we don't want to write a separate generator and shrinker for every type of data. Instead, the approach is:

1. Develop a shared canonical representation for test cases, across all types. This representation must have an ordering, so we can say that one test case is simpler than another if its canonical representation is smaller.
1. Write a generator for that representation.
1. Write a shrinker for that representation, using the ordering mentioned in (1).
1. For each atomic data type you want to support, write a translation from that canonical representation to the data type.

## "Nontechnical"

There's a lot more to say here, but one of the reasons I like Hypothesis so much is the extensive and well-written documentation. It's clearly all the reflection of a single mind, though there are several main developers. Overall the package provides an approachable but powerful interface to deep ideas, and leads to better software.

If you are interested in talking more about Hypothesis, or want some help getting started, I'd love to chat.

You may also be interested in [metamorphic testing](https://www.hillelwayne.com/post/metamorphic-testing/).
