# neural coding

External stimuli are encoded in neural activity. The internal representation is then transferred/read-out by other neural systems. Ultimately, presumably, there is a 'decoding' step that transforms the internal representation into a mental operation.

External stimuli -> Internal Representation -> Mental Operation

We consider spikes the carriers or encoders of information in neural systems. The question is how do spike times encode information?

## firing rate coding

See [[single neurons and spike trains]] for three notions of firing rate.

Consider firing rate (spike count rate) of a single neuron in the primary visual cortex of a monkey, responding to changing orientations of a bar.

![[firing rate encodes bar orientation in monkey v1.png|400]]

While the neuron seems to encode the orientation of the bar via firing rate, firing rate coding is limited in reaction time allowed. That is, if the whole neural system was reading out the firing rate of this neuron by averaging the number of spikes across small time windows, that does not leave enough time to react to the stimulus. 

e.g., 
- a fly reacts to new stimuli and changes direction within 30 - 40 ms.
- humans recognise a visual scene in a few hundred ms.

Notice firing rate (spike density), i.e. averaged across trials, would also often be infeasible as a coding mechanism. If a frog wants to catch a fly, it cannot wait for the fly to make repeated identical trajectories before calculating its end point.

## temporal (or 'coherence') coding

#### sequences

Spike trains may encode information by their place in a spatial sequence, as observed in the motor cortex of a songbird, during song. That is, one neighbourhood of neurons spike in a short time window, then another group, then another group, etc. 

Differences in time $\delta$ between each group may also meaningfully encode different stimuli.

'synfire' chains are suggested to produce these sequences, see [[neural coding##synfire chains|below]]. 

#### oscillations and phase

Spike trains might encode information in the phase of a spike event with respect to background oscillation (of population activity).

e.g. phase precession in place cells:
- rat enters a place field
- place cell, for that place field, fires at a particular phase in population's theta oscillation
- traversal through the field leads to phase being shifted to earlier part of theta oscillation
- phase was less correlated with time after entering the place field; more so just the spatial location.
- (place cells are also suggested to encode spatial location via firing rate (spike count rate); phase allows better encoding)
	- higher spatial resolution
	- faster encoding

See how phase is also utilised in pattern recogniton, for sub-threshold oscillations [[neural coding#neural coding#pattern recognition using action potential timing|below]].

#### synchrony

Famous idea that neurons which synchronise their spike events encode objects 'belonging together', i.e. synchronisation labels objects. Also called 'feature binding'.

## pattern recognition using action potential timing

> [!cite]
> Hopfield (1995) https://doi.org/10.1038/376033a0

Consider a set of stimuli $\{a, b, c \dots\}$ , each one characterised by a list of numbers
$\{ X_{a,1}, X_{a,2} \dots X_{a,i} \dots \}$ . 

An unknown stimulus is presented to the neural system with properties $X_u$.

The neural system might then determine for which, if any, of $\{a, b, c \dots\}$ , does

$$X_u \approx \lambda X_a \quad \text{or} \quad
X_{u,i} \approx \lambda X_{a,i} \space \text{for all $i$}$$

and what is the value of $\lambda$. Notice this form implies scale-invariant recognition of a stimulus' quality, and knowledge of its intensity.

Hopfield proposes encoding neurons via oscillating sub-threshold membrane potential, that spike if information relevant to recognised stimuli needs encoding, driven by additional input current.

Cell potential $u$ of the encoding neuron $j$ is given by

$$u_j (t) - u_{th} / R = I_j(t) - I_o - A( 1 - \cos 2 \pi ft )$$

where
- $I_0$ and $A$ are positive constants
- $f$ is the frequency of localised oscillation
![[action potentials encoding pattern recognition under subthreshold oscillation.png|500]]

The time $\tau_j$ each neuron fires, in advance of the maximum subthreshold oscillation, encodes the analogue input current $I_j$. Particularly, $\tau_j(I)$ is a function of the shape of oscillation and input due to stimuli (which may already have been transformed by the neural system).

Hopfield then suggests that an encoding neuron (for a particular stimulus $X_j$), will generate time advances given by

$$\tau_j \approx \ln (\frac{X_j}{\delta + 1}) \approx \ln (\frac{X_j}{\delta})$$

where 
- $\delta$ is a constant scaling factor.

Notice this form of the time advance $\tau_j$ suggests the stimulus $X_j$ is encoded, invariant of its intensity $\lambda$.

Above only describes one action potential per cycle, though more might occur with different regimes of input current, membrane time constant, and oscillation shape. The number of action potentials within a cycle could encode further analogue information.

**How might the pattern be decoded?**
- Encoding neurons detect features - but spike at different times.
- Delays can be used to let the relevant signals arrive at the same time at a *recognition* neuron
	- notice this allows models of plastic delays to learn patterns.


## synfire chains

> [!cite]
> Diesmann et al. (1999) https://doi.org/10.1038/990101

Feed forward arrangement allows generating coordinated sequences.

Neurons in a layer, i.e. with a large shared pool of simultaneously firing input cells, will also produce aligned spike times. Repeating this arrangement, the layer is also a source of synchronous input to the following layer.

The degree of alignment within a layer's spike times, determines whether subsequent layers can reproduce this alignment (or improve it). Otherwise, if spike times are dispersed, the sequence will die out.

We use activity $a$  (the number of spikes in a volley) and temporal dispersion $\sigma$ (the standard deviation of spike times in the volley), to characterise alignment of spike times in a layer. Some time window should be chosen to determine if a spike falls inside a 'volley'.

WIth high enough $a$ and low enough $\sigma$, the synchronisation is improved across feedforward layers ($a$ stays high, $\sigma$ stays low). i.e. the system is an attractor towards synchronised sequence propagation. Otherwise, the system is an attractor towards no sequence propagation (random firing). 

This can be seen in the below state space, with varying $a$ and $\sigma$. Arrows are transformations between feedforward layers.

![[state space of synfire chains.png|400]]

When a layer is not synchronously firing, but is between synchronisation and random firing, close spike times in sub-groups of neurons can be considered 'pulse packets'.

A neuron in the subsequent layer might encode varying pulse packets (i.e. which sub-group of neurons fired together in the previous layer).


