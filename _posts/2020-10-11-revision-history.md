New feature on the blog: every post shows its revision history.

{% include image.html url="img/revision-history.png" alt="Example of revision history" %}

The code is taken from [this post](https://ryanjduffy.github.io/blog/2016/01/08/including-git-history-in-a-jekyll-post.html) by Ryan Duffy -- thanks Ryan!

Currently the implementation is that the browser calls the Github API at page load time, but I don't love this solution. I want to avoid JS where possible, since this is supposed to be a static site! And on some level it irks me to call this API when the information should be available during site build.

This is part of an ongoing concern. I've been thinking for a while about how to show revision history for online text. Eventually I would like to move towards the vision I laid out in this tweet, but this is a good start:
<blockquote class="twitter-tweet">
<p lang="en" dir="ltr">`git blame` should be the default view for every piece of online text you read -- for every line, show when it was last modified, by whom, and provide an unobtrusive link to the diff that was introduced. are there any websites that take this seriously?</p>&mdash; small needs infinite desires (@louispotok) 
<a href="https://twitter.com/louispotok/status/1285594751694036993?ref_src=twsrc%5Etfw">July 21, 2020</a>
</blockquote> 
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script> 

Or imagine shading/color coding on the parts of the text that have been edited most often -- this would probably be more useful in a programming context. In general writing wants to seem timeless, as though that text has always been there and will always remain, and I'm interested in exploring textual media that play with time and impermanence. I want an archaeology of text. Wikipedia has some powerful tools for this, but I find them quite clunky to use.
