# self-organised criticality

## computational advantage of criticality

See (Langton, 1990).

- computational properties (information storage, transmission, modification) are more readily apparent in dynamical systems near phase transitions. Particularly critical phase transitions.
	- phase transitions are often towards a chaotic state, leading to term 'edge of chaos'.

**biological systems are often poised near critical phase transitions**:

See (Mora & Bialek, 2011).

This paper finds that data-driven models of dynamics in biological systems often exist at a point of criticality in their parameter space.

## criticality

Original paper: see (Bak, Tang & Weisenfeld, 1987).
Modern review paper: see (Cocchi et al., 2017).

Criticality occurs when a system is poised at the point of a dynamic instability. Because of this, microscopic fluctuations are not damped but instead appear at all scales of the sytem. 
- fluctuations (or 'perturbations', 'noise') describe changes in some parameter space of the system due to external input

This yields power-law fluctuations in the temporal domain ("crackling noise") and the spatiotemporal domain ("avalanches"). 
- fluctuations show a power-law fitting probability distribution; covering several orders of magnitude. 
	- a 'power law' exists between two variables $x$ and $y$ when
$$y \propto x^{-k} $$
- when exponent $k < 2$ in the fluctuations' probability distribution, those fluctuations are said to be 'scale-free'. 

**self-organised criticality**: 
emergent, i.e. occurs without a need to tune an external parameter, (e.g. due to plasticity and memory in a neural system)

**super-criticality vs sub-criticality**:
- at a super-critical bifurcation point, fluctuations are scale-free; on either side of this point fluctuations are quickly damped
	- this is the classic notion of 'criticality'
- at a sub-critical bifurcation point
	- there is a zone around this point where the system can be in any stable state (i.e. system exhibits 'multistability')
		- outside of this zone, the system behaves as above.
	- effects of small noise at this point tend to push the system towards one stable state
		- following a heavy-tailed exponential probability distribution
	- effects of large noise at this point lead to random choice of state
		- following an exponential probability distribution
	- this is *not* the classic notion of criticality since power-law distributions are *not* followed.

*Note, 'supercritical' and 'subcritical' are often used to simply mean above and below a bifurcation point in neuroscience papers, so take care when reading. The above comes from the classic notion of criticality from statistical mechanics in physics.*

## key ideas about criticality in neural systems

> Criticality arises when a system is close to dynamic instability and is reflected by scale-free temporal and spatial fluctuations 
- 'fluctuations', 'perturbations', or 'noise' in parameter space appear at all scales, i.e following power-law probability distribution
  
> Critical temporal fluctuations ('crackling noise') occur in ===simple=== systems close to a bifurcation
- a bifurcation is a point of transition, between two phases that exhibit stable states, where that transition can usually be controlled by a single parameter ('control parameter').
- these temporal fluctuations are also called $1/f$ noise (comes from observed, power-law fitted, frequency spectra)

> Critical spatiotemporal fluctuations (avalanches) occur in ===complex=== systems close to a phase transition  
- these have self-similar, fractal properties.

> Crackling noise and avalanches have now been observed in a wide variety of neuronal recordings, at different scales, in  different species, and in health and disease  

> Computational models suggest a host of adaptive benefits of criticality, including maximum dynamic range, optimal information capacity, storage and transmission and selective enhancement of weak inputs 
- 'maximum dynamic range' i.e., implying wider range of state-space of the dynamical system can be explored
- 'optimal information capacity/storage' i.e., following from above, if total entropy of explorable state-space increases, wider range of meaningful information can be encoded.
- 'optimal information transmission' i.e. system is more sensitive to varying inputs (separation property improves, see [[liquid state machines]])
- 'selective enhancement of weak inputs' i.e., weak inputs still cause meaningful fluctuation

>  Resting-state EEG and fMRI data show evidence of critical dynamics 

> The onset of a specific cognitive function may reflect the stabilization of a particular subcritical state under the influence of sustained attention 

> Mounting evidence and models suggest that several neurological disorders such as epilepsies and neonatal encephalopathy reflect bifurcations and phase transitions to pathological states 

## sandpile model

See (Bak, Tang & Weisenfeld, 1987).

## neuronal avalanches

See (Beggs & Plenz, 2003).

Cascades of spikes ('avalanches') due to external input, in cortical slices from rats, are scale-free. That is, they are power-law distributed in space and duration.

The branching parameter[^1]  was defined as the average number of electrodes activated in the next time bin, given a single electrode being active in the current time bin. critical value was observed to be near 1. On either side, system escapes into explosive growth, or minimal activity (no cascade).

[^1]: (similar in notion to 'control' parameter, but note no external control here, so kind of like the observed control parameter)

## optimal dynamic range of excitable networks at criticality

See (Kinouchi & Copelli, 2006)

uses 'stevens' law' to describe power law.
$$F(S)= C S^m $$
where
- $S$ is the stimulus level
- $C$ is a constant and $m$ is the Stevens exponent

**model**:
each excitable neuron has $n$ states.
$s_i = 0$ is resting
$s_i = 1$ is activated
remaining states $\dots n - 1$ are refractory.

each neuron changes from $0$ to $1$ state, if
- stimulated by a Poisson process that has rate $r$
- if neighbour $j$ was activated in previous time step; neuron $i$ has a probability $p_{ij}$ of firing.

each time step after being in $1$ state, neuron increments its state up to $n - 1$, then returns to $0$ (simulating refractory period)

each neuron is randomly connected to others with some probability

**criticality**:
'local branching ratio': average number of excitations in the next time step, by neuron $j$.
$$ \sigma_j = \sum_i^{K_j} p_{ij} $$
This local branching ratio was the control parameter; neuron $j$ had random $\sigma_j$ drawn from varying normal distributions.

![[local branching ratio distributions kinouchi .png|300]]

Critical regime was observed at mean $\sigma = 1$.

![[criticality in dynamic range kinouchi.png|350]]

left axis $\rho$ is instantaneous density of active sites ($\frac{n_{active}}{n_{total}}$ at $t$).

Below $\sigma = 1$, fluctuations due to external input are quickly damped; cascades do not propagate.
At $\sigma = 1$, fluctuations are damped more slowly. 
Above $\sigma = 1$, activity is self-sustaining, and fluctuations due to external input are quickly damped.

In the critical regime, sensitivity to external inputs are heightened.

![[response to external input in critical regime kinouchi.png|350]]

- rate of external square pulse ($0 \le t \le 300$ms ) input was $0.5 \text{ms}^{-1}$
- top figure shows instantaneous density of all sites, bottom figure shows raster plot of sample of neuronal population

![[response to external input in non-critical regime kinouchi.png|350]]

![[response curves and dynamic range at criticality kinouchi.webp]]

a) average density ($\frac{1}{T} \sum_{t=1}^T \rho_t$) at critical regime was linearly correlated with rate of external square impulse.
	subplot: average density in stable state below $\sigma = 1$, and above.
b) as above, but linear vertical scale.
c) response curve for $\sigma = 1.2$.
d) dynamic range ($10 \log (F_{0.9} / F_{0.1}$)) (i.e. log ratio of average density for $T=0.9$ to $T=0.1$). Maximised in critical regime.

## criticality as a model for high variability in spike times in the cortex

See (Fontenele et al., 2019)

spike times from avalanches under critical regime are power-law distributed.

## criticality in rich-club networks

See (Gu, Qi & Gong, 2019)

- model rich-club connectivity
- critical regime is in between asynchronous and locally propogating wave states
- dynamics in the critical regime include
	- diverse correlation between spike count rates and population activity
	- wave patterns with varying speeds
	- precise temporal patterns of spikes.
- above dynamics in the critical regime appear related to rich-club properties (which are observed in local cortical circuits) of the network.

