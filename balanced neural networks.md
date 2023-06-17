# balanced neural networks

A key observation in the dynamics of spiking neural networks, with balanced excitatory and inhibitory nodes, is the emergence of oscillations in population firing rates.

Suppose firing rates $v_E$ and $v_I$ in two interacting populations are as follows.

Excitatory
$$\tau_E \frac{dv_E} {dt} =
- v_E + \bigl[ M_{EE}\cdot v_E + M_{EI} \cdot v_I - \gamma_E \bigr] $$

Inhibitory
$$\tau_I \frac{dv_I} {dt} =
- v_I + \bigl[ M_{IE}\cdot v_E + M_{II} \cdot v_I - \gamma_I \bigr] $$
where
- $\tau$ is the relevant time constant
- $M$ is the relevant synaptic weight matrix
- $\gamma$ is the relevant external input constant

The phase plane of the two firing rates contains a fixed point, i.e. where
$$ \frac{dv_E}{dt} = 0 $$
$$\frac{dv_I} {dt} = 0$$
![[fixed point in balanced neural networks.png|300]]

## fixed point stability

We can characterise the stability of this fixed point, that is whether small perturbations are increased or dampened, as follows.

> [!note]
> Described in Ch. 6.3 Strogatz

Let the two populations' firing rates be $x$ and $y$ and their time derivatives be written as:
$$\dot x = f(x,y) $$
$$\dot y = g(x,y) $$
Suppose that $(x^*, y^*)$ is a fixed point, i.e.
$$ f(x^*, y^*) = 0 $$
$$ g(x^*, y^*) = 0 $$
We can write the components of a disturbance from the fixed point as $u$ and $v$ as follows:
$$ u = x - x^* $$
$$ v = y - y^* $$
To find whether such a disturbance grows or decays over time, we find the differential equations of each component.
$$\dot u = \dot x $$
- since $x^*$ is a constant

Next, substituting $x$ and $y$ in the definition of $\dot{x}$ we get
$$\dot{u} = f(u + x^*, v + y^*)$$
Next, perform a Taylor series expansion.
$$\dot{u} = f(x^*, y^*) + u \frac{\partial f}{\partial x} + v \frac{\partial f}{\partial y} + O(u^2, v^2, uv)$$
- where big O term just denotes the quadratic terms in the Taylor expansion. We will ignore these since $u^2$, $v^2$ and $uv$ are extremely small (since $u$ and $v$ are defined to be components of a small perturbation to begin with)
- partial derivatives are written in short hand but are evaluated at the fixed point $(x^*, y^*)$ (they are numbers not functions)

Next, since $f(x^*, y^*) = 0$ and ignoring the quadratic terms, we get
$$\dot{u} = u \frac{\partial f}{\partial x} + v \frac{\partial f}{\partial y}$$
Identically you can find
$$ \dot{v} = u \frac{\partial g}{\partial x} + v \frac{\partial g}{\partial y}$$
And so the two components of the disturbance $(u,v)$ actually evolve together depending on the Jacobian matrix at the fixed point $(x^*, y^*)$ of the system.
$$\Biggl( \matrix{\dot{u} \\ \dot{v}} \Biggr) =
\Biggl( \matrix{
	\partial f/ \partial x & \partial f/ \partial y \\
	\partial g/ \partial x & \partial g/ \partial y
} \Biggr)_{(x^*, y^*)} 
\Biggl ( \matrix{u \\ v} \Biggr)$$
We can classify the stability of a disturbance by finding the eigenvalues of the above Jacobian matrix as follows.

> [!note]
> Described in Ch. 5.2 Strogatz

Consider generally an initial vector $\mathbf{v}$ that evolves with time. Our goal is to classify whether its trajectory shows simple exponential growth or decay (first assuming that it does show one of these).

That is, we seek trajectories $\mathbf{x}(t)$ of the form
$$\mathbf{x}(t) = e^{\lambda t} \mathbf{v} $$
- where $\lambda$ is a growth rate

Considering that $\dot{\mathbf{x}}$ has the form $\mathbf{A}\mathbf{x}$, we substitute into $\dot{\mathbf{x}} = \mathbf{A} \mathbf{x}$ to get
$$\lambda e^{\lambda t} \mathbf{v} = e^{\lambda t} \mathbf{A} \mathbf{v} $$
simplifying to
$$\lambda \mathbf{v} = \mathbf{A} \mathbf{v}$$
which indicates that the desired trajectories exist if the initial $\mathbf{v}$ is an eigenvector of $\mathbf{A}$ with corresponding eigenvalue $\lambda$ (which was our growth rate).

Therefore finding the eigenvalues (there will be two possible solutions for a $2 \times 2$ matrix) of $\mathbf{A}$ allows us to characterise the growth rate of the evolution of $\mathbf{v}$.
- If the real parts of the eigenvalues are less than zero, trajectory should show exponential decay (dampen).
- If at least one real part is greater than zero, trajectory should show exponential growth (instability).
- If the real parts are both zero, trajectory lead to periodicity.

Plugging $\dot{\mathbf{x}} = \mathbf{Ax}$ back into our system of two populations' firing rates
- $\dot{\mathbf{x}} = (\dot{u} , \dot{v})$
- $\mathbf{A} = \text{Jacobian at the fixed point}$
- $\mathbf{x} = (u, v)$
- $\mathbf{v} = (x^*, y^*)$

From the original definitions of firing rates in the two populations, the Jacobian matrix at the fixed point is
$$\Biggl( \matrix{
	(M_{EE} - 1) /\tau_E & M_{EI} /\tau_E \\
	M_{IE} /\tau_E & (M_{II} - 1) /\tau_I
} \Biggr)$$
Shown in the image below:
- 1) When $Re(\lambda_{1,2}) < 0$, the fixed point is stable and rates undergo dampened oscillations, ultimately reaching the fixed point.
- 2) When $Re(\lambda _{1,2}) = 0$ both rates settle into periodic oscillations, around the fixed point.

![[stable vs oscillatory dynamics around fixed point in balanced networks.png|500]]

## chaos in balanced networks

See (van Vreeswijk and Sompolinsky, 1996).

This model generates irregular firing patterns in two balanced populations.

We have
- $N_E$ excitatory neurons
- $N_I$ inhibitory neurons
- $N_0$ external neurons providing excitatory input.

**Architecture**:
![[architecture of balanced neural networks generating chaos.png|200]]
where
- $J$ is the relevant synaptic weight matrix, between populations.
- $E$ is the relevant input synaptic weight vector.

The connectivity is random and sparse, such that
$$ K << N_E, N_I $$
where there are on average $K$ excitatory, $K$ inhibitory, $K$ external connections to each neuron.

State of neuron is described by $\sigma = \{0, 1\}$
$$\sigma_k^i = \Theta \bigl( u_k^i (t) \bigr) $$
- where $\Theta$ is Heaviside function (sets non-positive values to 0, positive values to 1)
Total synaptic input to neuron at time $t$ is
$$ u_k^i(t) = \sum_{l=1}^2 \sum_{j=1}^{N_l} 
J_{kl}^{ij} \sigma_l^j (t) +
u_k^0 - \theta_k$$
- where $\theta_k$ is neuron's firing threshold

Since individual connections are strong, only $\sqrt{K}$ excitatory inputs to a neuron are needed to cross its threshold. 

Deterministic chaos arises in spike times as follows
- total synaptic input to a cell is overwhelmingly excitatory or inhibitory
	- unless excitatory + inhibitory populations dynamically adjust to cancel eachother
	- this balance emerges, and does not require precise tuning
- at balance, synaptic input's mean and fluctuations are on the order of the threshold.
	- crossing the threshold is determined by fluctuations
	- leading to deterministic chaos

Although single neuron dynamics are non-linear, population activity increases linearly with external input.

This allows the balanced network to quickly track rates of change in external input.
- possible encoding mechanism

Note however, that fluctuations in synaptic input are observed to be very large ('synaptic background noise') in the cortex, unlike in the above model.

## varying states in balanced networks

See (Brunel, 1999)

Membrane potential $V_i$  of a neuron modelled using
$$ \tau \dot{V}_i(t) =
-V_i(t) + RI_i(t)$$
- where $I_i$ is total input current due to synaptic transmission.

When $V_i$ reaches firing threshold $\theta$ , an action potential is emitted by neuron $i$. Membrane potential is reset to $V_r$ after a refractory period $\tau_{rp}$.  

Spikes modelled using
$$RI_i(t) = \tau \sum_j J_{ij} \sum_k \delta(t - t_j^k - D)  $$
- first sum is over different synapses arriving at neuron $i$. 
- second sum is different spikes arriving at synapse $j$, at times $t^k_j  + D$, where $t_j^k$ is emission time from pre-synaptic neuron $j$ and $D$ is its transmission delay.

We model $N_E$ excitatory and $N_I$ inhibitory neurons. Each neuron receives $C$ randomly chosen connections from other neurons in the network.
- $C_E = \epsilon N_E$ from excitatory neurons.
- $C_I = \epsilon N_I$ from inhibitory neurons.
- $C_{\text{ext}}$ from external excitatory neurons.

Network is sparse such that
$$\epsilon = C_E / N_E = C_I / N_I << 1 $$
$J_{ij} = J > 0$ for excitatory synapses.
$J_{ij} = -gJ$ for inhibitory synapses.

We then choose $N_E = 0.8N$, $N_I = 0.2N$.
Notice this means $C_E = 4C_I$ as per above.
We further choose 
- $C_{\text{ext}} = C_E$
- $\tau_E = 20$ ms
- $\theta$ = 20 mV
- $V_r$ = 10 mV
- $\tau_{rp}$ = 2 ms

and external synapses are activated by independent Poisson processes with rate $v_{\text{ext}}$.

A number of stationary populaton activity states emerge in the phase space ($v_{\text{ext}}, g)$.

**stationary states**:

![[phase space external input vs coupling parameter in balanced network.png|350]]

At g = 4, note that excitatory weight exactly balances inhibitory weight, due to the ratio of connections.

- For $g > 4$ and high $v_{ext}$, population activity is Low
- For $g < 4$ and high $v_{ext}$, population activity is High
- For $g > 4$ and low $v_{ext}$, there are three states (Low, Intermediate unstable, almost Quiescent)
- For $g < 4$ and low $v_{ext}$, there are three states (High, Intermediate unstable, almost Quiescent)

![[population activity with varying coupling parameter or external input in balanced networks.png|500]]

Left: population activity with varying $g$. Lines are different external frequencies.
Right: population activity with varying $v_{ext}$. Lines are different $g$.

At $g=4$, population activity sharply decreases.
Interestingly, at $g>4$, population activity depends quasi-linearly on external frequency.

![[CV with varying coupling parameter or external input in balanced networks.png|500]]
Left: population average CV on ISI with varying $g$.
Right: population average CV on ISI with varying $v_{ext}$.

At $g=4$, irregularity in spike times sharply increases.
At very low external frequencies, neuron firing aymptotically approaches Poisson process.

**irregularity vs synchrony**:

![[varying irregularity and synchrony in balanced networks.png|400x400]]
- A: Highly synchronized, neurons firing regularly at high rate, ($g = 3, v_{ext}/v_{th} = 2$)
- B: Fast oscillation in population activity, neurons firing irregularly at rate lower than population activity, ($g = 6, v_{ext}/v_{th} = 4$)
- C: Stationary population activity, neurons firing irregularly ($g = 5, v_{ext}/v_{th} = 2$)
- D: Slow oscillation in population activity, neurons firing irregularly at very low rates  ($g = 4.5, v_{ext}/v_{th} = 0.9$)

same phase space ($v_{ext}, g$) as above, but with these states labeled:

![[phase space with varying synchrony and irregularity in balanced networks.png |350]]
- SR = synchronous regular firing
- SI = synchronous irregular firing
- AI = asynchronous irregular firing

## observations suggesting balanced excitation-inhibition ratio?

### excitatory vs inhibitory balance for tuning to stimuli

a neuron's response to preferred stimuli might be improved by the balance of excitatory/inhibitory input to that neuron.

![[sharpened tuning curve in balanced exc inh input.png|600]]

- a) *left:* Exc and Inh input are balanced for all stimuli (bar rotations). *right:* Exc and Inh input are balanced only for vertical rotation - which leads to sharpened tuning curve.
- b) During stimulus presentations, inhibitory input lags behind excitatory input a few seconds, but they are balanced in weight, for all stimuli (musical notes).
- c) During spontaneous firing (no stimuli), same observation as b).
- d) Gamma oscillations in the hippocampus, excitatory input a little ahead/stronger than inhibitory.
- e) Membrane potential for a pair of nearby/similar neurons.
	- neurons with similar inputs have strong co-fluctuations in membrane potential
	- and very low correlation in spike times.

### equalising excitation-inhibition in cortical visual neurons

See (Xue et al., 2014)

Found EPSC and IPSC across four pyramidal cells in L2/3 in V1 of mice to be balanced, in response to photoactivation of L4 (genetically modified to respond to light here).

![[balanced EPSC vs IPSC in pyramidal cells.png|300]]

Note this holds true for varying PSC size.

### sensory input shifts visual cortical neurons from synchronous to asynchronous state

See (Tan et al., 2014)

Membrane potential at a neuron, with varying input states.

![[membrane potential in v1 based on sensory input state.png|550]]

a) higher population activity (less correlated input activity), async firing in the network (not shown), gaussian fluctuations in membrane potential hovering close to threshold
b) lower population activity (more correlated input activity), sync firing in the network (not shown), non-gaussian fluctuations in membrane potential hovering far from threshold.

note visual drive means input activity is less correlated. it also makes sense that higher population activity is measured in less correlated input activity.

### propagating wave-based theory of variable neural spiking dynamics

See (Keane & Gong, 2015)

- alternative model of irregular spike times to the above, that reconciles two models (excitation-inhibition balance + correlated input).

> We show that propagating wave patterns with complex dynamics emerge from the network model. These waves sweep past neurons, to which they provide highly synchronized synaptic inputs. On the other hand, these  patterns only emerge from the network with balanced excitation and inhibition; our model therefore reconciles the two major models of  irregular neural dynamics. We further demonstrate that the collective dynamics of propagating wave patterns provides a mechanistic  explanation for a range of irregular neural dynamics, including the variability of spike timing, slow firing rate fluctuations, and correlated membrane potential fluctuations


### fractional diffusion theory of balanced heterogeneous neural networks

See (Wardak & Gong, 2021)

- alternative model for chaos arising out of balanced model.

> Based on balanced neural networks with heterogeneous, non-Gaussian features, our analysis reveals that synaptic inputs possess power-law fluctuations in the limit of large network size, leading to a remarkable relation between complex neural dynamics and the fractional diffusion formalisms of nonequilibrium physical systems. We derive a fractional Fokker-Planck equation with analytically tractable  boundary conditions for the network, uniquely accounting for the leapovers caused by the fluctuations of spiking  activity. This body of formalisms represents a fractional diffusion theory of heterogeneous neural networks and  results in an exact description of the network activity states. In particular, the fractional diffusion theory identifies  a dynamical state at which the neural response is maximized as a function of structural connectivity. This state  is then implemented in a balanced spiking neural network and displays rich, nonlinear response properties,  providing a unified account of a variety of experimental findings on neural dynamics at the individual neuron  and network levels, including fluctuations of membrane potentials and population firing rates.

## tutorial: Glauber dynamics and more on deterministic chaos in balanced networks




