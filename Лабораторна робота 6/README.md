```
# Лістинг 1 – Код реалізації алгоритму сортування купою

#include <stdio.h>

#define N 8
#define INF 1000000000

void dijkstra(int start, int pred[], int dist[], int w[N][N]) {
    int used[N];
    int i, j;

    for (i = 0; i < N; i++) {
        pred[i] = -1;
        dist[i] = INF;
        used[i] = 0;
    }

    dist[start] = 0;

    // Простий варіант "черги з пріоритетом": на кожному кроці
    // шукаємо вершину з мінімальною dist серед необроблених
    for (i = 0; i < N; i++) {
        int u = -1;
        int best = INF + 1;

        for (j = 0; j < N; j++) {
            if (!used[j] && dist[j] < best) {
                best = dist[j];
                u = j;
            }
        }

        if (u == -1) {
            break; // решта вершин недосяжні
        }

        used[u] = 1;

        // релаксація ребер з вершини u
        for (j = 0; j < N; j++) {
            if (w[u][j] < INF && !used[j]) {
                int nd = dist[u] + w[u][j];
                if (nd < dist[j]) {
                    dist[j] = nd;
                    pred[j] = u;
                }
            }
        }
    }
}

void print_path(int v, int pred[], int start) {
    if (v == start) {
        printf("%d", v + 1);
    } else if (pred[v] == -1) {
        printf("%d", v + 1);
    } else {
        print_path(pred[v], pred, start);
        printf(" -> %d", v + 1);
    }
}

int main(void) {
    // Матриця ваг для варіанта 19 (1..8 вершин)
    int w[N][N];
    int i, j;

    // ініціалізація INF та нулів на діагоналі
    for (i = 0; i < N; i++) {
        for (j = 0; j < N; j++) {
            if (i == j) {
                w[i][j] = 0;
            } else {
                w[i][j] = INF;
            }
        }
    }

    // ребра неорієнтованого графа (ваги з рисунка)
    // 1-2:6, 1-3:2, 1-4:4, 2-4:6, 2-6:6, 3-8:4,
    // 4-5:1, 5-6:4, 5-7:7, 6-7:5, 6-8:3, 7-8:5
    int a, b, c;
    int edges[][3] = {
        {1, 2, 6},
        {1, 3, 2},
        {1, 4, 4},
        {2, 4, 6},
        {2, 6, 6},
        {3, 8, 4},
        {4, 5, 1},
        {5, 6, 4},
        {5, 7, 7},
        {6, 7, 5},
        {6, 8, 3},
        {7, 8, 5}
    };

    int m = sizeof(edges) / sizeof(edges[0]);

    for (i = 0; i < m; i++) {
        a = edges[i][0] - 1;  // у масиві індексація з 0
        b = edges[i][1] - 1;
        c = edges[i][2];
        w[a][b] = c;
        w[b][a] = c;
    }

    int pred[N];
    int dist[N];
    int start = 0; // вершина 1 (індекс 0)

    dijkstra(start, pred, dist, w);

    printf("Найкоротші відстані від вершини 1:\n\n");
    for (i = 0; i < N; i++) {
        printf("До вершини %d: dist = %d, шлях: ", i + 1, dist[i]);
        print_path(i, pred, start);
        printf("\n");
    }

    return 0;
}

