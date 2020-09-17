One of the superpowers that you can develop, if you work with data in your life, is to get comfortable with simulation. It's a major member of my toolbox and I encourage everyone to give it a try. 

The basic situation:
1. There is a real "data-generating" process in the world.
1. You are designing a "system" that interacts with that data in some way, either just by analyzing it or by making decisions that then participate in the data-generating process.
1. You want to understand how that system will behave.

Now simulation isn't the only way to do this: you can often use an "analytical" approach, which basically means using formulas. But this can require deeper expertise, and often in practice there isn't a clean formula for what you want to do, or you have some domain complications that doesn't let you use an off-the-shelf approach.

So instead, you write code that mimics the world's data-generating process, usually a few different ways or with different parameter values. Then you hook your system up to that code and see what happens.

This has a few good side effects:
1. It forces you to think deeply about your beliefs: what does the world's data-generating process actually look like? How does your system's output affect that?
1. You can solve smaller versions of the problem first, and then add in complexity to your simulation progressively.
1. Whatever code you write or visualizations you create to understand the simulation results can be used with little-to-no modification once you launch "for real".
1. If you keep your simulation code around, you can re-run it as an "integration test" before major changes.
1. If your real-world results are different from the simulation, maybe you didn't understand the world as well as you thought -- and you can refine your simulation.

## Example 1: Reinforcement Learning

I was doing some work for a portfolio company at the Data Guild. Their product was a reinforcement learning system, but it didn't have any notion of time -- it just made decisions at a fairly arbitrary cadence. We wanted to add a feature that would allow it to choose explicitly *when* it would take actions, and we wanted to reuse existing system machinery as much as possible.

The main question we wanted to answer was "compared to just using overall population-level tendencies, how long would it take for our system to start customizing its timing effectively to particular counterparties?" This was an important question for us, but it didn't fall directly out of the literature on our techniques. And there were some nuances around how timing affected our outcome metrics that we needed to account for as well.

[Chris](https://www.cpdiehl.org/) suggested that we run some simulations before committing to a design, so we did! This was the first time I had used simulation like this and it worked great: we found some conceptual issues with the design, tightened our estimates for some hyperparameter settings, and were able to deliver some business estimates of the lift the feature would provide. We also built some really cool visualizations.

## Example 2: Causal Analysis 

When I was running the data science team at Agathos, we used a take-home case study to evaluate late-stage potential hires. It was a simplification of a real problem we had solved, and we structured the assignment like this:
1. We send the candidate a written description of the problem.
1. They reply with initial thoughts, including a plan for how they'd solve it and the data they would need to use.
1. We review their work, and then have a phone call to discuss their ideas.
1. We send over a dataset, and they submit a notebook with their code and results, including written discussion.

Candidates could be rejected after steps 2, 3 or 4.

Now, the problem had a somewhat tricky causal structure. We were asking about the effect of `X` on `Y`, but the way the problem was set up, `Y` also had a causal effect on `X`, so you had to be careful.[^success-rate]

So candidates had to design analytical strategies to get around this causal issue. Some of these were pretty complex, and it wasn't always easy for me to think through whether it would work or not. So I started simulating! 

I wrote a notebook that would generate fake data under a variety of "true effects", and then for each candidate I would implement their analytical strategy in code (about 5-10 minutes of work) and run it on this fake data. This would show me whether their approach successfully avoided the causal trap and isolated the effect we cared about.

Whether their solution actually worked wasn't the key selection criteria, but I could use it to build some intuition that would help me give good hints to the candidates about potential issues during the phone call.

[^success-rate]: About 75% of candidates didn't see this and just ran a regression of `X` on `Y`, or similar. This didn't take any specific statistical knowledge to see, just a little bit of critical thinking. Incidentally, I've posed this question to some smart friends with no "data science" background and most of them get this issue pretty quickly.

## Negative Example

Or, "I wish that I knew what I know now, when I was younger" ([song link](https://www.youtube.com/watch?v=LhjHBV20ZV4)).

Early in my career, I was designing the first "production" version of TrueAccord's outreach engine. The system would have repeated interactions with our customers, and it would have to decide when to initiate outreach and what strategies to use from our "library". At its core, this looked like a bandit problem, but there were a bunch of complications around domain-specific rules and heuristics, technical limitations, as well as the fact that new strategies were constantly becoming available. We ended up having to make a bunch of informed guesses, and tweaking when we could actually look at the data, but I often wish that I had just run some simulations to better understand the dynamics.

## Related Ideas

[Dan](https://www.danielgreene.net/) brought my attention to [this paper](https://scholar.harvard.edu/todd_rogers/publications/political-extremism-supported-illusion-understanding) a few years ago. His summary:
> when you ask people to explain how a given policy proposal would have its effects (the causal steps from that policy to some outcomes), people become less sure of their understanding of the policy, and this in turn reduces how strongly they support or oppose the policy. They become more moderate when they realize their lack of understanding.
> Notably, this effect does not happen when you ask people to explain why they support the policy - only when they have to explain how the policy would have its effects.

In other words, even qualitatively there is something special about being forced to specify the mechanism. This is what happens when you write code simulating the part of the world you care about!

Also related is Bret Victor's idea of [model-driven debate](http://worrydream.com/ClimateChange/#media-debate).

The heaviest dose of statistics-as-simulation I've seen is from Andrew Gelman, through his [excellent blog](https://statmodeling.stat.columbia.edu/) and books. He treats simulation as a major part of any statistical workflow. He's not alone in this, the Bayesian community leans fairly heavily on simulation, but Gelman is where I was first and most deeply exposed to it.
