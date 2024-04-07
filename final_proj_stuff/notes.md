# Adaptive Independent Metropolis-Hastings
Holden, Lars, Ragnar Hauge, and Marit Holden. "Adaptive independent metropolis–hastings." (2009): 395-413.

*Presenter: Arman Azizyan*

## Motivation and Introduction
- Remember adaptivity from 'generalized adaptive rejection sampling'?
    - Adaptivity comes from keeping track of accept/reject decisions!
    - This is incredibly useful because it allows us to recover from a bad proposal function.
        - If a proposal function is bad, it will still return accurate samples but it will be harder to find samples with high acceptance probability!
        - Adaptivity allows us to use previous accept/reject decisions to improve our proposal function.
- This algorithm uses these idea by updating a history vector, y, and using it in accept/reject decisions (when calculating alpha).
    - Specifically in the parametric case, y can be used to improve estimates on g(x)'s parameters.
- The algorithm is provided by the authors as: ![](adaptive_alg.PNG)

## Challenges
1. If the history vector is very large, this can add significant memory overhead!
    - This poses a tradoff between improving adaptivity and memory.
2. We can't have the proposal function adapt too fast or it will converge too fast and may misspecify the target distribution.
    - The proposal distribution should have heavier tails than the target distribution.
3. Similarly to (2), we need to ensure that we capture all density lumps of our target distribution. If we converge too fast, we'll only capture one or a few of the lumps (or local maximums/modes). To avoid this, there should be *mode jumps*, where proposals are jumped to other parts of the domain in search of other density lumps.
    - This figure is a good example of a target density with multiple lumps: ![](density_lumps.jpg)

## Convergence
There are three important concepts in understanding the authors' proof that this algorithm converges.
1. *Doebin Condition* - This requires that the proposal distribution has heavier tails than the target distribution.
    - More technically, and in the authors' words: ![](dc.png)
2. *Theorem 1 (Invariance)* - The limiting density 𝜋 conditioned on the history is invariant for the adaptive independent Metropolis-Hastings algorithm. That is, once we arrive at a density lump after a jump, the chain will remain there.
3. *Theorem 2 (Convergence)* - If the Doebin condition is satisfied and the total variance is under a certain ratio for all samples, then the algorithm will sample from the target distribution with a probability approaching 1.
    - This is the constraint on the total variance: ![](tv.png)

## Examples
1. The first example is a simple bimodal target distribution.
    - ![](example1.png)
    - The solid line is a random walk and the dashed line is adaptive MH.
    - The 4th plot is ratio of mode over our proposal density

2. The second example has five variables and represents a case where you may have 1000's of variables.
    - In this case, we can use linear regression to estimate f(x).
    - ![](example2.png)

## Questions
- What's the difference between a mode jump and a local jump?
    - Depends on your jump parameters, but for example 1 say we have a triangular proposal distribution. Mode jumps represent movement between mixtures/modes whereas local jumps represent movement within a single mixture/mode.

## Summary
- Adaptive approaches to rejection sampling can improve efficiency. This is an approach to make Metropolis-Hastings adaptive.
- Adaptive algorithms are all about the balance of exploration vs. refinement! If there is too much exploration, the algorithm takes too long or never converges. Too much refinement means the target distribution may not be captured well .