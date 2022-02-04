# Scalable Distributed Topologies
## Graphs
A graph G(V, E) can be defined by a set of vertices, and a set of edges that connect pairs of vertices. 
- Graphs can be **directed** or **undirected** (bi-directional edges)
- **Simple Graph**: undirected graph with no loops and no more than one edge between any two different vertices
- **Adjacent Vertices**: have an edge connecting them (neighbours)
- **Weighted Graph**: each edge has a weight
- **Path**: sequence of vertices with edges connecting them
	- **Walk**: edges and vertices can be repeated
	- **Trail**: only vertices can be repeated
	- **Path**: no repeated vertices or edges
- **Complete Graph**: each pair of vertices has an edge connecting them
- **Connected Graph**: there is a path between any 2 nodes
- **Star**: a "central" vertice and many leaf nodes connected to center
- **Tree**: a connected graph with no cycles
- **Planar Graph**: vertices and edges can be drawn in a plane and no 2 edges intersect (Rings and Trees)
- **Connected Component**: maximal connected subgraph of G
- **Degree of vi**: number of adjacent vertices to vi
- **Distance d(vi, vj)**: length of the shortest path connecting those nodes
- **Eccentricity of vi**: ecc(vi) = max({d(vi, vj)|vj ∈ V})
- **Diameter**: D = max({ecc(vi)|vi ∈ V})
- **Radius**: R = min({ecc(vi)|vi ∈ V})
- **Center**: {vi |ecc(vi) == R} 
- **Periphery**: {vi |ecc(vi) == D}
- **Random Geometric**: Vertices are dropped randomly uniformly into a unit square and adding edges to connect any two points within a given euclidean distance.
- **Random Erdos-Renyi**: G(n, p) model, n nodes are connected randomly. Each edge is included with independent probability p.
- **Watts-Strogatz model**
- **Barabasi-Albert model**: Preferential attachment -> the more connected a node is, the more likely it is to receive new links. Degree Distribution follows a power law.

### Spanning Trees
*Synchronous SyncBFS Algorithm*

- **Strongly Connected**: for every pair of vertices u and v there is a path from u to v and a path from v to u
- **Distance from u to v**: length of the shortest path from u to v
- **Breadth First (root node i)**: each node at distance d from i in the graph appears at depth d in the tree 
- Every strongly connected graph has a breadth-first directed spanning tree.

Processes communicate over directed edges. Unique UIDs are available, but network diameter and size is unknown

#### Initial state in SyncBFS
- parent = nil
- marked = False (True in root node i0)

#### SyncBFS Algorithm
- Proc