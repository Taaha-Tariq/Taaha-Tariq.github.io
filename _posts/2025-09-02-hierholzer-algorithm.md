---
title: "Hierholzer's Algorithm: Finding Eulerian Circuits"
date: 2025-09-02
categories: [Algorithms, Graph Theory]
tags: [hierholzer's algorithm, eulerian circuit, eulerian path, graph, trail]
excerpt: "A detailed explanation of Hierholzer's Algorithm for finding Eulerian circuits and paths in graphs, with definitions and examples."
math: true
---

Imagine standing on an archipelago, its islands connected by bridges, and facing a seemingly simple question: can you start at one point, cross each bridge exactly once, and return to where you began?
This puzzle, first posed in the city of [**Königsberg**](https://en.wikipedia.org/wiki/Seven_Bridges_of_K%C3%B6nigsberg), puzzled mathematicians for years until Euler provided a groundbreaking solution. His work gave birth to what we now call *Euler paths* and *Euler circuits* — fundamental concepts in graph theory.

To make the discussion clearer, let’s first go over some important definitions.
# Incidence
Considering the same problem, imagine standing on one of the bridges and asking yourself where you can go from there. This idea of a bridge being connected to a landmass is exactly what incidence means.
<div class="definition">
<b>Definition:</b>  
An edge is said to be incident to a vertex if that vertex is one of the edge's endpoints.
</div>
***
And to get an idea of the number of ways to get to a landmass through bridges is what we commonly refer to as the degree of a vertex or landmass in this context.

<div class="definition">
<b>Definition:</b>  
<i>Degree</i> of a vertex refers to the number of edges incident to that vertex, denoted as $deg(v)$.
</div>
***
A loop is counted twice in the degree of a vertex since both its edges are incident to the same vertex.

In the context of a directed graph, we differentiate between *out-degrees* and *in-degrees* since the edges offer a unidirectional connection. **In-degree** of a vertex counts the number of ways to get into that vertex, whereas **Out-degree** counts the number of ways to get out of that vertex. An important thing to note is that if we were to sum up the in-degrees and out-degrees of all the vertices, then they would be equal. To see why, consider the directed edge as a one-way road; such a road must have a starting point and an endpoint (the vertices), and the same is true for all directed edges.

<div style="text-align:center;">
$$\sum_{v \in \mathbf{V}} deg^+(v) =\sum_{v \in \mathbf{V}} deg^-(v)
\text{ where } \mathbf{V} \text{ is the vertex set.}
$$
</div>

These definitions motivate a theorem, commonly referred to as the first theorem of graph theory or the [**Handshake theorem**](https://en.wikipedia.org/wiki/Handshaking_lemma). 
<div class="theorem">
    <b>Theorem:</b>  
    In an undirected graph, the sum of the degrees of all its vertices is equal to twice the number of edges in the graph.

    $$\sum_{v \in \mathbf{V}} deg(v) = 2 \cdot |\mathbf{E}|$$
</div>
***
It follows from the fact that each edge has two endpoints and so, contributes two to the degrees of the graph. This theorem provides insight into the graph structure and allows us to validate graph configuration, and is also useful in graph coloring.

# Eulerian Path/Circuit
To get an intuition of what a path means in graph theory, imagine starting from an island and going around the archipelago, following the bridges, and ending at some island, not necessarily the one you started from (if so, then it is called a circuit), but the bridges and islands can not be revisited. This traversal of islands constitutes a path.

<div class="definition">
    <b>Definition:</b>  
    A <b>path</b> is a sequence of distinct vertices joined by distinct edges, representing a traversal of the graph.
</div>
***
A circuit is a path whose starting and ending vertices is the same.

![Illustration of a path](/assets/images/figures-11.png)

 The path depicted in *figure 1* can be thought of as the sequence of vertices $A, B, C, E$ where there is an undirected edge between $(a_{i - 1}, \; a_i)$ but the directed edges indicate the path traversal.

Now that we are done with the preliminary definitions and concepts, we turn to the crux of the article.
 <div class="definition">
 <b>Definition:</b>
     An <b>Euler path</b> is a path that visits each edge exactly once.
 </div>
 ***
 An Euler circuit is defined in the same way, with the additional constraint that it must start and end at the same vertex.

## Existence
### Criteria 1
For an Euler path to exist, the graph must be connected. If it has an isolated subgraph, then such a subgraph must not have any edges. This is because if isolated subgraphs have distinct edges, then any path through the graph can't traverse all of these isolated subgraphs.

![Illustration of criteria 1](/assets/images/figures-12.png)

If we start from any $b_i$ we can only traverse the $b_j's \text{ where} \; j= 1, 2, 3, 4$ vertices and not the $(a_i, a_j)$ edges. Similarly for $a_i's$.

### Criteria 2
For a directed graph, the difference between $deg^+(v)$ and $deg^-(v)$ must be less than two, that is $|deg^+(v) - deg^-(v)| < 2$, for if it is two or greater, not all the edges can be traversed.

To see why, consider the case where $deg^+(v) = 2$ and $deg^-(v) = 4$.

![Illustration of criteria 2](/assets/images/figures-13.png)

There are two ways into the vertex and four out of it, so even if you were to start from this vertex, you can at most cover three of the four outgoing vertices.

### Criteria 3
For all the vertices excluding the starting and the ending vertex, the $deg^+(v)$ must be equal to $deg^-(v)$; this ensures that for every way into the vertex, there's a way out. If this is true for the starting and ending vertices as well, then we have an Euler circuit. Otherwise, we can have $deg^-(v) - deg^+(v) = 1$ for the starting vertex, which ensures that for every edge entering the vertex, there is a corresponding way out, with one additional outgoing edge reserved for the initial move. Similarly, for the ending vertex, we can have $deg^+(v) - deg^-(v) = 1$.

![Illustration of Euler path](/assets/images/figures-14.png)
  
Consider the sequence of vertices $s,\; a, \;b, \;c, \;a, \; d, \;t$.

For an undirected graph, this can be succinctly expressed as:
"There's an Euler path if the degree of exactly two of the vertices is odd, and an Euler circuit if the degree of none of the vertices is odd".

# Hierholzer's Algorithm
The basic idea of Hierholzer's algorithm is the stepwise construction of the Eulerian path by connecting disjoint cycles.

First, we ensure that an Euler path or circuit exists by the above-stated criteria. If either one fails, then we can't have an Euler path or circuit.

### Algorithm
- Begin at a starting vertex (the designated start if one exists, otherwise any vertex with edges). Follow edges successively, choosing each edge only once, until you can no longer continue. This produces an initial trail that either forms a closed cycle (Euler circuit case) or ends at the terminal vertex (Euler path case).
- If there remain unused edges, find a vertex on the current trail that still has unvisited outgoing edges. From this vertex, construct another trail in the same way, and then splice it into the original trail. Repeating this process until all edges are used yields the Euler path or circuit.


![Illustration of Hierholzer's algorithm](/assets/images/figures-15.png)

Link to Hierholzer's algorithm's implementation in different languages: [Implementation of the algorithm](https://rosettacode.org/wiki/Hierholzer%27s_algorithm).

# Conclusion
Euler paths and circuits arise in many applications throughout computer science and other disciplines, where they can be used to find optimal routes minimizing travel distance and ensuring complete coverage of an area, amongst others, and it is of utmost importance to find an efficient solution to this problem, something that this algorithm offers to do in $O(E)$ time complexity.