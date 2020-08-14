
Newton made time absolute, and for centuries afterwards clocks chimed through Europe with precise regularity, one second[^one-second] after another, as the clockmakers labored to make time more and more precise, as the historians and geologists and physicists worked to send the evenly spaced timegrid backwards to the dawn of the universe.

[^one-second]: 9,192,631,770 periods of the radiation corresponding to the transition between the two hyperfine levels of the ground state of the caesium-133 atom, if you must know.

> Absolute, true and mathematical time, of itself, and from its own nature flows equably without regard to anything external...

Geologic and even historic time are basically [incomprehensible](https://louispotok.com/dont-hold-your-breath/) to humans, as evidenced by the [eternal popularity](https://www.reddit.com/r/todayilearned/search?q=moon+cleopatra&restrict_sr=on&sort=relevance&t=all) on Reddit of facts along the following lines:
* Cleopatra was born closer in time to the moon landing than to the construction of the Great Pyramid
* Oxford University is older than the Aztec Empire
* T-rex lived closer to today than it did to the Stegosaurus

One small hook I use to grapple with this is to compare the *duration* of events to *how long ago they were*. I discovered this in my own life first. Four years after graduating, I realized that I had been out of college for longer than I had been in college. This then became a kind of natural lens with which to look at past jobs, relationships, and other such periods. I'm at almost exactly this point with respect to my first tech job: I was there for 3 years and 3 months, and left exactly that long ago. 

Turning towards history, I usually find that I've underestimated historical time durations. To get some examples, I pulled Wikipedia's [list of empires](https://en.wikipedia.org/wiki/List_of_empires) into a csv using [this tool](https://wikitable2csv.ggor.de/). I then cleaned it up a bit and did a bit of manipulation in a jupyter notebook. You can download the [cleaned csv]({{site.baseurl}}{% link assets/notebooks/empire-moments/empires.csv %}) and the [notebook]({{site.baseurl}}{% link assets/notebooks/empire-moments/empire-moments.ipynb %}) or view the full notebook html [here]({{site.baseurl}}{% link assets/notebooks/empire-moments/empire-moments.html %}).

Here are empires with an "inflection point" within 50 years of today. A negative number means the inflection point has passed, and a positive number means its upcoming.

{% include empires/output-table.html %}

The one that really jumps out to me is the Khmer Empire, maybe because of my recent connection to Cambodia. It lasted 629 years! That means if it were ending today, it would have started 100 years before Columbus sailed to America.

Or take the Belgian Empire. Lasted 61 years and about to hit the time-inflection point. It fits comfortably within my dad's lifetime.

As I [tweeted](https://twitter.com/louispotok/status/1294296551662055430), I would like to find the right terminology for these kinds of moments -- I find them personally meaningful and a small useful tool for understanding time.
