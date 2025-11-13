```

# Лістинг 5.1 – Код реалізації програми алгоритму Пріма.

#include <iostream>
#include <cstring>
using namespace std;

#define INF 9999999
// number of vertices in graph
#define V 8

// adjacency matrix for graph (variant 19)
int G[V][V] = {
    {INF, 6,   2,   4,   INF, INF, INF, INF}, // 1
    {6,   INF, INF, 6,   INF, 6,   INF, INF}, // 2
    {2,   INF, INF, INF, INF, INF, INF, 4  }, // 3
    {4,   6,   INF, INF, 1,   INF, INF, INF}, // 4
    {INF, INF, INF, 1,   INF, 4,   7,   INF}, // 5
    {INF, 6,   INF, INF, 4,   INF, 5,   3  }, // 6
    {INF, INF, INF, INF, 7,   5,   INF, 5  }, // 7
    {INF, INF, 4,   INF, INF, 3,   5,   INF}  // 8
};

int main() {
    int no_edge;           // number of edge

    // array to track selected vertex:
    // selected[i] = true, якщо вершина вже у МКД
    int selected[V];

    // set selected to false initially
    memset(selected, false, sizeof(selected));

    // set number of edges to 0
    no_edge = 0;

    // choose 0th vertex and make it true (це вершина 1 у твоєму графі)
    selected[0] = true;

    int x; // row number
    int y; // col number

    // print header
    cout << "Edge" << " : " << "Weight" << endl;

    while (no_edge < V - 1) {
        // For every vertex in the set S, find all adjacent vertices,
        // calculate the distance from the vertex selected.
        // If the vertex is already in the set S, discard it,
        // otherwise choose the nearest vertex.
        int min = INF;
        x = 0;
        y = 0;

        for (int i = 0; i < V; i++) {
            if (selected[i]) {
                for (int j = 0; j < V; j++) {
                    if (!selected[j] && G[i][j]) { // not in selected and there is an edge
                        if (min > G[i][j]) {
                            min = G[i][j];
                            x = i;
                            y = j;
                        }
                    }
                }
            }
        }

        cout << x << " - " << y << " : " << G[x][y] << endl;

        selected[y] = true;
        no_edge++;
    }

    return 0;
}


# Лістинг 5.2 – Код реалізації програми алгоритму Крускала.

#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

#define edge pair<int,int>

class Graph {
private:
    vector<pair<int, edge>> G; // graph
    vector<pair<int, edge>> T; // mst
    int *parent;
    int V; // number of vertices in graph
public:
    Graph(int V);
    void AddWeightedEdge(int u, int v, int w);
    int find_set(int i);
    void union_set(int u, int v);
    void kruskal();
    void print();
};

Graph::Graph(int V) {
    this->V = V;
    parent = new int[V];
    for (int i = 0; i < V; i++)
        parent[i] = i;
    G.clear();
    T.clear();
}

void Graph::AddWeightedEdge(int u, int v, int w) {
    G.push_back(make_pair(w, edge(u, v)));
}

int Graph::find_set(int i) {
    if (i == parent[i])
        return i;
    else
        return find_set(parent[i]);
}

void Graph::union_set(int u, int v) {
    parent[u] = parent[v];
}

void Graph::kruskal() {
    int i, uRep, vRep;
    sort(G.begin(), G.end()); // increasing weight
    for (i = 0; i < (int)G.size(); i++) {
        uRep = find_set(G[i].second.first);
        vRep = find_set(G[i].second.second);

        if (uRep != vRep) {
            T.push_back(G[i]); // add to tree
            union_set(uRep, vRep);
        }
    }
}

void Graph::print() {
    cout << "Edge : Weight" << endl;
    for (int i = 0; i < (int)T.size(); i++) {
        cout << T[i].second.first << " - "
             << T[i].second.second << " : "
             << T[i].first << endl;
    }
}

int main() {
    Graph g(8);

    // 1–3 (2)
    g.AddWeightedEdge(0, 2, 2);
    g.AddWeightedEdge(2, 0, 2);

    // 3–8 (4)
    g.AddWeightedEdge(2, 7, 4);
    g.AddWeightedEdge(7, 2, 4);

    // 8–6 (3)
    g.AddWeightedEdge(7, 5, 3);
    g.AddWeightedEdge(5, 7, 3);

    // 6–7 (5)
    g.AddWeightedEdge(5, 6, 5);
    g.AddWeightedEdge(6, 5, 5);

    // 7–8 (5)
    g.AddWeightedEdge(6, 7, 5);
    g.AddWeightedEdge(7, 6, 5);

    // 6–5 (4)
    g.AddWeightedEdge(5, 4, 4);
    g.AddWeightedEdge(4, 5, 4);

    // 5–7 (7)
    g.AddWeightedEdge(4, 6, 7);
    g.AddWeightedEdge(6, 4, 7);

    // 5–4 (1)
    g.AddWeightedEdge(4, 3, 1);
    g.AddWeightedEdge(3, 4, 1);

    // 4–1 (4)
    g.AddWeightedEdge(3, 0, 4);
    g.AddWeightedEdge(0, 3, 4);

    // 1–2 (6)
    g.AddWeightedEdge(0, 1, 6);
    g.AddWeightedEdge(1, 0, 6);

    // 2–4 (6)
    g.AddWeightedEdge(1, 3, 6);
    g.AddWeightedEdge(3, 1, 6);

    // 2–6 (6)
    g.AddWeightedEdge(1, 5, 6);
    g.AddWeightedEdge(5, 1, 6);

    g.kruskal();
    g.print();

    return 0;
}


```
