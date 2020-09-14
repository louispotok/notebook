How does knowledge progress? What is our model for how different thinkers relate to each others work?

<blockquote class="twitter-tweet" data-conversation="none" data-dnt="true"><p lang="en" dir="ltr">In short I guess what i want is for science to be more like the talmud.</p>&mdash; small needs infinite desires (@louispotok) <a href="https://twitter.com/louispotok/status/1178065688503570432?ref_src=twsrc%5Etfw">September 28, 2019</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

## The Paper DAG

Modern science proceeds mostly by papers, using citations for reference. That is, the unit of knowledge is the paper. It is meant to bring new knowledge into the world, in the form of new data, new results, a new synthesis or theory, or even raising questions about the previous literature. 

But the paper stands alone. It can be read and understood by itself. The data structure is a directed acyclic graph with homogeneous nodes and homogeneous edges: a paper can cite any number of previous papers, and all those citations are "the same". Because papers can only refer to earlier papers, and are never edited, cycles are impossible. Citations are usually to the whole paper, but on occasion will refer only to a specific claim.

There is a threshold for what makes a paper, a minimum novelty.

## An alternative

But there is another model: annotation. In this, there is still some notion of a base-level work, but contributors can raise comments or questions directly on sections of the original work, and whole conversations can happen branching off the original annotation. Given an ecosystem of annotators, and appropriate tools, it would require intention and be unusual to read a text without the annotations. You could imagine, too, adding different layers of annotations depending on your goals.

Annotations are a different data structure but also fundamentally a question of user experience, and they require some training in how to use properly.

The most successful annotation ecosystem I'm aware of is in Judaic scholarship. You have a canonical source text, and commentators write their comments on individual lines. These can be clarification of unclear language, or citations to other relevant sections of the text, or expositions of the greater lesson, or really anything that relates to that line from the text. Later commentators can add annotations on the source text or on the previous annotations! The most useful annotation layers became "canonized" on their own, you would really never read the Torah without Rashi's annotation layer. So when you buy a book with the source text, the canonical annotation layers come pre-included for you.

There is a place in the ecosystem, too, for "curators" who pick annotation layers and publish a work with a few selected layers. Similarly, for a specific talk or workshop, the presenter will create a "source sheet" with annotations plucked out from various layers.

The potential applications to digital media are staggering, but no one has made it work yet.
* Google-docs style inline comments are the barest version of this and already incredibly useful for individuals and small groups.
* [Hypothesis](https://web.hypothes.is/) is a system for web annotations, probably the major existing effort.
* Factlink was trying to do something similar, and [JP's website](https://janpaulposma.nl/) has a section on this.
* [Genius](https://genius.com/Genius-about-genius-annotated) has a similar system, for music lyrics only.

## Others

Two other mechanisms for knowledge reference and arrangement are transclusion and bidirectional links, and in fact these are not separate categories so much as conceptual clusters in implmeentation space. Many systems could be considered a blend of all these. Yet another model is reddit-style comment trees, in which a conversation can splinter off from its parents but can only refer to one parent.

