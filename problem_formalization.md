---
author:
- |
  Ryan Batubara\
  `rbatubara@ucsd.edu`\
  Ivy Hawks\
  `mahawks@ucsd.edu`\
  Darren Ho\
  `dah103@ucsd.edu`\
  Ciro Zhang\
  `ciz001@ucsd.edu`\
  Shachar Lovett\
  `slovett@ucsd.edu`\
title: "Problem Definition: Week 2"
---

# Problem {#problem .unnumbered}

Consider a graph $G$ with edges $E$ and vertices $V$, and subgraphs
$G_1, \ldots, G_k \subset G$. Let $G_i = (E_i, V_i)$ for these
subgraphs. For now let us suppose the graph is unweighted and
undirected. The subgraphs need not cover $G$ and can overlap. Each
player $i$ knows $G$ and $G_i$, but not the other player's graphs. (This
is akin to a map of a city, where each player is a different regional
taxi company.) The goal is, given some starting vertex $v \in V$, and
some end vertex, $w \in V$ to:

1.  Identify if it is possible to construct a path
    $v, v_1, \dots, v_l, w$ such that every edge on the path is in
    $\bigcup_{i=0}^{k} E_i$. []{#one label="one"}

2.  If such a path exists, find the minimum number of edges needed to
    get from $v$ to $w$, while using only edges belonging to
    $\bigcup_{i=0}^{k} E_i$. []{#two label="two"}

<figure>

<figcaption>A valid path from <span
class="math inline"><em>v</em></span> to <span
class="math inline"><em>w</em></span> in bold. Note that <span
class="math inline"><em>G</em></span> may contain nodes not belonging to
any player.</figcaption>
</figure>

# Lower bound sketches and ideas {#lower-bound-sketches-and-ideas .unnumbered}

For [\[one\]](#one){reference-type="ref+label" reference="one"}, we
think this would require computing $\binom{k}{2}$ pairwise set
disjointnes in the worst case, to determine where the intersections of
each $G_i$ are, at which point each player could on their own determine
the possible paths between each of the subgraphs that intersect with
their own.

<figure>

<figcaption>Worst case for <a href="#one"
data-reference-type="ref+label" data-reference="one">[one]</a>: Disjoint
Subsets</figcaption>
</figure>

For [\[two\]](#two){reference-type="ref+label" reference="two"}, we
would be required to compute $\binom{k}{2}$ pairwise intersections,
since in order to minimize the path we would need to know all possible
paths that could connect $v$ and $w$. To find the shortest path, each
player works backwards and sends the distances through their internal
vertices to the players they intersect with, and each player will
attempt to identify if a longer path can be dropped whenever they
receive this list.

# Lower bound using Equality {#lower-bound-using-equality .unnumbered}

#### Equality problem.

In the two-party Equality problem, Alice is given a string
$x \in \{0,1\}^n$ and Bob is given a string $y \in \{0,1\}^n$. The goal
is to determine whether $x = y$. It is known that Equality has
deterministic communication complexity $\Omega(n)$.

#### Equality gadget.

We construct a gadget that enforces equality at a single bit position.
The gadget is shown in
Figure [1](#fig:equality_gadget){reference-type="ref"
reference="fig:equality_gadget"}. The gadget consists of two parallel
branches.

1.  The top branch corresponds to the assignment $x_i = y_i = 1$,

2.  The bottom branch corresponds to the assignment $x_i = y_i = 0$.

<figure id="fig:equality_gadget">

<figcaption>Gadget for proving equality. A path only exists if <span
class="math inline"><em>x</em><sub><em>i</em></sub> = <em>y</em><sub><em>i</em></sub></span>
for either branch.</figcaption>
</figure>

Alice enables exactly one entry node of the gadget depending on $x_i$,
and Bob enables exactly one exit node depending on $y_i$. A path exists
through the gadget if and only if Alice and Bob select nodes on the same
branch, which holds exactly when $x_i = y_i$.\

#### Reduction to our graph problem.

We construct a graph by chaining one copy of this gadget for each index
$i \in [n]$ between a global start vertex $v$ and end vertex $w$. In the
resulting graph, there exists a path from $v$ to $w$ if and only if all
gadgets are traversable, which occurs if and only if $x_i = y_i$ for all
$i$, i.e., if and only if $x = y$.

<figure>

<figcaption>Equality gadgets chained as adjacent hexagonal
modules.</figcaption>
</figure>

Therefore, any protocol that solves the reachability problem on this
graph can be used to solve the Equality problem. Since Equality requires
$\Omega(n)$ deterministic communication, it follows that our graph
problem also requires $\Omega(n)$ deterministic communication.

# Lower bound using Disjointness {#lower-bound-using-disjointness .unnumbered}

#### Set Disjointness problem.

Alice is given a vector $x \in \{0,1\}^n$ and Bob is given a vector
$y \in \{0,1\}^n$. The goal is to determine whether the sets represented
by $x$ and $y$ are disjoint. It is known that Disjointness has
deterministic communication complexity $\Omega(n)$.

#### Disjointness gadget.

We construct a gadget that enforces disjointness at a single bit
position. The gadget is shown in Figure 5. The gadget is constructed so
that traversal is possible for all assignments except when both parties
contain the element.

1.  The top branch corresponds to when $x_i$ has the element but $y_i$
    does not

2.  The middle branch corresponds to when $x_i$ and $y_i$ do not have
    the element

3.  The top branch corresponds to when $y_i$ has the element but $x_i$
    does not

<figure id="fig:disjoint_gadget">

<figcaption>Gadget for proving equality, a path only exists if <span
class="math inline"><em>x</em><sub><em>i</em></sub></span> and <span
class="math inline"><em>y</em><sub><em>i</em></sub></span> do not have
the same element</figcaption>
</figure>

#### Reduction to our graph problem.

We construct a graph by chaining one copy of this gadget for each index
$i \in [n]$ between a global start vertex $v$ and end vertex $w$. In the
resulting graph, there exists a path from $v$ to $w$ if and only if all
gadgets are traversable, which occurs if and only if $x$ and $y$ does
not contain the same element\

<figure>

<figcaption>Equality gadgets chained as adjacent hexagonal
modules.</figcaption>
</figure>

Therefore, any protocol that solves the reachability problem on this
graph can be used to solve the Disjoint problem. Since Disjoint requires
$\Omega(n)$ deterministic communication, it follows that our graph
problem also requires $\Omega(n)$ deterministic communication.
