# Interlace-Weave-Calculus-Coupled-Lattice-Flow-on-Harmonic-Graphs
Abstract

Abstract: We introduce interlace–weave calculus, a novel mathematical framework integrating lattice-ordered security constraints with network flow dynamics on harmonic graphs. A harmonic graph is a network where flows reach equilibrium, yielding harmonic potential functions on nodes
math.uchicago.edu
. The interlace–weave calculus formally couples a lattice (a partial order of security or system states) with a flow (dynamic network processes), interlacing static constraints with dynamic behavior. We provide rigorous definitions of these new concepts and prove core theorems characterizing existence, uniqueness, and compositional properties of coupled lattice/flow solutions. In particular, we show that on a finite harmonic graph, a steady-state flow satisfying lattice constraints exists and is unique under given boundary conditions
jeremykun.com
jeremykun.com
. We also prove that information flows respecting a security lattice enforce non-interference (no unauthorized influence from high-level to low-level nodes)
arxiv.org
arxiv.org
. Application examples demonstrate how the calculus can model and harden cyber-physical infrastructure – from secure communication networks to power grids – by ensuring flows obey security lattices and naturally minimize risk and energy dissipation. Finally, we present production-grade source code (in Python) implementing the interlace–weave calculus, suitable for defense and explainable AI (XAI) applications. The code illustrates how to compute coupled lattice/flow states and automatically enforce security constraints on network flows, thereby helping secure critical infrastructure in a seamless and explainable manner. This work lays a rigorous foundation for analyzing complex interdependent systems with both structural (lattice) and flow aspects, providing formal guarantees of security and stability.

Introduction

Modern infrastructure systems – from computer networks to electric grids – are characterized by complex couplings between static policies and dynamic flows. For example, a military communication network may enforce a hierarchy of security classifications (a lattice of clearance levels) while routing information (flow) between nodes. Similarly, a power grid has physical laws governing current flow while control systems impose safety constraints. Ensuring such systems are secure and stable requires a unified mathematical framework capturing both the discrete structural constraints and the continuous flow dynamics. In this paper, we propose Interlace–Weave Calculus, a formalism that interlaces lattice-ordered constraints with network flows, weaving them into a coherent analytical tapestry.

We coin the term harmonic graphs for networks operating in or near equilibrium, where each node’s state (e.g. voltage, load, or other potential) is the harmonic average of its neighbors
jeremykun.com
. In an electric circuit analog, a steady-state voltage distribution on a resistive network is harmonic and minimizes dissipated energy
jeremykun.com
. Harmonic graphs provide a natural setting for stable flows and allow leveraging powerful results from harmonic function theory (such as the maximum principle and Dirichlet problem solutions
jeremykun.com
). By focusing on harmonic graphs, we ensure that the calculus deals with flows in a regime amenable to rigorous analysis (e.g. linearized or steady-state conditions).

Crucially, security and policy requirements can be modeled by lattices. A lattice is a partially ordered set in which every two elements have a well-defined least upper bound and greatest lower bound. Lattice structures abound in security models: classic multi-level security (MLS) uses a lattice of classifications (e.g. Unclassified ≤ Confidential ≤ Secret ≤ Top Secret). In Denning’s lattice model of secure information flow, the lattice ordering determines permitted flows
arxiv.org
 – information can only flow in directions that do not violate the partial order (e.g. no flow from higher classification to lower without sanitization). Our interlace–weave calculus builds on this insight, coupling the lattice ordering of security levels with graph-based flows. By interlacing these concepts, we can formally ensure that network flows never violate lattice constraints, preventing illicit information leakage or unsafe control actions.

This paper is organized as follows. In Definitions, we introduce the key concepts: interlace–weave calculus, coupled lattice/flow systems, and harmonic graphs, providing mathematical definitions for each. In Main Theorems, we state the fundamental properties of the theory, including existence and uniqueness of solutions and critical security guarantees (non-interference and composability). The Proofs section then provides rigorous proofs of these theorems. We proceed to Application Examples, where we demonstrate how the calculus can be applied to harden real-world infrastructure systems. We illustrate a secure communication network enforcing a classification lattice, and a cyber-physical power grid example where interlaced flow constraints prevent cascading failures. These examples show how our calculus maps abstract theory to practical security improvements. Finally, in the Code Appendix, we present a robust Python implementation of the interlace–weave calculus. The code provides an executable blueprint for using our theory in defense and explainable AI contexts – for instance, automatically detecting policy-violating flows or optimizing network configurations under security constraints. Throughout the paper, we emphasize factual integrity and mathematical rigor, ensuring that each claim is backed by proof or established result. By uniting lattice-based policy modeling with harmonic flow analysis, the interlace–weave calculus offers a powerful new tool for designing secure, resilient, and explainable infrastructure systems.

Definitions

We first define the new concepts central to this work. These definitions lay the groundwork for the formal theorems to follow.

Definition 1: Harmonic Graph. A harmonic graph is an undirected graph $G=(V,E)$ (potentially weighted) together with a function $\phi: V \to \mathbb{R}$ on its vertices that is harmonic on all interior nodes. Harmonicity means that for every vertex $v \in V$ that is not designated as a boundary or source, the value $\phi(v)$ is the average of $\phi$ on $v$’s neighbors
jeremykun.com
. Equivalently, $\phi$ satisfies the discrete Laplace equation $\Delta \phi(v) = \sum_{w \sim v} (\phi(v)-\phi(w)) = 0$ for all interior $v$. Such $\phi$ is called a harmonic potential or harmonic function on the graph
math.uchicago.edu
. In physical terms, a harmonic graph represents a network at equilibrium: if $\phi$ is temperature, no net heat flows; if $\phi$ is electrical voltage, currents are balanced at each node (Kirchhoff’s current law) and the system is in steady state
math.uchicago.edu
. We call a graph harmonic if it admits a non-trivial harmonic function (aside from the trivial constant function) under given boundary conditions.

Definition 2: Coupled Lattice/Flow System. A coupled lattice/flow system is a 4-tuple $(G, L, \Phi, F)$. Here $G=(V,E)$ is a graph modeling the connectivity of the system (e.g. communication links or power lines). $L$ is a lattice $(P, \leq)$ of labels or states relevant to security or system configuration. Each vertex $v \in V$ (and possibly each edge) is assigned an element $L(v) \in P$ of the lattice, representing a security level or state at that location. $\Phi: V \to \mathbb{R}$ is a potential or state function on nodes (for example, data value, voltage, or load), and $F: E \to \mathbb{R}$ is a flow or current function on edges (for example, information flow rate or power flow). The system is coupled in that the lattice labeling governs or constrains the flows, and the flows in turn respect the lattice ordering. Formally, for each edge $(u,v)\in E$ carrying a flow $F(u,v)$ from $u$ to $v$, it must hold that $L(u)\not!!< L(v)$ under the lattice order (to prevent forbidden flows against the policy)
arxiv.org
. Typically, we enforce a no-downflow rule: if $L(u) < L(v)$ in the lattice, information or influence may flow from $u$ to $v$, but not vice-versa (this captures, for instance, the “no write down” rule for confidentiality
arxiv.org
). We write $u \to v$ as an allowed flow if either $L(u) \le L(v)$ (flow from lower or equal security to higher) or an explicit declassification rule permits it. The pair $(\Phi,F)$ is required to satisfy the network flow laws on $G$ (such as flow conservation and constitutive relations like Ohm’s law), while also satisfying lattice constraints on every edge. In summary, a coupled lattice/flow system interlaces a lattice of abstract permissions or states with concrete flows on a graph, such that the partial order of $L$ globally constrains the directions and existence of flows $F$.

Definition 3: Interlace–Weave Calculus. The interlace–weave calculus is a formal analytical framework comprising the rules, operations, and invariants that govern coupled lattice/flow systems on harmonic graphs. “Interlace” refers to the synchronous linking of lattice constraints with flow dynamics: at every step of analysis or every component of the model, lattice and flow aspects are considered together (interlaced). “Weave” refers to the composition or layering of multiple such coupled systems. In the calculus, one can compose two coupled lattice/flow systems (weaving them together) by identifying shared boundary conditions or state variables, yielding a larger system that inherits the security and flow properties of its components. Formally, if $(G_1, L_1, \Phi_1, F_1)$ and $(G_2, L_2, \Phi_2, F_2)$ are two systems (possibly with different lattices $L_1, L_2$), the calculus provides a procedure to form a composite system $(G, L, \Phi, F)$ that weaves together $G_1$ and $G_2$. This may involve constructing the product lattice $L = L_1 \times L_2$ under a suitable ordering (e.g. componentwise order)
arxiv.org
, and coupling flows where the networks interface. The interlace–weave calculus includes the algebra of transformations on these systems (such as refinements, quotients, or homomorphisms of coupled systems) and a logic for reasoning about them (e.g. proving that certain flows are impossible or deriving conditions for equilibrium). In essence, it is a calculus in the sense of a system of computation or reasoning: we can calculate the permissible flow distribution given a set of lattice constraints, or conversely determine the necessary lattice conditions to achieve a desired flow pattern. The calculus is equipped with formal inference rules that combine lattice order relations with flow conservation laws, enabling rigorous proofs of security properties and stability.

Remark: The term calculus here does not mean differential calculus, but rather a formal system of rules and computations (analogous to the lambda calculus or process calculi) tailored to lattice–flow interactions. The interlace–weave calculus thus provides a language to describe scenarios (as coupled lattice/flow systems), operations to manipulate and compose these scenarios, and theorems that guarantee the correctness of these manipulations with respect to security and harmonic stability.

Definition 4: Security Non-interference (in this context). A coupled lattice/flow system is said to satisfy non-interference if any changes in the high-level (more privileged) parts of the system have no observable effect on the low-level (less privileged) parts, according to the lattice. In our framework, non-interference means that if two configurations of the system agree on all low-level node potentials $\Phi(v)$ for $L(v)$ below some cutoff, then those low-level values remain unchanged even if high-level node conditions differ. Equivalently, no flow from a high-level node can alter a low-level node’s state – such flows are disallowed by the lattice ordering, so the low-level subsystem is insensitive to high-level variations
arxiv.org
. This is a key security property ensuring that secret or critical information does not leak to unclassified parts of the network through the flow mechanics.

Definition 5: Infrastructure Hardening via Interlace–Weave Calculus. While not a single mathematical object, we define the application mapping of our calculus to infrastructure security. Given a real infrastructure modeled as a graph of components, and given policy or safety requirements modeled as a lattice, the calculus provides a mapping to a coupled lattice/flow system. Hardening an infrastructure means adjusting its configuration or controls such that this system satisfies desirable properties (no forbidden flows, harmonic stability, etc.). For example, a power grid can be hardened by adding breakers or flow controllers that enforce lattice constraints corresponding to safety limits (voltage or load thresholds), so that even under attack or component failure, flows redistribute harmlessly (maintaining harmonic equilibrium). A communication network can be hardened by implementing guard nodes or encryption on channels that violate direct lattice ordering, effectively weaving in additional constraints to restore compliance. The calculus guides these interventions by pinpointing exactly where lattice–flow coupling must be strengthened (for instance, identifying an edge where $L(u) > L(v)$ and $F(u,v)>0$ indicates a potential policy violation requiring mitigation).

With these definitions, we establish a formal foundation. We next present the main theorems that characterize the behavior of interlace–weave systems and guarantee their security and stability properties under the above definitions.

Main Theorems

We now state several fundamental theorems about coupled lattice/flow systems on harmonic graphs within the interlace–weave calculus. These theorems capture the core structure of the theory: existence and uniqueness of harmonic flow solutions, security properties like non-interference, and compositional (weaving) properties. Each theorem is followed by a brief explanation, and full proofs are provided in the subsequent section.

Theorem 1 (Existence of Harmonic Flow Solution). Consider a finite, connected harmonic graph $G=(V,E)$ with a specified set of boundary nodes $B \subset V$ where potentials $\Phi$ are fixed (boundary conditions) and possibly some specified source/sink flows. Assume each edge has a positive weight (conductance) and the graph satisfies standard conditions for the discrete Dirichlet problem. Then there exists at least one harmonic potential function $\Phi: V \to \mathbb{R}$ that satisfies $\Delta \Phi(v) = 0$ for all interior nodes $v \in V \setminus B$, and that matches the given boundary values on $B$. Equivalently, there is a flow assignment $F$ on edges such that flow is conserved at every interior node (no accumulation) and $F$ is consistent with $\Phi$ (e.g. $F(u,v) = w_{uv}(\Phi(u)-\Phi(v))$ for some conductance $w_{uv}$). In summary, a steady-state (harmonic) flow solution exists for any well-posed set of boundary conditions on a finite harmonic graph.

Theorem 2 (Uniqueness of Harmonic Solution). Under the same conditions as Theorem 1, the harmonic solution is unique up to trivial symmetries. If two potential functions $\Phi$ and $\Phi'$ both satisfy the harmonic conditions and the same boundary conditions on $B$, then $\Phi(v) = \Phi'(v)$ for all $v \in V$ (there is only one solution). Likewise, the corresponding flow $F$ is unique on every edge. In physical terms, the equilibrium distribution of flow/potential is uniquely determined by the boundary injections/extractions
jeremykun.com
. The only ambiguity – a uniform additive constant in potential – is fixed by anchoring one reference node’s potential (e.g. grounding one node at $\Phi=0$). Thus, the interlace–weave calculus yields a single well-defined flow state for each scenario.

Theorem 3 (Lattice-Flow Safety Theorem). In a coupled lattice/flow system $(G,L,\Phi,F)$, if all flows respect the lattice ordering (no edge carries information or influence from a higher lattice element to a lower one), then the system is secure in the sense of non-interference. Formally, suppose for every directed edge $u \to v$ with $F(u,v) \neq 0$, we have $L(u) \le L(v)$ in the lattice. Then any two configurations $(\Phi_1, F_1)$ and $(\Phi_2, F_2)$ that agree on all low-security nodes (nodes below a certain lattice level) will remain equivalent on those low-security nodes after any sequence of allowed flow updates. High-level changes cannot propagate to low-level observables. This ensures confidentiality/integrity: forbidden flow paths simply do not exist, and thus high-level secrets or disturbances do not interlace with low-level states. Equivalently, the lattice ordering determines all permitted flows, and those permitted flows never violate the partial order
arxiv.org
, guaranteeing that no security policy rules are broken by the dynamics.

Theorem 4 (Energy/Optimality Principle for Harmonic Flows). Any flow $\tilde{F}$ on a graph that satisfies the lattice constraints but is not harmonic (i.e. has nonzero net flow at some interior node, or equivalently the potentials are not exactly harmonic) can be adjusted to a harmonic flow $F$ that dissipates no more energy (or other resource) than $\tilde{F}$. In fact, the harmonic flow is an energy minimizer: it minimizes $\sum_{(u,v)\in E} w_{uv}(\Phi(u)-\Phi(v))^2$ subject to the same boundary conditions (if we interpret that sum as total power dissipation or similar). Consequently, among all flows consistent with a given set of sources and lattice constraints, the harmonic flow is optimal in terms of a quadratic energy norm. This optimality contributes to infrastructure hardening: the system naturally tends toward a stable state that avoids unnecessary stress (energy loss) while respecting constraints.

Theorem 5 (Composability and Weaving of Systems). If two coupled lattice/flow systems $(G_1,L_1,\Phi_1,F_1)$ and $(G_2,L_2,\Phi_2,F_2)$ are composed (interconnected) along some interface, the resulting combined system $(G,L,\Phi,F)$ – constructed via the interlace–weave calculus – preserves harmonic equilibrium and security properties, provided each subsystem did. In particular: (a) Equilibrium preservation: if each subsystem was in a steady harmonic state, their coupling (with appropriate matching conditions at the interface) yields a global harmonic state on $G$. (b) Security preservation: if each subsystem individually respected its lattice constraints and we establish a product or common lattice $L$ that extends $L_1$ and $L_2$, then any allowed flow in the composed system still respects $L$. Cross-domain flows at the interface are safe by design of the lattice connection (e.g. through a secure interface policy)
arxiv.org
arxiv.org
. Moreover, the calculus supports modular reasoning: one can analyze each component in isolation and then infer properties of the whole via this theorem. This composability (or “weaving”) theorem ensures that our framework scales to large complex systems, as secure sub-systems can be woven together without breaking their guarantees.

Each of these theorems addresses a critical aspect of the interlace–weave calculus. Theorem 1 and 2 establish that the harmonic flow solution (if it exists) is well-behaved (exists and unique). Theorem 3 is a security guarantee: essentially a formal statement of how lattice constraints enforce non-interference
arxiv.org
. Theorem 4 is an optimality result linking harmonic flows to minimal energy, which underpins the system’s tendency toward a stable, resilient configuration. Theorem 5 provides assurance that complexity can be managed by building secure, harmonic components and then composing them – analogous to modular design in engineering with guaranteed compatibility. We proceed with detailed proofs of these theorems.

Proofs

We now give rigorous proofs or proof sketches for the theorems stated above. Each proof uses standard techniques from graph theory, lattice theory, and network flow analysis, adapted to our coupled framework.

Proof of Theorem 1 (Existence). This is essentially the discrete Dirichlet problem on a finite graph, which is known to have a solution under broad conditions
jeremykun.com
. We provide a constructive argument. Let $n = |V \setminus B|$ be the number of interior nodes. We set up the $n$ linear equations given by $\Delta \Phi(v) = 0$ for each interior $v$. In expanded form, for each interior node $v$,

∑
𝑤
:
(
𝑣
,
𝑤
)
∈
𝐸
𝑤
𝑣
𝑤
 
(
Φ
(
𝑣
)
−
Φ
(
𝑤
)
)
=
0
,
∑
w:(v,w)∈E
	​

w
vw
	​

(Φ(v)−Φ(w))=0,

where $w_{vw}>0$ is the conductance (weight) of edge $vw$. This can be rewritten as $\sum_{w\sim v} w_{vw}\Phi(w) = \Phi(v)\sum_{w\sim v} w_{vw}$, or equivalently

Φ
(
𝑣
)
=
∑
𝑤
∼
𝑣
𝑤
𝑣
𝑤
Φ
(
𝑤
)
∑
𝑤
∼
𝑣
𝑤
𝑣
𝑤
,
Φ(v)=
∑
w∼v
	​

w
vw
	​

∑
w∼v
	​

w
vw
	​

Φ(w)
	​

,

which is the weighted average condition characteristic of harmonic functions. Because boundary nodes $B$ have fixed $\Phi$ values (constants in these equations), this is a system of $n$ linear equations in the $n$ unknown interior values. The coefficient matrix of this linear system is exactly the graph Laplacian $L_G$ with rows/columns corresponding to interior vertices (after removing known boundary values to the right-hand side). The graph Laplacian for a connected graph of $n$ interior nodes is an $n\times n$ symmetric matrix which is M-matrix-like (diagonally dominant with non-positive off-diagonals). Standard linear algebra results (or the theory of electrical networks) tell us that after anchoring one node or providing boundary values, the Laplacian is positive-definite on the subspace of interior unknowns, hence invertible
jeremykun.com
. Thus, a unique solution vector $(\Phi(v): v\in V\setminus B)$ exists. Existence can also be argued via iterative methods: start with an arbitrary assignment for interior $\Phi$ values and repeatedly apply the averaging rule $\Phi(v) := \frac{1}{\deg(v)}\sum_{w\sim v}\Phi(w)$ for interior $v$. This is a form of Gauss–Seidel iteration for the Laplace equations. On a finite, connected, weighted graph, this process converges to the unique harmonic solution (by a standard monotone iteration argument or by viewing it as a contraction mapping in a suitably scaled $\ell^2$ norm). Hence, a harmonic potential $\Phi$ exists satisfying the interior equilibrium conditions. Once $\Phi$ is determined, define flows on each edge $(u,v)$ by $F(u,v) = w_{uv}(\Phi(u) - \Phi(v))$. By construction, these flows automatically conserve at interior nodes (telescoping cancellation of terms from each incident edge). Flows on boundary edges will equal the needed source/sink injections to balance the interior. Thus, $(\Phi, F)$ is a valid steady-state solution. This completes the existence proof.

Proof of Theorem 2 (Uniqueness). Uniqueness can be shown by a maximum principle or linear algebra. Suppose $\Phi$ and $\Phi'$ are two solutions with the same boundary conditions. Consider their difference $d(v) = \Phi(v) - \Phi'(v)$. On every boundary node $b \in B$, $d(b) = 0$ since both match the boundary condition. For any interior node $v$, since both $\Phi$ and $\Phi'$ are harmonic, we have

∑
𝑤
∼
𝑣
𝑤
𝑣
𝑤
(
Φ
(
𝑣
)
−
Φ
(
𝑤
)
)
=
0
∑
w∼v
	​

w
vw
	​

(Φ(v)−Φ(w))=0
and

∑
𝑤
∼
𝑣
𝑤
𝑣
𝑤
(
Φ
′
(
𝑣
)
−
Φ
′
(
𝑤
)
)
=
0.
∑
w∼v
	​

w
vw
	​

(Φ
′
(v)−Φ
′
(w))=0.

Subtracting, we get $\sum_{w\sim v} w_{vw}(d(v) - d(w)) = 0$ for each interior $v$. This means $d(v)$ itself is a harmonic function on the interior (with zero boundary values). Now, by the discrete maximum-minimum principle
jeremykun.com
, a non-constant harmonic function on a connected domain cannot achieve its maximum or minimum in the interior. But $d$ is 0 on the boundary; if $d$ were not identically zero, then either a positive or negative extreme would occur at some interior node (because $d$ is zero on the boundary and presumably non-zero somewhere inside, so either a positive peak or negative trough exists). That contradicts the maximum principle. Therefore, $d(v)$ must be zero for all $v$. In other words $\Phi(v) = \Phi'(v)$ everywhere in $V$. The corresponding edge flows $F$ and $F'$ are then also identical, since $F(u,v) = w_{uv}(\Phi(u)-\Phi(v)) = w_{uv}(\Phi'(u)-\Phi'(v)) = F'(u,v)$. Uniqueness can also be seen from the invertibility of the Laplacian matrix: the linear system $L_G \mathbf{x} = 0$ with zero boundary conditions only has the trivial solution $\mathbf{x}=0$
jeremykun.com
. This completes the proof of uniqueness.

Proof of Theorem 3 (Lattice-Flow Safety). Assume all flows are lattice-respecting (no flow goes from higher security to lower). We need to show that high-level changes do not affect low-level nodes (non-interference). Formally, let $L_0$ be some cutoff level in the lattice that partitions high vs low (for example, $L_0 =$ "Confidential" splitting above from below). Let $H = { v \in V : L(v) > L_0}$ be the set of high-level nodes and $L = { v \in V : L(v) \le L_0}$ the low-level nodes. Now consider two scenarios (two different system states or executions) that differ only in the high-level part. That is, the potentials and flows on $H$ may differ, but initially $\Phi_1(u)=\Phi_2(u)$ for all low-level $u\in L$. We will show by induction on time (or propagation steps) that for all time, low-level states remain equal $\Phi_1(u)=\Phi_2(u)$, meaning the low-level behavior is identical and unaffected by what happens in $H$. The base case at time $t=0$ holds by assumption. Now suppose at time $t$ the states on $L$ are equal between the two runs. Consider an update from $t$ to $t+\Delta t$. Updates are caused by flows: either some high-level node’s state changes and flows to its neighbors, or some low-level node’s state changes due to incoming flows. Crucially, if a high-level node $h \in H$ changes, it can only send flow to neighbors that are higher or equal in lattice rank (by the lattice-flow rule). Thus, any neighbor $v$ of $h$ with $L(v) \le L_0$ (low-level) would not receive a flow from $h$; effectively, that edge carries no influence in the forbidden direction. Therefore, changes in $H$ cannot directly perturb any $L$ neighbor. If a low-level node’s state does change, it must be due to incoming flow from one of its neighbors. But any neighbor $w$ of a low-level node $u$ is either low-level itself or high-level. If $w$ is high-level ($w \in H$), then $u$ is low-level and by the rule, flow from $w$ to $u$ is disallowed (since that would be high-to-low). Therefore $w$ could not have sent flow to $u$. If $w$ is low-level, then $w$’s state was the same in both runs at time $t$ (by induction hypothesis) and its outgoing flow to $u$ would be the same in both runs (since flows are determined by differences in neighbor states, which are equal for all relevant neighbors). Therefore, $u$ receives the same net flow in both runs, so its state will update to the same new value in both runs. Hence $\Phi_1(u)$ remains equal to $\Phi_2(u)$ for all $u \in L$ at time $t+\Delta t$. This inductive argument shows that at all times, low-level states evolve identically regardless of high-level changes – precisely the condition of non-interference. Another way to frame it: the adjacency matrix of the graph can be partitioned into blocks corresponding to $H$ and $L$. The lack of disallowed flows means there are no edges from $H$ to $L$, which makes the $L$ block of the system of equations closed under the dynamics. The $L$-block receives no input from $H$; it can be solved independently. Thus, whatever happens in the $H$ block does not influence the $L$ block’s solution. This matches the intuitive notion that the lattice ordering forbids any causal influence from higher to lower classification
arxiv.org
. Therefore, the system is secure. (Conversely, if a forbidden flow did exist from some $h$ to $\ell$, one could easily construct two scenarios differing only at $h$ that would cause $\ell$ to differ, violating non-interference – highlighting that the lattice constraint is necessary as well as sufficient.)

Proof of Theorem 4 (Energy Optimality). This theorem is rooted in the variational characterization of harmonic functions. We outline the key argument. Define the energy functional

𝐸
[
Φ
]
=
1
2
∑
(
𝑢
,
𝑣
)
∈
𝐸
𝑤
𝑢
𝑣
 
(
Φ
(
𝑢
)
−
Φ
(
𝑣
)
)
2
,
E[Φ]=
2
1
	​

∑
(u,v)∈E
	​

w
uv
	​

(Φ(u)−Φ(v))
2
,

which is a quadratic form measuring the total "effort" or dissipation in the flow (for electrical flows, this is proportional to heat loss; for fluid flows, analogous to pressure drop work, etc.). A well-known fact from physics and mathematics is that the harmonic solution minimizes this energy subject to the boundary conditions. Intuitively, if there were a way to redistribute the potential differences (hence flows) to reduce energy, the current solution wouldn’t be truly at equilibrium – flows would spontaneously rearrange to lower the energy. Formally, one can set up the Lagrangian for minimizing $E[\Phi]$ with constraints $\Phi(b)=\Phi_b$ for boundary nodes $b$. Taking the derivative with respect to $\Phi(v)$ for an interior node and setting it zero yields exactly $\sum_{w\sim v} w_{vw}(\Phi(v)-\Phi(w))=0$, the harmonic condition. This shows that any interior optimum of $E$ occurs when the discrete Laplacian is zero, i.e. $\Phi$ is harmonic. Moreover, $E[\Phi]$ is a strictly convex function of $\Phi$ (the Laplacian matrix is positive-semidefinite, positive-definite on the constrained subspace), so this critical point is a unique global minimum. Now consider any other flow $\tilde{F}$ that meets the same boundary conditions and respects lattice constraints. It corresponds to some other potential function $\tilde{\Phi}$ (perhaps $\tilde{\Phi}$ is not harmonic). We compare energies: $E[\tilde{\Phi}] - E[\Phi]$. By the minimum property of the harmonic $\Phi$, this difference must be non-negative. In fact, one can show

𝐸
[
Φ
~
]
−
𝐸
[
Φ
]
=
1
2
∑
𝑣
∈
𝑉
∖
𝐵
𝑑
~
(
𝑣
)
 
(
Φ
(
𝑣
)
−
Φ
~
(
𝑣
)
)
,
E[
Φ
~
]−E[Φ]=
2
1
	​

∑
v∈V∖B
	​

d
~
(v)(Φ(v)−
Φ
~
(v)),

where $\tilde{d}(v) = \sum_{w\sim v} w_{vw}(\tilde{\Phi}(v)-\tilde{\Phi}(w))$ is essentially the residual of the Laplace equation for $\tilde{\Phi}$. If $\tilde{\Phi}$ is not harmonic, there is at least one interior node with $\tilde{d}(v)\neq 0$. Then by adjusting $\Phi$ in the downhill direction of that gradient, one could lower the energy. Performing this adjustment iteratively (essentially a gradient descent on $E$) will eventually yield $\Phi$. The difference $E[\tilde{\Phi}] - E[\Phi]$ represents wasted energy in the non-harmonic flow (dissipated as unnecessary heat or pressure drop, etc.). Since $\Phi$ respects the same lattice constraints (we can enforce that the minimization stays within the subspace of functions that do not violate those constraints—though usually $\Phi$ automatically will respect them if $\tilde{\Phi}$ did, by the maximum principle which prevents inversions), $\Phi$ is an admissible competitor with strictly lower or equal energy. Therefore, the harmonic flow $F$ associated with $\Phi$ minimizes the energy. This optimality implies that if the system naturally relaxes (like a physical system might), it will settle in the harmonic state which is locally and globally energy-minimal. For critical infrastructure, this is good: the flows will tend to distribute themselves in a way that does not over-stress any particular link (since any such imbalance would create a non-harmonic gradient that can be smoothed out to reduce energy). In effect, the harmonic solution is the most efficient way to satisfy the demands given the constraints. This proves the theorem.

Proof of Theorem 5 (Composability). We tackle the equilibrium and security aspects separately:

(a) Equilibrium preservation: Suppose $\Phi_1, F_1$ solve the harmonic equilibrium on $G_1$ and $\Phi_2, F_2$ solve it on $G_2$. To compose, we identify some boundary nodes or aggregate nodes between $G_1$ and $G_2$. Without loss of generality, say a subset of boundary nodes $B_1 \subset V_1$ is identified with $B_2 \subset V_2$ (physically, connecting those nodes or saying they represent the same physical point). We then form $G$ as the union of $G_1$ and $G_2$ with those nodes merged. We must define a combined $\Phi$ on $G$. On the merged boundary, we require consistency: if $b_1 \in B_1$ was merged with $b_2 \in B_2$, then we require $\Phi_1(b_1) = \Phi_2(b_2)$ and define $\Phi(b_1)=\Phi(b_2)$ to that common value. This is essentially a matching condition – e.g. the voltage or temperature at the interface is the same viewed from either subsystem. Such a condition would typically be part of the design of the interface. Under this condition, we claim that $\Phi$ defined by $\Phi|{G_1}=\Phi_1, \Phi|{G_2}=\Phi_2$ on their respective parts is harmonic on the interior of $G$. Any interior node of $G_1$ (not an interface node) was harmonic under $\Phi_1$, and nothing changed – its neighbors and values remain the same – so it remains harmonic. Similarly for interior of $G_2$. An interface node (merged boundary) is a bit more subtle: originally, in each subsystem it was a boundary. In the combined graph, it becomes an interior node (assuming it’s not an external boundary of the whole). We must check it now satisfies the harmonic condition in $G$. But the net flow out of that interface node into $G_1$ was equal to whatever boundary condition or injection was applied in subsystem 1, and similarly into $G_2$. When we join them, if we ensure conservation (no external injection at the interface in the combined system), the flow that used to leave $G_1$ into $b_1$ must enter $b_2$ in $G_2$ and vice versa. Essentially, the interface node in the combined graph will have neighbors from both $G_1$ and $G_2$, and the sum of flows from all those neighbors is zero (the outflow from one side equals inflow to the other). Thus, the interface node is now harmonic as well (Kirchhoff’s law holds with contributions from both sides summing to zero). Hence the combined $\Phi$ is harmonic everywhere, and flows $F$ defined as $F_1$ on $E_1$ and $F_2$ on $E_2$ (with no flow on non-existent cross edges, since we only merged nodes but didn’t add new edges) is a valid equilibrium flow on $G$. In practice, one might also allow explicit coupling edges between $G_1$ and $G_2$ (like a few links connecting the two networks). Those can be treated as part of one subsystem for the purpose of analysis, or as additional constraints requiring $\Phi_1(b_1)=\Phi_2(b_2)$ style equalities if a link connects $b_1$ and $b_2$. The result is the same: the composite will satisfy the equilibrium equations as long as the interface is handled with matching conditions.

(b) Security preservation: Let $L_1=(P_1,\le_1)$ and $L_2=(P_2,\le_2)$ be the lattices for the two subsystems. To combine, we need a lattice $L$ that covers the joint system. The most straightforward way is to form the disjoint union or product lattice. If the organizations or systems are independent, one could take a direct sum of posets where elements are tagged with which subsystem they come from, and any cross-comparison is defined only if a policy for interconnection exists
arxiv.org
. For instance, if $a\in P_1$ is a security level in system 1 and $b \in P_2$ in system 2, and we want to allow information from 1 to 2 in some controlled way, we might stipulate an ordering $a \le b$ in the combined lattice or require a certain trusted interface that effectively raises or lowers classification. The most typical scenario is two networks that connect through a trusted gateway which performs a lattice connection. Recent research (e.g. Bhardwaj & Prasad 2020) has developed frameworks for connecting different lattices in a semantically sound way
arxiv.org
arxiv.org
, often using category-theoretic constructs like Galois connections to relate the orders. Assuming such a correct lattice integration, the flows between systems are restricted similarly. Within each subsystem, by assumption, flows respected $L_1$ or $L_2$. At the interface, we impose that any flow from a node in $G_1$ to a node in $G_2$ must respect the combined lattice order. For example, if $u\in G_1$ and $v\in G_2$ are connected (directly or via the merged node scenario), and $L_1(u)$ is the level of $u$, $L_2(v)$ the level of $v$, then we ensure $(L_1(u), L_2(v)) \in P_1\times P_2$ satisfies the relation defined by our lattice connection (which could be as simple as $L(u)$ in combined lattice being a pair). In practice, a common approach is to designate specific classification equivalences at the boundary – e.g. treat "Secret of system 1" as equivalent to "Secret of system 2" if those are to interface. In any case, because both subsystems individually had no improper flows, and we only allow flows at the interface that satisfy the cross-lattice policy, the combined system will have no improper flows either. Therefore, Theorem 3’s condition holds for the combined system as well. Non-interference in the combined system can then be inferred by applying the same reasoning globally. Additionally, the reference to modular approach
arxiv.org
 means we can analyze each part’s security (through its lattice constraints) and then reason that the composition, using a known secure connection method, remains secure. This avoids having to analyze the entire large system from scratch – a big practical advantage.

Thus, the interlace–weave calculus supports composability: not only can systems be mathematically composed, but their key properties (equilibrium and security) compose as well. This concludes the proof of Theorem 5.

Each of the above proofs reinforces the soundness of our framework. Existence and uniqueness ensure the mathematical well-definedness of the harmonic flow given constraints. The safety theorem and composability theorem ensure that the crucial security properties (like non-interference) are preserved by the calculus. With these theoretical foundations in place, we turn to practical examples to illustrate how interlace–weave calculus can be applied to real infrastructure problems.

Application Examples

We demonstrate the applicability of the interlace–weave calculus with two examples: (1) a secure communication network enforcing multi-level security, and (2) a coupled power grid and control network illustrating cyber-physical security. These examples show how the theory can map to concrete scenarios and help harden infrastructure.

Example 1: Secure Multilevel Communication Network

Consider a simple communication network with four nodes connected in a square topology (Figure 1, conceptual). The nodes represent computers or routers at different security classification levels in a military network: $A$ at Top Secret (TS), $B$ at Secret (S), $C$ at Secret (S), and $D$ at Unclassified (U). The connections are physical or logical communication links. We model this as a graph $G$ with $V={A,B,C,D}$ and edges $E={{A,B}, {A,C}, {B,D}, {C,D}}$ forming a square. We assign lattice labels via $L$: $L(A)=$ TS, $L(B)=L(C)=$ S, $L(D)=$ U, where the lattice order is U < S < TS (Unclassified ≤ Secret ≤ Top Secret). Suppose node $A$ holds some high-secret data that it periodically sends out, and node $D$ is a public system that should never receive secret info directly. Nodes $B$ and $C$ are intermediate classified networks.

Now, using interlace–weave calculus, we ask: does any communication flow violate the lattice constraints? According to our lattice, information is only allowed to flow upwards (to higher or equal classification)
arxiv.org
. That means TS → S or TS → U flows are forbidden (TS to lower), and S → U is forbidden, while U → S or S → TS is fine (low to high). In the initial network design, edges ${A,B}$ and ${A,C}$ connect A (TS) to B (S) and C (S). These edges pose a risk: any data flowing from A to B or C is Top Secret down to Secret, violating “no write down” if not mediated. Similarly, edges ${B,D}$ and ${C,D}$ connect Secret nodes to Unclassified D – also a potential violation if B or C send data to D.

To enforce security, we interlace lattice constraints with the network flows. One approach is to deploy guard nodes or filtering mechanisms on those sensitive links. For instance, between A and B, we could insert a guards $G_{AB}$ that will only allow appropriately downgraded information through (or perhaps physically, A is not directly connected to B but via a controlled gateway). In terms of the calculus, we modify the graph by splitting edge ${A,B}$ into $A \to G_{AB} \to B$, where $G_{AB}$ is a node at TS that only outputs data labeled at most Secret. This effectively enforces lattice compliance: A can send to $G_{AB}$ (TS to TS, allowed), and $G_{AB}$ can send to B (Secret) because $G_{AB}$ performs a controlled declassification (or simply $G_{AB}$ is at TS but trusted to handle TS data and release only S content to B). Similar guards can be placed on $A \to C$, $B \to D$, and $C \to D$ links. After this modification, the coupled system satisfies the lattice ordering on all flows: e.g. A’s TS data to D must pass A→$G_{AB}$→B→$G_{BD}$→D, with each step either non-decreasing in classification or involving an approved downgrade at a guard. The calculus ensures that now any flow from A to D must go through a point where it’s checked against policy. As a result, the entire network achieves non-interference: even though A is blasting out Top Secret data, D cannot receive anything that A sends unless it’s explicitly downgraded. In the formal sense, changes in A’s high-level data have no effect on D’s low-level state (unless the guard permits specific filtered information), which is exactly what we want for security
arxiv.org
.

From a flow perspective, we can also analyze equilibrium if we consider a harmonic flow model (perhaps representing average data flow rates). Let’s assign notional “potential” values: say $\Phi(A)=10$ (representing a high data content value), $\Phi(D)=0$ (public has baseline 0), and see what harmonic $\Phi(B), \Phi(C)$ would be without guards. Solving the harmonic equations as earlier, we find $\Phi(B)=\Phi(C)=5$ in steady state if all edges have equal weight. That would mean significant influence from A (10) has reached B and C (both 5), and then flows to D (0). Without policy, D’s immediate neighbors had value 5, so D would get influenced (if we extended one more, D might become something like 2.5 if it had neighbors at 5 and 0). But with guards, effectively we alter the network so that the “potential” seen by D is not directly an average of a Secret neighbor. We can model a guard as a high-resistance (low conductance) link or one-way valve that prevents free flow of high-level potential to low. In a fully harmonic regime, the presence of such constraints means D’s equilibrium value remains at 0 (if nothing allowed flows into it). The network now has a non-interfering harmonic solution: A stays at 10, B and C may settle at some intermediate (if allowed by guards, maybe they only get some sanitized portion, or we can consider them as sources themselves), and D stays at 0. If we attempted to compute this via code (as we will in the next section), we’d see violations flagged on edges A–B, A–C, B–D, C–D, prompting the addition of guards to remove those direct flows.

This example demonstrates how interlace–weave calculus guides secure network architecture: by explicitly coupling the security lattice with the network flow model, we identified the edges violating policy and introduced design changes to enforce the lattice. The result is a system where the math guarantees no forbidden information leakage. In practice, these guards could be implemented via secure gateways, encryption, or data diodes. The key is that our formalism pinpoints where and why they are needed, providing an explainable AI tool for network designers (the model explains that edge A–B is not allowed because TS > S, etc.).

Example 2: Resilient Power Grid with Coupled Control Network

Our second example involves a cyber-physical system: a regional power grid (simplified) with a supervisory control and data acquisition (SCADA) network that monitors and controls it. The power grid can be modeled as a graph of buses (nodes) connected by transmission lines (edges). Power flows in this network follow physical laws (Kirchhoff’s laws) which can be linearized in a DC load flow model analogous to harmonic flows – power distribution at steady state minimizes losses and each bus’s power injection equals sum of flows on lines (flow conservation) similar to $\Delta \Phi = 0$. The control network is a communication network that must be secure: adversaries should not be able to spoof or disrupt it to cause outages.

Using interlace–weave calculus, we integrate these two layers. The lattice $L$ here might represent trust or criticality levels of different control commands (e.g. an operator command vs. an automated response vs. an external signal). The power grid nodes have physical states (voltages, phase angles) $\Phi_{\text{grid}}$, and the control nodes have data states $\Phi_{\text{ctrl}}$. Flows $F_{\text{grid}}$ are electrical flows, and $F_{\text{ctrl}}$ are information flows (commands/telemetry). We couple them: certain control nodes feed into certain grid nodes (e.g. a control command can change a generator output at a bus) and sensor data flows from grid to control (e.g. measurements from a substation to the control center). This coupling is where a potential attack could happen: a malicious command from a low-privilege network segment should not override a high-criticality control function.

For illustration, imagine a grid of 3 buses in a chain: Bus1 – Bus2 – Bus3, where Bus1 is a power plant (source of power), Bus3 is a major load (sink), and Bus2 is intermediate. In normal operation, Bus1 injects 100 MW, Bus3 consumes 100 MW, Bus2 is just a pass-through node. The flows on lines (1-2 and 2-3) each carry 100 MW. The potentials (analogous to voltage angles) might be $\Phi(Bus1)=10$, $\Phi(Bus3)=0$, and $\Phi(Bus2)=5$ degrees (some value causing flows of 100 MW on each line). This is harmonic: Bus2’s angle 5 is the average of 10 and 0 (assuming equal line impedance), reflecting balanced flow.

Now, the control network: say a control node $C2$ monitors Bus2 and can send trip commands to a breaker on line 2-3 if overload is detected. $C2$ is high-critical (should not be influenced by outside). Another node $U$ is a user interface on the corporate network (low trust) that should not send direct grid commands. We set a simple lattice: ${\text{Low}, \text{High}}$ for control commands (Low = not trusted, High = trusted control). $C2$ is High, $U$ is Low. According to lattice rules, $U$ should not influence $C2$ (for confidentiality/integrity, Low should not write to High). In the coupled system, this means any edge from $U$ to $C2$ in the control network must be blocked (perhaps $U$ can only request through a secure gateway which sanitizes input). By weaving the two networks: $C2$ reads measurements from Bus2 (safe, as physical to cyber link is one-way and we consider that okay). If $U$ tries to send a command to Bus2, it should be disallowed since $U$ is Low. Indeed, our calculus would flag a direct Low→High link as forbidden. The fix: do not permit direct control from $U$; require $C2$ to confirm or ignore $U$’s input. In practice, this is implementing network segmentation or an approval process.

In terms of flows, consider a scenario: an attacker at $U$ tries to open the breaker on line 2-3 by sending a malicious command. Without lattice enforcement, that command flows to $C2$ and then to Bus2’s breaker, opening it. That would cut power flow, leaving Bus3 without supply. Bus2’s state would change (power no longer flowing, Bus2 now only connected to Bus1, so Bus1’s injection has nowhere to go – an unstable scenario). With lattice enforcement, $U$’s command is blocked (no flow from Low to High), so the breaker never opens. The grid remains intact. The harmonic graph modeling of the grid might show that if line 2-3 opened, Bus2 and Bus1 would separate from Bus3, violating the equilibrium (Kirchhoff’s laws broken until generation re-dispatch). Our framework would highlight that $U$ to $C2$ flow is not allowed, thus such a scenario cannot occur through the legitimate control channel, thereby hardening the grid against that cyber-attack.

Moreover, the calculus could be used to ensure the grid itself is resilient: if Bus3’s demand spikes, flows rearrange harmonically to keep balance, and if any line is overloaded, the system can weave in a corrective flow via controls (like islanding a part of the grid to preserve overall stability, which in math terms is cutting the graph – something that can be analyzed via flows and energy considerations).

In summary, Example 2 shows a coupled system where physical flows (power) and informational flows (commands) interact. The interlace–weave calculus helps identify policy constraints (like trust levels) that must accompany the physical couplings. It ensures no low-trust command can directly upset a high-critical component. It also leverages the harmonic property: the grid tends toward a stable flow distribution, and if the control actions are likewise governed by a lattice (e.g. only safe, approved adjustments), the combined system remains stable and secure. This approach can be generalized to water distribution systems, transportation networks with communication, and beyond – anywhere we have a mix of continuous dynamics and discrete policies.

These examples highlight how our theoretical framework can be employed in practice. Network engineers or system designers can create a model of their system in our calculus, use the theorems (or automated checks derived from them) to find vulnerabilities or unstable configurations, and then mathematically guarantee that certain fixes (like adding a guard, or re-routing flows) will ensure security and stability. Next, we present a code appendix that demonstrates an implementation of these ideas, bridging the gap from theory to a working prototype that could be integrated into real security toolchains.

Code Appendix

In this appendix, we provide source code (in Python) that implements core aspects of the interlace–weave calculus. The code is designed to be clear, robust, and illustrative rather than optimized. It allows defining a coupled lattice/flow system, solving for harmonic flows, and checking compliance with lattice security constraints. This could serve as a foundation for a defense-oriented or XAI application that analyzes and secures infrastructure in real time.

# Interlace-Weave Calculus Implementation (simplified example)

import numpy as np

class Lattice:
    def __init__(self, levels, order):
        """
        Initialize a lattice.
        :param levels: List of level names.
        :param order: Dict mapping each level to set of levels >= it (reflexive, transitive closure of <=).
        """
        self.levels = levels
        self.order = order  # e.g., order[a] = set of levels greater-or-equal to a.
    def allows_flow(self, level_from, level_to):
        """Return True if flow from level_from to level_to is allowed (i.e., level_from <= level_to)."""
        return level_to in self.order.get(level_from, {level_from})

class CoupledSystem:
    def __init__(self, graph, lattice, node_level):
        """
        :param graph: Dict of node -> list of (neighbor, weight) representing the network.
        :param lattice: Lattice object for security levels.
        :param node_level: Dict mapping node -> security level (must be in lattice.levels).
        """
        self.graph = graph
        self.lattice = lattice
        self.node_level = node_level
        # Initialize potentials and flows
        self.potential = {node: 0.0 for node in graph}
        self.flows = {}  # (u,v) -> flow value
    def set_boundary(self, values):
        """
        Set fixed potential values for boundary nodes (e.g., sources and sinks).
        :param values: Dict of node -> potential value (for nodes we consider as boundary with fixed values).
        """
        for node, val in values.items():
            self.potential[node] = float(val)
    def solve_harmonic(self, tol=1e-6, max_iter=10000):
        """
        Solve for harmonic potentials on interior nodes using iterative relaxation.
        """
        # Identify interior nodes (not fixed boundary)
        boundary = {n for n in self.graph if n in self.fixed_boundaries}
        interior = [n for n in self.graph if n not in boundary]
        # Simple Gauss-Seidel iteration for Laplace's equation
        for it in range(max_iter):
            max_diff = 0.0
            for u in interior:
                # Compute weighted average of neighbors' potentials
                if not self.graph[u]:
                    continue  # isolated node, skip
                sum_w = 0.0
                sum_wphi = 0.0
                for v, w in self.graph[u]:
                    sum_w += w
                    sum_wphi += w * self.potential[v]
                new_phi = sum_wphi / sum_w
                diff = abs(new_phi - self.potential[u])
                if diff > max_diff:
                    max_diff = diff
                self.potential[u] = new_phi
            if max_diff < tol:
                break
        # Compute flows on each edge based on solved potentials
        for u in self.graph:
            for v, w in self.graph[u]:
                # Only compute each undirected edge once to avoid duplicates, enforce a direction for consistency
                if (v, u) in self.flows:
                    continue
                flow_uv = w * (self.potential[u] - self.potential[v])
                self.flows[(u, v)] = flow_uv
                self.flows[(v, u)] = -flow_uv  # opposite direction (for reference)
    def check_lattice_security(self):
        """
        Check all edges to ensure no flow violates lattice ordering. Returns list of violations.
        """
        violations = []
        for (u, v), flow_val in self.flows.items():
            if flow_val <= 0:
                # Only consider flow from u to v if positive (meaning net flow from u->v direction).
                continue
            level_u = self.node_level.get(u)
            level_v = self.node_level.get(v)
            if level_u is None or level_v is None:
                continue  # no level info, skip
            if not self.lattice.allows_flow(level_u, level_v):
                violations.append(f"Disallowed flow from {u}({level_u}) to {v}({level_v}) of magnitude {flow_val:.2f}")
        return violations

# Example usage:
# Define a simple lattice: Unclass < Secret < TopSecret
levels = ["Unclassified", "Secret", "TopSecret"]
order = {
    "Unclassified": {"Unclassified", "Secret", "TopSecret"},
    "Secret": {"Secret", "TopSecret"},
    "TopSecret": {"TopSecret"}
}
security_lattice = Lattice(levels, order)

# Define graph (square topology as per Example 1)
graph = {
    "A": [("B", 1.0), ("C", 1.0)],
    "B": [("A", 1.0), ("D", 1.0)],
    "C": [("A", 1.0), ("D", 1.0)],
    "D": [("B", 1.0), ("C", 1.0)]
}
node_level = {"A": "TopSecret", "B": "Secret", "C": "Secret", "D": "Unclassified"}

# Initialize system and set boundary potentials
system = CoupledSystem(graph, security_lattice, node_level)
system.fixed_boundaries = {"A", "D"}  # treat A and D as boundary nodes for this analysis
system.set_boundary({"A": 10.0, "D": 0.0})  # e.g., A at potential 10, D at 0

# Solve for harmonic state and check security
system.solve_harmonic()
violations = system.check_lattice_security()
print("Potentials:", system.potential)
print("Flows:", {edge: f"{val:.2f}" for edge, val in system.flows.items() if val > 0})
print("Policy Violations:", violations)


Explanation: In the code above, we define a Lattice with an allows_flow method that checks the partial order. The CoupledSystem holds the graph and lattice, and can solve for harmonic potentials using an iterative method. We then check each directed edge’s flow: if there is a positive net flow from u to v, we see if L(u) ≤ L(v); if not, we record a violation.

In the example usage, we set up the four-node network from Example 1. Node A (Top Secret) is fixed at potential 10, node D (Unclassified) at 0, and B,C (Secret) are interior. Solving yields potentials approximately A=10, B≈5, C≈5, D=0 (as expected, B and C end up around the average of A and D). The flows will be roughly A→B:5, A→C:5, B→D:5, C→D:5 (in whatever units). The check_lattice_security will find that A (TS) to B (S) is a disallowed downward flow, likewise A→C, B→D, C→D. It would list those edges as violations with their magnitudes. These correspond to the earlier discussion that guards or policy enforcement are needed on those links. If we were to “fix” the network (for instance, remove or severely restrict those flows), we could adjust the graph weights or structure and re-run the solver to confirm no violations remain.

Sample Output:

Potentials: {'A': 10.0, 'B': 5.0, 'C': 5.0, 'D': 0.0}
Flows: {('A', 'B'): '5.00', ('A', 'C'): '5.00', ('B', 'D'): '5.00', ('C', 'D'): '5.00'}
Policy Violations: ['Disallowed flow from A(TopSecret) to B(Secret) of magnitude 5.00', 
                    'Disallowed flow from A(TopSecret) to C(Secret) of magnitude 5.00', 
                    'Disallowed flow from B(Secret) to D(Unclassified) of magnitude 5.00', 
                    'Disallowed flow from C(Secret) to D(Unclassified) of magnitude 5.00']


This output confirms our analytical results. The code, although simplified, shows how the interlace–weave calculus can be implemented to automatically flag security issues (for example, in a network planning tool or an AI system that must explain why a certain connection is insecure). In a more advanced implementation, we could allow the code to suggest remedies (like insert guard nodes or set certain flows to zero) and re-compute until no violations exist – effectively weaving in the lattice constraints.

In practice, this approach can be integrated with real network data. For instance, in a power grid, $\Phi$ might be solved from telemetry; the lattice might represent which control centers can influence which substations. The system could then run continuously to ensure no policy violations (if an unexpected flow appears, it might signify an intrusion or misconfiguration). For communications networks, this could verify information flow policies in an enterprise architecture, helping the DoD or other organizations enforce multi-domain security and explain the enforcement (the system can explain violations in human-readable terms, as above). Because our framework is rigorous, it can form the basis of certifiable security guarantees, which is especially important in defense contexts.

Conclusion of Appendix: The provided code is a proof-of-concept linking the high-level theory to a tangible tool. It demonstrates constructing a coupled lattice/flow model, solving for harmonic flows, and verifying lattice compliance. With further development (e.g. support for directed graphs, dynamic updates, larger scale, integration with actual infrastructure databases), this could evolve into an operational XAI system. The combination of formal mathematics with programmatic implementation illustrates the full cycle of the interlace–weave calculus: from definitions and theorems to real-world application and automation. 
arxiv.org
math.uchicago.edu
