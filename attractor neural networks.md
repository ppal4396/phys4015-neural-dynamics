# attractor neural networks

## definitions

**attractor**:
consider some phase space $X$.
an attractor is a closed set $A$ where
1. $A$ is an invariant set: any trajectory $X(t)$ that begins in $A$ stays in $A$ for a long time
2. $A$ attracts all trajectories that start sufficiently close to it.
	There is an open set of conditions $U$ containing $A$, such that if $X(0) \in U$, then the distance from $X(t)$ to $A$ approaches zero, as $t$ goes to infinity.
3. $A$ is minimal: no subset should satisfy the above.

**basin of attraction**:
from above, this is the largest set $U$.

**fixed point:**
a point which remains fixed under each transition in a dynamic system. this point may or may not be the attractor.

**limit cycle**:
or 'cyclic' attractor, is an attractor that is a periodic and isolated orbit.

**strange attractor**:
when the dynamics due to the basin are chaotic, in that they depend sensitively on initial conditions. the set $A$ is fractal, with complicated structure near any point, under any scale of magnification.

## dynamic systems as computers

See (Bennet, 1990).

this paper 
- introduces how dynamic systems might allow computation by encoding bits inside its variables that are updated by a set of differential equations.
- considers computation as described by the Turing model
- calls for a theory that might describe under what physical conditions dynamic systems could contain universal computation (analagous to Turing completeness), and what the limit of such computation is (analagous to Turing undecidability).
	- i suppose the idea is limited by describing dynamic systems as Turing computers.

## hopfield networks

See (Hopfield, 1982)

this paper 
- searches for how spontaneous collections of simple physical elements might produce emergent computational properties.
- proposes a model for how a physical system that is a multi-stable attractor, might encode a memory in each of its basins of attraction.
	- initial conditions, that are drawn to a particular basin, represent a partial memory. the physical system produces the complete memory.
		- thus allowing content-addressable retrieval

**the model**:
- network of neurons, with random connection probability
- neuron $i$ has state $V_i$ and threshold $U_i$
- synaptic weight matrix stored in $T$ where $T_{ij}$ is the weight of connection from neuron $j$ to $i$.
- neuron $i$ randomly updates itself with rate $W$
	- if $\sum^j T_{ij} V_j > U_i$ then $V_i$ is set to 1, and 0 otherwise.
	- note in this model, state update is done *asynchronously*

**mechanism for information (or 'pattern') storage**:
- to store a pattern, that is, a set of neuronal states $\{ V_1, V_2 , \dots , V_s \}$, we need a weight matrix as follows.
$$T_{ij} = \sum^s (2V_i^s - 1) (2V_j^s - 1)$$
**show the model has stable limit point(s)**:
first note that the above mechanism uses symmetric interactions, i.e. the special case of a synaptic weight matrix where $T_{ij} = T_{ji}$ 

we define $E$ (energy function), for $j \ne i$ (or assume $T_{ii} = 0$)
$$ E = - \frac{1}{2}\sum^i \sum^j T_{ij} V_iV_j $$
now small changes in $E$ due to small changes in $V_i$ appear as
$$\Delta E = - \Delta V_i \sum^j T_{ij} V_j$$
which means that updates to $V_i$ lead $E$ to be monotonically decreasing.
- $\Delta V_i = 0$ means that $\Delta E = 0$
- otherwise RHS will be negative.
 
 State changes therefore continue until a local minimum in $E$ is reached.
- analagous to a Boltzmann machine (stochastic Ising model).

empirically, stability was observed in random networks 
- most often, state sets were stable, with a select few absorbing most of the initial conditions
- sometimes, a simple cycle occurred
- sometimes, chaotic wandering in a small region of state space occurred

### worked example of pattern storage

We assume that a simple Hebbian rule for updating synapses allows patterns to be learned as per Hopfield (above). (Some more complex mechanism would be necessary though.) 

here, we're using $W$ to denote synaptic weight matrix.
also state of neuron $i$ is in $\{-1, +1 \}$ and is now given by
$$\text{sign} \biggl( \sum^j W_{ij} V_j\biggr) $$

**Hebbian learning**:
neurons that fire together, wire together. neurons that fail to synchronise, fail to link.
$$ \Delta W_{ij} = \eta s_i s_j $$

now let's say we have two patterns of neuronal states we would like to store, in a small network of $N = 5$ neurons.

$$\xi^1 = (+1,  -1, +1, +1, -1 ) $$
$$\xi^2 = (+1,  +1, +1, -1, -1 ) $$
We have (from Hopfield) 
$$W_{ij} = \frac{1}{N} \sum^\mu \xi_i^\mu \xi_j^\mu$$
- note where $i = j$, we set $W$ to 0.

In our example this gives $W$
$$\frac{1}{5} \matrix{
0 & 0 & 2 & 0 & -2 \\
0 & 0 & 0 & -2 & 0 \\
2 & 0 & 0 & 0 & -2 \\
0 & -2 & 0 & 0 & 0 \\
-2 & 0 & -2 & 0 & 0 
} $$

**showing the stored pattern is a stable attractor**:
Consider a single pattern $\xi$.
So our weight matrix is simply given by $$W_{ij} = \frac{1}{N} \xi_i \xi_j$$
The next state $V'_i$ of neuron $i$ is given by
$$V'_i = \text{sign} \biggl( 
\xi_i \frac{1} {N} \sum^j \xi_j V_j
\biggr) $$
which means that the next state of neuron $i$ is the exactly the sign of $\xi_i$ multiplied by some factor $m$ given by 
$$ m = \frac{1} {N} \sum^j \xi_j V_j $$
indicating that $V_i$ will approach $\xi_i$ or its opposite sign; there are two stable attractors when there is one pattern stored.

Similar argument can be made for multiple patterns.

### pattern storage with continuous dynamics

See (Hopfield, 1984)

**the paper**:
if states of neurons in the above model are not discrete (0 or 1), but rather continuous, similar arguments of the emergence of pattern storage ability can be made. a systematic link between the discrete and continuous model is drawn.

**the model**:
membrane driving current at neuron $i$ is given by
$$\tau \frac{dI_i}{dt} = 
- \alpha I_i(t) + \sum_{j \ne i}
W_{ij} r_j(t)$$
where $\alpha$ is some constant due to resistance and 
$$r_i(t) = F(I_i(t)) $$
where $F$ is a sigmoidal function, bounded by $r_{min}$ and $r_{max}$.
![[sigmoidal function of membrane current.png|250]]

**mechanism for pattern storage**:
as above, symmetric weight matrix allows this, where stored patterns are still binary.

**showing the model has stable limit points**:
energy function $E$ becomes, for $j \ne i$
$$ E = - \frac{1}{2}\sum^i \sum^j W_{ij} r_ir_j +
\alpha \sum^i \int_{r_0}^{r_i} F^{-1}(r) \thinspace dr + \sum^iI_i r_i $$

see the paper for how small changes in $r_i$ again means that E is monotonically decreasing (equations 7 - 10).

the paper removes the third term from the energy function, afterwards, in order to discuss constraints on $E$. not sure why.

## unstable attractors

see (Milnor, 1985).

assume phase space $X$ is a smooth manifold, and therefore its subsets have a notion of measure $\rho$, that can be zero or positive.

**Milnor attractor** (or 'measure attractor'):
$A$ is an attractor if 
- it is a compact set in $X$
- the basin of attraction, consisting of all points whose trajectories converge towards $A$, has strictly positive measure
$$ \rho(B(A)) > 0$$
- for any proper closed subset of $A$, the set difference between the basin of attraction of $A$, and of this subset $A'$, also has strictly positive measure.

$$ \rho(B(A) / B(A')) >0$$

That is, for some phase space, where each point also has some associated 'measure', the basin of attraction only contains points of positive measure. Further, points of difference between basin of attraction of a subset, and of the whole set $A$, are also of strictly positive measure. This gives a notion that $A$ is non-redundant.
- $A$ is strictly 'minimal' if no subset has a basin of positive measure.
- every dynamical system has a global attracting set $\widehat{A}$, which is the smallest closed set where every trajectory, outside of a set with measure zero, converges towards $\widehat{A}$.

**instability**:

Milnor's definition of an attractor, allows multiple attractors in a dynamic system, whereby states are attracting, but this definition does not imply stability!

We can consider the intersection of two basins to lead to an **unstable manifold**, where small changes to the initial conditions can change the ultimate trajectory of the system.

![[unstable but attracting states.png|300]]

Consider some multidimensional phase space in a neural network then, where conditions move between many attractors, based on external noise/input.

![[multidimensional phase space with unstable but attracting states.png|300]]

this saddle between fixed points (or limit cycles) can be called a 'heteroclinic cycle/orbit'.

### neural computation via heteroclinic cycle

See (Rabinovich et al., 2001)

This paper posits, that during olfactory processing, particular neurons fire in a particular sequence to encode a particular odour. 

this can be considered computation via a heteroclinic cycle, where each memorised spatiotemporal sequence is some place in the *saddle between attractors*, and these *saddle states* 'compete' when a new input is presented.

their observations (experimenting with neuronal recordings from locusts antennae) support a dynamic system that
- is strongly dissipative (trajectories are rapidly changed when a stimulus is turned on)
- represents information using transient trajectories, rather than stable attractors
	- multistability is argued against, since that would suggest that, at rest (with no olfactory stimuli) the system could be encoding some odour.

**model** of firing rate of neuron $i$:

$$\frac{da_i(t)}{dt} =
a_i(t) \biggl[
	\sigma_i(\mathbf{S}) - \biggl( 
	a_i + \sum_{j \ne 1}^N \rho_{ij} (\mathbf{S}) a_j(t)
	\biggr)
\biggr]$$

- where $\rho_{ij}(\mathbf{S})$ is the strength of inhibition of neuron $j$ onto neuron $i$ (for some stimulus?)
- $a_i(t)$ is the firing rate of neuron $i$
- $\sigma_i(\mathbf{S})$ represents this neuron's response to stimulus
	- when -1, there is no stimulus
	- when 1, the stimulus has a component at neuron $i$.

this model showed 
- that stimuli could be encoded temporally and spatially (across varying neurons).
- dynamics were highly dependent on stimulus
	- in a deterministic, reproducible way

![[phase space of 3 neurons with hetero-clinic cycle dynamics.png|350]]

above is a sample of phase space for the activity of three neurons. saddle states exist in the saddles between limit cycles. odours cause a crossing of saddle by the system; the particular trajectory takes the system through a particular saddle state, such that the odour is encoded.

