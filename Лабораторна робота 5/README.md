```

# Лістинг 5.1 – Python-код реалізації програми алгоритму Пріма.

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



```
