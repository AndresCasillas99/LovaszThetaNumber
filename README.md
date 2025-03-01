# LovaszThetaNumber
Heuristic to approximately compute the Lovasz Theta Number of a given graph G=(V,E)

# 01.03.2025
## Theory

The Lovasz Theta number of an undirected graph G, which we will denote as $L(G)$ for simplicity, can be found by solving the following eigevalue problem:

$L(G) = min_{A \in S^{n}}\{\lambda_{max}(A): A_{i,j} = 1 \iff i=j \hspace{0.1 cm} $or$ \hspace{0.1 cm} (i,j) \not \in E\}$

where S^{n} denotes the set of $n$x$n$ real symmetric matrices.

An interesting property of $L(G)$ is given by Lovasz's Sandwich Theorem, namely:


$\omega(G) \leq L(\bar{G}) \leq \chi(G)$ where $\bar{G}$ is the complementary graph of $G$, $\omega(G)$ is the clique number and $\chi(G)$ is the chromatic number.

Thus, for perfect graphs, it is enough to find $L(\bar{G})$ with an accuracy of $\epsilon < \frac{1}{2}$ in order to know $\omega(G) = \alpha(\bar{G})$ and $\chi(G)$. This allows for efficient polynomial time algorithms for finding maximum independent sets and optimal colorings.  

It also satisfies 

$L(G \square H) = L(G) L(H)$ where $G \square H$ is the strong product of G and H.

Moroever, it upper bounds the Shannon Capacity $\Theta(G) \leq L(G)$.

## How the code works

### Encoding the graph
We will represent undirected graphs as vectors of vectors. 
graph_list will be a vector of length $n$ (number of vertices) and will have in entry $i$ the neighbors of vertex $i$ in an array.

Example:
graph_list = [[2,3],[1,3],[1,2]] means vertex $1$ is connected to $2,3$. Vertex $2$ is connected to $1,3$. Vertex $3$ is ocnnected to $1,2$. In this case graph_list encodes $C_{3}$.

##### function create_matrix(graph_list)
Creates the matrix $A_{i,j} = 1 \iff i=j \hspace{0.1 cm} $or$ \hspace{0.1 cm} (i,j) \not \in E$

##### function max_eigen(A)
Computes the maximum eigenvalue fo A via the power method

##### function randomize!(A)
Gives random uniform values in the interval $[-1,1]$ to the entries $A_{i,j} \hspace{0.1 cm} $s.t.$ \hspace{0.1 cm} (i,j) \not \in E$ and returns the minimum eigenvalue over all of them.

##### function Lovasz(g)
Calls the above functions in order and returns the minimum eigenvalue computed over all random matrices explored. Outputs the value of the eigenvalue and the matrix that achieves the minimum. 

#### function complement(g)
Returns the complementary graph list of a given graph list g.

### One problem

Inside the function $function randomize!(A)$, it is not clear how to select the interval where the random values should live in. I have not found many useful papers that might give some insight into the matter, so I have decided to somply try experimentally by progressively augmenting the interval length and then returning the lowest value among all chosen intervals. My intuiton tells me that the possitivive values of should not be much larger that 1, while the negative values can be somewhat larger in absoulte value (I suspect of the order of n/2).

The function $function randomize_flexible(A,l)$ allows to change the lower bound of the interval where the random variable lives.

# Enjoy!!
