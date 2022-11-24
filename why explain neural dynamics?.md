# why explain neural dynamics?

Two relevant questions

- how do fundamental brain functions emerge from dynamics of complex neural
circuits?
- and what can we learn when building AI?

We will try build models of spatiotemporal dynamics of the brain (at varying scales) to understand how information is being integrated/encoded. This is because the brain is a open non-equilibrium adaptive complex system with emergent spatiotemporal dynamics - which we think underly complex brain function.

Three levels of analysis when trying to work out computational brain function
1. computational (what's the problem (input, output)?)
2. algorithmic (what's the strategy?)
3. implementational (how is it done by a network of neurons)

Example:

1. Problem: Recall events based on partial information
  - our memory is content addressable
  - our memory is associative
2. Algorithm: Perturbation to a dynamical system with fixed point attractors
3. Implementation: Hopfield networks

$$ x_i = sign(\sum_jJ_{ij}x_j) $$

