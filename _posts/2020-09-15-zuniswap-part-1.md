Notes as I attempt to understand [Zuniswap](https://zuni.fi/). It is/will be a "defi" (decentralized finance) protocol built on Ethereum, forked from UniswapV2. I am completely new to this world and am trying to understand what it does.

## In their own words
The team has posted 3 essays on Medium so far. They also have a discord, a telegram group, and a twitter account. Their stated goals at the moment are about building a community, understanding its composition and goals, and then designing a protocol around that information.

They seem to be reacting to problems with past protocols, mainly "exit scams" which seem to involve people creating protocols, making a quick buck, and then dumping losses onto the community that joined their project.

### Medium Posts

#### Synthetic Minnowing

[Synthetic Minnowing](https://medium.com/zuniswap/im-a-happy-fish-don-t-make-me-into-sushi-af55c6f198b9). Describes an ecosystem with different kinds of users ("Whales, Sharks, Dolphins, Turtles, Minnows"). The major concern is collusion between users of a specific type, in a way that harms other users. Specifically the concern is about larger users (whales) harming smaller users (minnows). I think large/small here is defined by how many tokens you own. A naive approach might just give different rules to these different user types, but whales can always make a bunch of small accounts, pretending to be a lot of minnows. So you need a protocol that can account for synthetic minnows. Does not give a solution, just says it would be good to solve this. 

This refers to an [article](https://medium.com/@peterludlow/synthetic-minnows-and-polycameral-governance-f1fa1efb017a) written by EJ Spode (aka [Peter Ludlow](https://en.wikipedia.org/wiki/Peter_Ludlow)). Oddly it was only published two days earlier than the Zuniswap article, which makes me think it must have been circulating in advance -- the Zuniswap team lists Spode as an advisor of sorts so this may not be surprising. Spode proposes that minnows be defined in way that makes them relatively costly to synthesize. For example, you count as a minnow only if your holding in the relevant token is <10%, and your overall account balance under $100k. He also proposes "polycameral governance" -- once you have these user classes defined, you can have a "house of Minnows" and a "house of Whales", as the English system has houses for Lords and Commons.

#### K-Y-Who?

[K-Y-Who?](https://medium.com/zuniswap/k-y-who-de-risking-defi-for-mainstream-ethereum-growth-45d5356d8a7f). Traditional finance focuses on KYC -- Know Your Customer. DeFi is founded in opposition to this idea and what it entails. So it's moved towards total anonymity. This piece pushes instead for KYL -- Know Your Leadership. Explains their approach of being very public with real identities, communicating directly with the community, to increase trust.


#### Community or Collusion?

[Community or Collusion?](https://medium.com/zuniswap/the-defi-conspiracy-is-real-but-is-it-community-or-collusion-2d214603bed). Builds off Vitalik Buterin's piece [Coordination, Good and Bad](https://vitalik.ca/general/2020/09/11/coordination.html). The core idea there is that Cooperation (which we see as good) and Collusion (bad) are both kinds of Coordination. Sometimes there is a tradeoff where making the good thing easier also makes the bad thing easier, and you must be cognizant of this relationship when structuring coordination rules. Zuniswap builds on this and defines community "Roles" to structure the coordination.

### KeyKeyFi

They are also related somehow to a project called [KeyKeyFi](https://github.com/KeyKeyFi/KeyKey). From their Discord:
> @barnyard:
> how is Zuni associated with KeyKey?
> > @zuniswap:
> > @barnyard They are totally distinct projects and tokens, built and maintained by mostly the same core team. Both projects are concerned with implementing community governance on top of an Eth DEX business.

## Goals (Uniswap)

So, what does this project actually try to do? Since it's based on Uniswap, and they have good docs, we'll go there.

The core goal seems to be "liquidity provision" -- ability to swap one kind of token for another. 

The relevant tokens are [ERC-20](https://eips.ethereum.org/EIPS/eip-20). This is the first I'm seeing this term but as best I can tell, it's a common API for Ethereum tokens, and most "reputable" tokens on Ethereum use it. 

From the perspective of a user: you have token `A`, and you want to exchange it for token `B`. In other words, you want `A<>B` liquidity. (This is my notation.)

Uniswap provides this by incentivizing people to create `A<>B pools`. They dump some amount of `A` and `B` into a liquidity pool. In order to make an `A<>B` trade (in either direction), users just need to find an `A<>B pool`. 

Immediate questions: 
1. How is the exchange rate determined?
1. How does the user discover pools?
1. What is the incentive for pool creators (aka "liquidity providers").
1. What is the incentive for Uniswap itself?

Answers to come in part 2.

Links:
* https://uniswap.org/blog/uniswap-v2
* https://uniswap.org/docs/v2/protocol-overview/how-uniswap-works
* https://uniswap.org/docs/v2/core-concepts/pools/
* https://uniswap.org/faq
* Also look into Sushiswap
