# $r$-neighbour bootstrap percolation in $k$ steps #

See https://arxiv.org/abs/2209.07594 for a description of $r$-neighbour bootstrap percolation.


**Input:** A graph $G$, an integer $r\geq1$, and an integer $0 \leq k \leq |V(G)|$. 

**Output:** The minimum of $|A_0|$ such that $A_0\subseteq V(G)$ and $A_0$ percolates under $r$-neighbour bootstrap percolation in $G$  in at most $k$ steps. Note, initial state corresponds with step 0 and if $k = |V(G)|$ this solves the problem in general.


For each $v \in V(G)$ and $0 \leq i \leq k$ let $v_i = 1$ if $v$ is infected at step $i$ and $0$ otherwise. Note that we require that $v_i \geq v_{i-1}$, i.e., once infected, a vertex cannot be uninfected. In this sense, for each $v$, $\ell = \min_{0 \leq i \leq k}\{v_i = 1\}$ gives the step $v$ is infected.

**Minimize:** $\sum_{v\in V(G)}v_0.$

**Subject to:**

1. $v_i \leq v_{i-1} + \frac{1}{r}\left[\sum_{u \in N(v)} u_{i-1} \right], \quad\forall v\in V(G) \textnormal{ and } 1 \leq i \leq k,$
2. $v_i \geq \frac{\left(\sum_{u \in N(v)} \frac{u_{i-1}}{r}\right) + v_{i-1} - \frac{r-1}{r}}{d(v) + 1}, \quad\forall v\in V(G) \textnormal{ and } 1 \leq i \leq k,$
3. $\sum_{v\in V(G)} v_k = |V(G)|,$  
4. $v_{i} \in \{0,1\} \quad \forall v \in V(G) \textnormal{ and } 0 \leq i \leq k.$


**Constraint (1):** This step encodes that fact that $v$ cannot be infected at step $i$ unless it is already infected or has $r$ neighbors who are infected. At step $i$, there are three possible states:

A) $v$ has already been infected $(v_{i-1} = 1)$ and some subset (possibly empty) of $v$'s neighbors are infected. Therefore, 
    $$1 \leq v_{i-1} + \frac{1}{r}\left[\sum_{u \in N(v)} u_{i-1} \right] \Rightarrow v_i \leq 1.$$
B) $v$ has not been infected $(v_{i-1} = 0)$ and strictly less than $r$ of $v$'s neighbors are infected. Therefore, 
    $$v_i \leq v_{i-1} + \frac{1}{r}\left[\sum_{u \in N(v)} u_{i-1} \right] \leq \frac{r-1}{r} < 1 \Rightarrow v_i = 0.$$
C) $v$ has not been infected $(v_{i-1} = 0)$ and at least $r$ of $v$'s neighbors are infected. Therefore, 
    $$1 = \frac{r}{r} \leq v_{i-1} + \frac{1}{r}\left[\sum_{u \in N(v)} u_{i-1} \right] \Rightarrow v_i \leq 1$$


**Constraint (2):** This step encodes that fact that $v$ must be infected at step $i$ if it is already infected or has $r$ neighbors who are infected. At step $i$, there are three possible states:

A) $v$ has already been infected $(v_{i-1} = 1)$ and some subset (possibly empty) of $v$'s neighbors are infected. Therefore, 
    $$v_i\geq \frac{\left(\sum_{u \in N(v)} \frac{u_{i-1}}{r}\right) + v_{i-1} - \frac{r-1}{r}}{d(v) + 1} \geq \frac{0 + 1 - \frac{r-1}{r}}{d(v) + 1} > 0 \Rightarrow v_i = 1.$$
    Note that, as $v$ has at most $d(v)$ neighbors and $r \geq 1$,
    $$1 = \frac{d(v) + 1}{d(v) + 1} \geq \frac{\frac{d(v)}{r} + 1 - \frac{r-1}{r}}{d(v) + 1} \geq \frac{\left(\sum_{u \in N(v)} \frac{u_{i-1}}{r}\right) + v_{i-1} - \frac{r-1}{r}}{d(v) + 1}$$
    so this bound never forces $v_i > 1$.
B) $v$ has not been infected $(v_{i-1} = 0)$ and strictly less than $r$ of $v$'s neighbors are infected. Therefore, 
    $$0 =\frac{\frac{r-1}{r} + 0 - \frac{r-1}{r}}{d(v)+1} \geq \frac{\left(\sum_{u \in N(v)} \frac{u_{i-1}}{r}\right) + v_{i-1} - \frac{r-1}{r}}{d(v) + 1} \Rightarrow v_i \geq 0.$$
C) $v$ has not been infected $(v_{i-1} = 0)$ and at least $r$ of $v$'s neighbors are infected. Therefore,
    $$v_i\geq \frac{\left(\sum_{u \in N(v)} \frac{u_{i-1}}{r}\right) + v_{i-1} - \frac{r-1}{r}}{d(v) + 1} \geq \frac{\frac{r}{r} +0 - \frac{r-1}{r}}{d(v) + 1} > 0 \Rightarrow v_i = 1.$$
    Note that, as $v$ has at most $d(v)$ neighbors and $r \geq 1$,
    $$1 = \frac{d(v) + 1}{d(v) + 1} \geq \frac{\frac{d(v)}{r} + 0 - \frac{r-1}{r}}{d(v) + 1} \geq \frac{\left(\sum_{u \in N(v)} \frac{u_{i-1}}{r}\right) + v_{i-1} - \frac{r-1}{r}}{d(v) + 1}$$
    so this bound never forces $v_i > 1$.

**Constraint (3):** All vertices must be infected by step $k$.

This ILP has $|V(G)|k \leq |V(G)|^2$ variables and $2|V(G)|(k-1) + 1 \leq 2|V(G)|^2$ constraints.

# Run code #

See the .cpp file for an implementation of this ILP. Lines 11, 13, and 16 define the number of vertices in the graph, $r$ value, and the $k$ value of the instance of the problem. Line 138 defines the adjacency matrix of the graph. Change values appropriately for you instance of the problem.

This method requires a local copy of a gurobi solver (https://www.gurobi.com/). The file gurobi.env contains some gurobi settings. To compile with gurobi, run the following command (assuming you have a mac and used the default directory for gurobi)

g++ -m64 -g -O3 r_neighbour_k_per.cpp -I/Library/gurobi951/macos_universal2/include -L/Library/gurobi951/macos_universal2/lib -lgurobi_c++ -lgurobi95 -lm
