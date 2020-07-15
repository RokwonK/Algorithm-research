# MCMF (Minimum Cost Maximum Flow)

1.최소 비용을 구하여 - 2.그 비용에 해당하는 간선으로의 최대유량을 구하는 것  
즉, 최단 거리(최소 비용)길로 유량을 채우면서 최대유량으로 만드는 것  

### 주의점
- 유량상쇄 시 비용또한 상쇄되어야 하므로 역방향 때의 비용에 -를 붙여 저장해줘야한다.

### 시간복잡도 UP
- 비용이 음수가 될 수 있으면 벨마포드를 써도 좋지만 SPFA를 사용하여 시간복잡도 끌어올리면 좋다.
    - SPFA는 최악의 경우 O(VE) 지만 평균 O(V+E)의 시간복잡도를 가진다.
- SPFA시 시간복잡도 : O( (V+E)*f ) => f의 최대 E

## Code

```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <cstring>
#include <algorithm>

using namespace std;

const int INF = 987654321;

// 용량
int capacity[803][803];
// 흐른 용량
int flow[803][803];
// 월급
int cost[803][803];

vector<int> arr[803];

int main(void) {
    ios_base :: sync_with_stdio(0);
    cin.tie(0);

    int N, M;
    cin >> N >> M;
    int source = N+M+1;
    int sink = N+M+2;

    for (int i = 1; i <= N; i++) {
        capacity[source][i] = 1;
        arr[source].push_back(i);
        arr[i].push_back(source);
    }
    for (int i = N+1; i <= N+M; i++) {
        capacity[i][sink] = 1;
        arr[sink].push_back(i);
        arr[i].push_back(sink);
    }

    for (int i = 1; i <= N; i++) {
        int num;
        cin >> num;

        for (int j = 0; j < num; j++) {
            int a,b;
            cin >> a >> b;

            capacity[i][N+a] = 1;

            arr[i].push_back(N+a);
            arr[N+a].push_back(i);

            cost[i][N+a] = b;
            cost[N+a][i] = -b;
        }
    }

    int ended = N+M+3;
    int answer_cnt = 0;
    int answer_money = 0;
    

    while(1) {
        int parent[ended];
        int dist[ended];
        bool inq[ended];
        
        memset(parent, -1, sizeof(parent));
        memset(inq, false, sizeof(inq));
        fill(dist, dist+ended, INF);

        queue<int> q;

        q.push(source);
        parent[source] = source;
        dist[source] = 0;
        inq[source] = true;

        // sink로 흐르는 유량을 구하되 
        // 현 상황(유량이 채워져을 때)에서 가장 최소비용으로 흐르는 곳을 찾음
        while(!q.empty()) {
            int now = q.front();
            q.pop();
            inq[now] = false;

            for (int next : arr[now]) {
                if (capacity[now][next] - flow[now][next] > 0 && dist[next] > dist[now] + cost[now][next] ) {
                    dist[next] = dist[now] + cost[now][next];
                    parent[next] = now;

                    if (!inq[next]) {
                        inq[next] = true;
                        q.push(next);
                    }
                }
            }

        }

        if (parent[sink] == -1) break;
        int amount = INF;
        answer_cnt++;
        
        // 흐른 유량의 중 최소 유량을 구함
        for (int i = sink; i != source; i = parent[i])
            amount = min(amount, capacity[parent[i]][i] - flow[parent[i]][i] );
        
        // 
        for (int i = sink; i != source; i = parent[i]) {
            // 역방향으로 즉, 유량상쇄를 하며 흘렀다면 
            // cost는 음수니 그 전에 정방향으로 흐른 값과 상쇄가 됨
            answer_money += cost[parent[i]][i] * amount; 
            flow[parent[i]][i] += amount;
            flow[i][parent[i]] -= amount;
        }

    }

    cout << answer_cnt << '\n' << answer_money << '\n';

    return 0;
}

```