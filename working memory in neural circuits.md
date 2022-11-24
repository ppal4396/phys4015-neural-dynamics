# working memory in neural circuits

working memory is to actively hold some information in short-term memory
- can be a perceptual cue, or retrieved from long-term memory store

a characteristic feature may be a persistent pattern of activity, during some delay.

we search for a physical process leading to persistent neural activity that is sustained internally in the brain, rather than driven by inputs from the external world.

## ring attractor model of working memory

See (Compte et al., 2000)

---
**model**:
post synaptic currents $I_{syn}$ due to either AMPA, NMDA or GABA channels are modeled by
$$I_{syn} = g_{syn} s(V - V_{syn})$$
where
- $g_{syn}$ is a synaptic conductance
	- fixed for AMPA, GABA, voltage-dependent for NMDA
- $s$ a synaptic gating variable (for AMPA, GABA or NMDA)
	- AMPA; $s$ steps up 1 instantaneously when a spike occurs in exc pre-synaptic neuron
		- then exponential decay with time constant 2 ms
	- GABA; $s$ steps down 1 instantaneously when a spike occurs in inh pre-synaptic neuron
		- then exponential decay with time constant 10 ms
	- NMDA; see below
- $V_{syn}$ is the synaptic reversal potential

NMDA channel kinetics are modeled by
$$ \frac{ds}{dt} = - \frac{1}{\tau_s} s + \alpha_sx(1-s), \quad 
\frac{dx}{dt} = - \frac{1}{\tau_x} s + \sum_i \delta(t-t_i)$$
where
- $s$ is the fraction of open channels
- $x$ is an intermediate gating variable
- $t_i$ are the pre-synaptic spike times
- $\tau_s = 100\text{ ms}$ is the decay time constant
- $\tau_x = 2 \text{ ms}$ is the rise time constant
- $\alpha_s$ controls the saturation properties at high presynaptic firing frequencies.

cells are spatially distributed on a ring and their position in the ring has a linear relationship with their preferred cue angle. (input to a neuron is meant to encode angle of a peripheral cue in an oculomotor delayed response task).

structured connectivity of the model is shown below. 

![[ring attractor connectivity working memory compte.png|450]]
- the synaptic strength decreases with the difference in the preferred cues of two neurons
	- strong interactions between neighboring neurons and weak interactions between more distant neurons
	- note that very distant neurons are *inhibited*.
- right axis shows coupling strength (pos to neg).
- bottom axis shows distance (spatially and in bar-orientation-preference) of pre-synaptic neuron to post-synaptic neuron 

synaptic weight $W$ between $\theta_i$ and $\theta_j$ given by: ($W$ written as a function)
$$ W(\theta_i - \theta_j) = J^- + (J^+ - J^-) \exp
[ - (\theta_i - \theta_j)^2/2\sigma^2] $$
where
- $\sigma$ is the std in gaussian, as above.
---

underlying process for working memory is proposed to be a tuned network activity state ('bump state') maintained for the delay period

![[bump state ring attractor working memory compte.png|400]]

- bottom axis is time course
- cue period denoted by $C$ (250 ms)
- delay period denoted by $D$ (8.75 sec)
- response period denoted by $R$ (250 ms)
- top figure: raster plot (spike count rate over population shown on right)
	- note the enhanced and localized neural activity triggered by the cue stimulus; persists during the delay period
- middle figure: colour coded spatiotemporal activity (basically just colour coded raster I think)
- bottom figure: as above, but when the cue was less specific. network reaches bump state with same width as middle figure.
- not showing inhibitory interneurons

consider that this attractor model has an energy landscape better described by a ring minimum, rather than fixed point minima. this is because any one of the bumps, depending on cue orientation, may be the persistent state.

![[ring attractor, energy landscape.png|450]]

Persistent states in networks constructed by this model can randomly drift over time, due to noise (background current); robustness to this phenomenon is increased in large networks, with weaker recurrent synaptic connections.

![[random drift but stable bump state ring attractor working memory compte.png|450]]

- top: population vector position versus time
- bottom: variance of the population vector position around the initial stimulation point averaged across trials and plotted versus time. Note the linear trend, similar to a diffusion process

A decrease of the NMDA channel contribution (increase in AMPA) to recurrent synapses gives rise to oscillations in the delay period.

![[oscillations in bump state ring attractor model of working memory compte.png|450]]

- top: spatiotemporal firing pattern with moderate AMPA component in recurrent interactions (NMDA channels contributing to 67% of total recurrent excitatory charge entry)
- middle: zoomed in 500 ms from above - notice oscillations
- bottom: NMDA channel contribution to 50% of total recurrent excitatory charge entry

## delayed, but not persistent, activity for working memory

See (Lundqvist, Herman & Miller, 2018).

this paper argues that delayed bursts are more often observed, rather than persistent activity during the delay period, in oculumotor delayed response tasks.

## theta-gamma code - a storage mechanism with limited capacity

Original paper, see (Lisman and Idiart, 1995)
Modern review, see (Lisman and Jensen, 2013).

this was a nice result for a while because it corresponded to the often observed 7 +/- 2 capacity of working memory in humans. however that capacity (with better defined constraints) has since been reduced to 4, by psychological studies.

activity patterns associated with multiple memories can be stored in a single neural network that exhibits nested oscillations.
- each memory is stored in a different high frequency (~ 40 Hz) subcycle of a low frequency oscillation (~ 5 to 12 Hz) in population activity
- memory-specific subcycles repeat across low freq oscillations
	- depending on short-term changes to membrane excitability
- oscillations could be a timing mechanism for serial processing of short term memories

firing induces afterdepolarisation (ADP)
- consider that firing can increase excitability of post-synaptic neuron, via effects of neuromodulators like serotonin + acetylcholine
	- (in their absence, afterhyperpolarisation occurs)

---
**model:**

Excitatory (pyramidal) cells are modeled as leaky integrate-and-fire neurons. A single 'inhibitory interneuron' is included in the network but not explicitly modeled; it is thought of as being activated by each spike in a pyramidal cell, and in turn inhibits all pyramidal cells.

**alpha function**: $\alpha(t) = A(t / \tau) \times \exp( 1 - t/ \tau)$
- with amplitude $A$ and time constant $\tau$.

membrane potential of neuron $i$, ($V_i(t)$) given by
$$\tau_V \frac{d V_i (t)}{dt} = - V_i(t) +
V^{rest} + V^{osc}(t) + V_i^{ADP}(t) +
V^{inh}(t)$$
membrane potential is reset to $V^{rest} = - 60$ mV when it exceeds $V^{thresh} = - 50$ mV.
$$V_i(t) \approx V^{rest} + V^{osc}(t) + V_i^{ADP}(t) +
V^{inh}(t)$$
$V^{inh}$ has the form of a linear superposition of many inhibitory post synaptic potentials
$$V^{inh}(t) = \sum \alpha(t - t_n) $$
- where $t_n$ is the time of the $n$th spike in the network. $\alpha$ is an alpha function with amplitude $- 4$ mV and time constant $5$ ms.

$V_i^{ADP}$ increases from zero after each action potential in cell $i$, following an alpha function with amplitude $10$ mV and time constant $200$ ms.

$V^{osc}(t)$ follows $B\sin(2\pi f t)$, with $f = 6$ Hz and $B = 5$ mV. This oscillation is sub-threshold.

A memory is inserted through 'informational input' at the negative peak in the theta oscillatory drive. This activates the cell (and evokes an ADP).

---

the ADP allows information storage in a single cell.

![[nested oscillations working memory lisman.png|350]]

- A: 
	- top: a single neuron receives informational input (above threshold), and oscillatory input (below threshold.)
	- bottom: spikes occur at theta frequency, as a result of these two inputs. (arrowhead indicates when informational input is introduced)
- B: four pyramidal cells excite a single inhibitory interneuron 
	- produces feedback inhibition of pyramidal cells
	- note informational input + oscillatory input to pyramidal cells, as above.
- C: a network can have seven cells (as above; or think of each cell as a group receiving the same oscillatory/informational input)
	- each group is activated according to one phase of a theta frequency (since informational input arrives at negative peak)
	- so a whole network can maintain at most seven meaningful groups; beyond that, phases begin to overlap
	- grey dotted regions show seven groups each with their own phase (or 'subcycle'). Notice subcycles repeat for each group.
		- when X is introduced, R is lost
- D: showing two memories (cells or groups as above) stored in the network - before and after removing feedback inhibition. Doing so removes phase information.

Each possible phase has since been referred to as a 'gamma cycle'. The cycle arising from every possible phase of sub-threshold oscillation is called the 'theta cycle'.

![[theta gamma neural code working memory lisman.png|350]]

- circled above: states of the same network during two gamma cycles (active cells are black and  
constitute the ensemble that codes for a particular item)
- different ensembles are active in different gamma cycles.

Some relevant psychological and physiological observations of working memory are shown below

![[psychological observations of capacity in working memory lisman.png|450]]

- A: probability of all correct items with increasing list length, in human short term memory task.
- B: subject responds if a test item is on a list presented just prior. bottom axis is length of list. left axis is responce latency. i.e. showing delayed response that indicates 'scanning' a list stored in memory.
- C: nested oscillations in MEG from human cortex, evoked by acoustic stimulus
- D: nested oscillations from recordings from rat hippocampus
- E: ADP recorded from cortical pyramidal cell (intracortical electrode). large initial deflection is a current pulse injection; then ADP rises slowly, and falls again.

## short term plasticity in working memory

See (Mongillo, Barak & Tsodyks, 2008)

- recurrent network of integrate-and-fire neurons
- the network can store a set of memories
	- each is encoded by a random subpopulation of excitatory neurons
		- connections between exc neurons encoding the same memory are stronger, than neurons that do not encode the same memory
		- can think of this as the result of long-term Hebbian learning, or intrinsic clustering of recurrent connections.
	- inh neurons connected to exc neurons, resulting in competing between memories (one can be represented at a time)

short-term plasticity: synaptic efficacy reflects recent pre-synaptic activity.

short-term depression: caused by depletion of neurotransmitters at the axon terminal of the pre-synaptic neuron

short-term facilitation: caused by influx of calcium into the axon terminal after spike generation which increases the release probability of neurotransmitters

synapses can be STD or STF dominated, or can show a mixture

---
**model**:
plastic synapses (short term facilitation and depression) modelled according to Tsodyks (2000).
- synaptic efficacy is modulated by the amount of available resources, i.e. neurotransmitters, ($x$), normalized so that $0 < x < 1$) and the utilization parameter ($u$) that defines the fraction of resources used by each spike, reflecting the residual calcium level. 
- upon a spike, an amount $ux$ of the available resources is used to produce the postsynaptic current, thus reducing $x$.  This process mimics neurotransmitter depletion. 
- the spike also increases u, mimicking calcium influx into the presynaptic terminal and its effects on release probability. 
- between spikes, $x$ and $u$ recover to their baseline levels ($x = 1$ and $u = U$) with time constants $\tau_D$ (depression time constant) and $\tau_F$ (facilitation time constant), respectively. 
- to reproduce depressing behaviour of cortical synapses use ($\tau_D$ > $\tau_F$); for facilitating, use ($\tau_F$ > $\tau_D$)

so each connection between iaf neurons has $x$ and $u$ following
$$\frac{dx}{dt} = \frac{1 - x}{\tau_D} - u x \delta(t - t_{sp}) $$


$$\frac{du}{dt} = \frac{U - u}{\tau_F} + U(1 - u) \delta(t - t_{sp}) $$

synaptic weight of connection $i$ probably given by $uxA_i$ or something.

network composed of $N_E$ excitatory neurons, and $N_I$ inhibitory neurons. membrane potential of each neuron $i$ given by

$$\tau_m \dot{V_i} = - V_i + I^{rec}_i(t) + I_i^{ext}(t)$$
where driving current due to recurrent connections from $j$ to $i$ is given by the following. (external just some constant for neuron $i$)
$$ I_i^{rec}(t) = \sum_j \hat{J}_{ij}(t) \sum_k \delta
(t - t^{(j)}_k - D_{ij})$$
where $D_{ij}$ stores delays, and $\hat{J}_{ij}$ is the effective weight at time $t$ given by
$$\hat{J}_{ij}(t) = J_{ij} \cdot u_j(t - D_{ij}) \cdot x_j (t - D_{ij}) $$
and plastic synapses (each $ij$) are updated by summing over pre-synaptic spikes
$$\dot{x}_j(t) = \frac{1 - x_j(t)}{\tau_D} - u_j(t) x_j(t) \sum_k\delta(t - t_{sp}) $$
$$\dot{u}_j(t) = \frac{U - u_j(t)}{\tau_F} + U \bigl( 1 - u_j(t) \bigr)
\sum_k \delta(t - t_{sp}) $$

Exc-to-exc connections display short term facilitation (fitting experimental observations with $\tau_F >> \tau_D$)
- $\tau_F = 1.5$ sec and $\tau_D = 0.2$ sec

---

below shows plastic synapse behaviour in depressing (left) or facilitating (right) regimes.

![[std vs stf dominated synapse tsodyks.png|700]]

- bottom: pre-synaptic spike times
- middle: post-synaptic membrane potential over time course
- top: parameters $u$ and $x$ over time course

A specific subpopulation is excited; 'loading' the network with a specific memory.
- the subpopulation activity increases for the duration of the input, changing the internal state of the synaptic connections. 

Reactivation of the memory is expressed as a short burst of synchronized activity ('population spike'), where almost every neuron in the subpopulation fires a spike within an interval of about 20 ms. 
- the network response is memory-specific: the neurons coding for the loaded item produce the above burst; the others stay at baseline activity level.
- the PS also refreshes the memory by producing additional facilitation, thus enabling subsequent memory reactivations. 

In the absence of reactivating signals, the memory fades away over a time scale on the order of $t_F$.  
- appropriately timed external signals are required to extract the memory from 'synaptic form' to 'spiking form'

Memory is loaded in a network as follows (dark gray region).

![[short term facilitation loading of memory in recurrent network mongillo.png|550]]

Later (light gray region) same subpopulation in network bursts spontaneously - a 'reactivation' of the memory.

You can increase the rate of subsequent spontaneous bursts with higher background current.

![[short term facilitation loading of working memory until resource depletion mongillo.png|550]]

the bursts end after $x$ is depleted. 

note that residual calcium (use parameter $u$) as a memory 'buffer' requires low neurotransmitter emission rates.
- memory 'buffer' in the sense that, since $\tau_F$ is so high, $u$ takes longer to recover to $U$, and so there is more calcium/utilisation of $x$, allowing subsequent subpopulation bursts to occur for longer.

multiple memory items can be 'loaded' as above.

![[loading multiple memories in short term facilitation working memory network mongillo.png|550]]
- first memory loaded into neurons 80 - 160, at t = 0
- second memory loaded into neurons 0 - 80, at t = 3
- pretty epic because no population 'misfires'
