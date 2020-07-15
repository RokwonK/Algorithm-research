# Network Flow

한 정점에서 다른 정점까지 얼마나 많은 양이 흐르는가를 측정하는 알고리즘 ex) 교통 체증, 네트워크 데이터 전송  
즉, 최대 유량(Maxflow)을 구하는 것

## 용어
- Source : 시작 위치
- Sink : 끝 위치
- capacity(u,v) : u에서 v로 가는 용량
- flow(u,v) : u에서 v로 흐른 유량

</br>

## 중요한 Fact
1. 용량 제한 => 각각의 흐르는 유량은 정해진 용량을 넘지 못한다.
2. 유량 보존 => Source와 Sink를 제외한 모든 정점은 들어오는 유량과 나가는 용량이 같다.
3. local equilibrium => 특정 경로를 통해 보내는 유량은 그 경로에 포함된 간선 중 ***용량이 가장 작은*** 간선에 의해 결정됨
4. 유량 대칭 => flow(u,v)로 흐르는 것은 -flow(v,u)와 같다는 것 (유량 상쇄) 역행 했을때, 받은 유량을 보내는 것은 애초에 안들어 왔다는 것과 같다.
5. 각 간선의 용량은 비포화에서 포화(유량=용량)로만 변한다. 
    - 포화=>비포화는 이루어지지 않는다. 이 뜻은 흐른 유량이 사라진다는 뜻이므로 성립x
    - 유량 상쇄는 흐른 유량이 사라지는 것이 아닌 이미 흐른 길이므로 다른길을 찾게 해주는 것 사라지는 것이아니다. 항상 증가경로를 찾는다.

</br>

### 유량 상쇄에 대해서..
예시로 상대가 사과 3개를 가지고 있고 나는 사과를 가지고 있지 않다고 가정하자  
상대가 나에게 사과를 주면  
(상대=>나) +3이 되겠지만 반대로 생각하자면  
(나=>상대)에게 사과를 -3개를 준 것과 마찬가지  

즉, 증가 경로를 찾다 전에 찾아 놓은 경로로 지금 찾는 경로로 갈 수 있다면  
이 경로를 택했던 정점으로 받은 사과를 다시 넘긴다면 애초에 이 길은 그 상대가 흐른게 아니고 내가 흐른게 되고, 상대는 또 다른 증가 경로를 찾아다닌다.

</br>

## Ford-Fulkerson(포드-폴커슨)
- Source에서 시작해서 증가 경로를 찾음
- 찾은 경로 중 최소 용량을 가지는 것을 골라냄
- 모든 경로의 유량에 그 용량을 더해줌  

</br>

## Edmonds-Karp(에드모든 카프)
- 포드-폴커슨 방식을 사용하여 BFS로 구현한 알고리즘
- 증가경로를 찾고 그 경로에 유량을 더해주는 것을 반복
- 더 이상 증가경로를 찾지 못했을때(Sink에 도달하지 못했다는 뜻) 끝

```cpp
#include <iostream>
#include <queue>
#include <vector>

using namespace std;

const int INF = 987654321;

//용량
int capacity[256][256];
//유량
int flow[256][256];

int main(void) {
    ios_base :: sync_with_stdio(0);
    cin.tie(0);

    int N;
    int all_flow = 0;
    cin >> N;
    for (int i = 0; i < N; i++) {
        char a,b;
        int c;
        cin >> a >> b >> c;
        capacity[a][b] += c;
        capacity[b][a] += c;
    }

    while(true){
        queue<int> q;
        vector<int> parent(256, -1);
        q.push('A');
        parent['A'] = 0;

        //q가 비지 않았고 끝에 도달하지 않앗으면 계속 지속
        while(!q.empty() && parent['Z'] == -1) {
            int now = q.front();
            q.pop();

            for (int i = 0; i < 256; i++) {
                //수용력이 있는지
                if ( capacity[now][i]-flow[now][i] > 0 && parent[i] == -1) {
                    q.push(i);
                    parent[i] = now;
                }
            }
        }
        //Sink에 도달하지 못했다면 더 이상 증가 경로가 없다는 것
        if(parent['Z'] == -1) break;
        
        int amount = INF;
        // 증가경로 중 최소 용량을 찾음
        for (int i = 'Z'; i != 'A'; i = parent[i])
            amount = min(amount, capacity[parent[i]][i] - flow[parent[i]][i]);
        
        // 최소 용량을 지나온 경로에 전부 더해줌
        // 반대로 역간선에는 -해줌 (상대가 나한테 준건 +지만 반대로 내가 상대한테는 -준것과 같으니)
        for (int i = 'Z'; i != 'A'; i = parent[i]) {
            flow[parent[i]][i] += amount;
            flow[i][parent[i]] -= amount;
        }
        all_flow += amount;

    }
    cout << all_flow << '\n';

    return 0;
}

```


