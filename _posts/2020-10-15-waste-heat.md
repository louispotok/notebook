This is a follow up to [Propane vs Electric Heaters]({{site.baseurl}}{% link _posts/2020-10-14-heaters.md}).

I spent more time looking into anthropogenic waste heat and its relationship to climate change the other day. I figured some of it out, but am still a bit confused on the fundamentals and could only find some older sources.

Disclaimer: I think that "waste heat vs CO2" was an argument that climate deniers used to make. That's obviously not my situation.

# Comparing waste heat to GHGs

## Waste heat
As I understand it, waste heat is measured in W/m2 (= J/sm2): the energy flow per time per area. This makes sense to me, a net outflow of heat.

The [Wikipedia article on waste heat](https://en.wikipedia.org/wiki/Waste_heat#Environmental_impact) says: "Global forcing from waste heat was 0.028 W/m2 in 2005."

There's significant local variation but I'll ignore that for simplicity.

## GHGs

GHG emission are usually measured in GWP or CO2-equivalent tons. For simplicity I'll ignore non-CO2 GHGs like methane and f-gases -- I'd be curious how much this changes the story.

How do we convert this to compare with waste heat? Section 2.2 in [Zhang 2015](https://agupubs.onlinelibrary.wiley.com/doi/full/10.1002/2015GL063514) explains how to convert CO2 concentration to W/m2: "The radiative forcing from CO2 in the atmosphere increases approximately with the logarithm of atmospheric CO2 content, with an increase of about 3.7 W m−2 for a doubling of atmospheric CO2". 

Radiative forcing, then, is also measured in W/m2 -- it's the flow of heat that *would have* escaped the atmosphere if not for GHGs. So radiative forcing from atmospheric CO2 is `5.34 * ln(CO2)`, where `5.34 = 3.7/ln(2)`.

If we plug a pre-industrial baseline of 280ppm, and a current level of 415ppm, this leads me to find a change in radiative forcing of 2.1 W/m2. 

## Comparison:

0.028 / 2.1 = .013, about 1.3%. And indeed, the Wikipedia article says:
> "globally [waste heat] accounted for only 1% of the energy flux created by anthropogenic greenhouse gases". 

Okay, this seems to check out.

# Current consensus:

I went digging to understand that 1% claim in the Wikipedia article. It looks like it's citing [Flanner 2009](http://clasp-research.engin.umich.edu/faculty/flanner/content/ppr/Flannr09.pdf), which in turn cites [Crutzen 2004](https://www.researchgate.net/publication/44158048_New_Directions_The_growing_urban_heat_and_pollution_island_effect_-_Impact_on_chemistry_and_climate), which in turn looks like its citing publications from 1998 and 2001. 

[Jin et al 2019](https://www.nature.com/articles/s41597-019-0143-1) claims that global average AHF flux was 0.05 W/m2 in 1970 and 0.13 W/m2 in 2015.

So if we take this updated number 0.13, and compare to our 2.1 above, it looks like waste heat is now about 6% of radiative forcing.

# Effects over time

There's something about the comparison that I still don't quite understand intuitively. When you emit 1 ton of CO2, you are changing its atmospheric concentration and changing the rate at which the earth can radiate heat permanently (or at least for a very long time). When you generate 1 joule of heat, you are adding heat once. In other words, this is like "narrowing the bathtub drain" vs "adding 1 cup of water to the tub". To draw a direct comparison, don't we need to understand the time horizon and make some assumptions about future emissions, both heat and CO2? Am I missing that in the Zhang paper?
