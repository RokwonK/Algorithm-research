# SPFA (Shortest Path Faster Algorithm)를 위한 

다익스트라, 벨만포드, SPFA의 차이점 소개  
**(다익스트라, 벨만포드의 흐름을 알고있다는 전제하에 설명)**
#

## 다익스트라(Dijkstra)
- 이름  
그냥 사람이름이다 - 에츠허르 비버 데이크스트라(네덜란드 컴퓨터 과학자)
- 사용  
하나의 정점에서 다른 정점들까지의 최단거리를 구해줌(BFS , *우선순위 큐 사용: 값 갱신을 최소화 시켜줌)
**시간 복잡도** O(E logV) => 최악의 경우 O(V^2 logV)  
**속도에 영향을 주는 요소** => 큐에서 노드를 꺼내오는 횟수(E) 와 우선순위 큐로서 갱신 횟수(logV)

- 단점  
음의 가중치가 있을 시 기능상실  
다익스트라의 전제조건은 '정점을 지날수록 가중치가 증가한다'  
but, 음의 가중치가 존재할 시 정점을 지나칠때 가중치가 낮아 질수도 있다는 것  

*음의 가중치 있을 시 예시*

<div>
    <img src="https://goodgid.github.io/assets/img/algorithm/dijkstra_algorithm_6.png" style="width:400px" alt="음의 가중치 있을 시">
</div>

- 다익스트라 코드

```cpp
#include<iostream>
#include<queue>
#include<vector>
using namespace std;

//각 정점에서 이어지는 정점 및 가중치 값들을 저장
vector< pair<int,int> > arr[N];

int main(void) {
    //최소 힙으로 만들어주기
    priority_queue< pair<int,int> > q;

    //각 정점들 INF로 초기화해주기
    int vertex_weight[N] = {INF, };

    //시작 정점, 처음 가중치(0부터 시작)
    q.push({v, 0});

    while (!q.empty()) {
        int vertex = q.top().first;
        int weight = q.top().second;
        q.pop();

        if (vertex_weight[vertex] > weight) {
            vertex_weight[vertex] = weight;

            //현재의 정점에서 연결된 정점들을 모두 큐안으로~
            for (auto a : arr[vertex])
                q.push({a.first, a.second+weight});
        }
    }
}
```
#

## 벨만포드(Bellman_Ford)
- 이름  
벪만 과 포드 과학자의 이름을 붙임

- 사용  
하나의 정점에서 다른 정점들까지의 최단거리를 구해줌  
음의 가중치를 허용O  
음의 사이클(무한히 한 사이클을 도는 것)을 잡아낼 수 있음  
**시간 복잡도** O(EV)  
**속도에 영향을 주는 요소** => 큐에서 노드를 꺼내오는 횟수(E) 와 우선순위 큐로서 갱신 횟수(logV)

- 원리  
비둘기 집의 원리를 사용 => 비둘기 집이 N-1개 비둘기가 N마리 일 경우 필연적으로 하나의 비둘기 집에 두마리 이상의 비둘기가 들어갈 수 있음 

**이 말 뜻은 하나의 노드가 갱신 될 수 있는 횟수는 최대 N-1번 까지라는 뜻 만약 그 이상이라면 음의 사이클이 존재한다는 것**  
코드를 돌리는 중에 음의 사이클이다! 라고 단정지을 수 없음 그래서 N-1번을 돌린 것과 N번 돌린 것이 다르다면 음의 사이클이라고 판단

>*최대 횟수 일때의 예시*  
사진 첨부

- 각 점점마다 모든 간선을 검사 음의사이클이 있던 없던 O(VE) => 모든 간선에 대해 업데이트 진행  
코드는 간단하지만 불필요한 작업이 많음

- 벨만포드 코드

```cpp
#include<iostream>
#include<vector>
using namespace std;

//N은 정점수, M은 간선 수
int N, M;
struct info {
    int x;  //시작 정점
    int y;  //도착 정점
    int count;  //가중치
}

vector< info > arr;
//각 정점들 INF로 초기화
vector< int > weight(N+1, INF);
int main(void) {

    //각 값들 갱신

    //정점마다 (V)
    for (int i = 1; i <= N; i++) {
        //모든 간선 (E)
        for (int j = 0; j < arr.size(); j++) {
            int x = arr[j].x;
            int y = arr[j].y;
            int count = arr[j].count;

            if (weight[x] != INF && weight[y] > weight[x]+count)
                weight[y] = weight[x]+count;
        }
    }


    // 음의 사이클이 있는지 검사, 모든 간선 즉,
    // 최대 N-1번은 확인했으니 한번더 돌렸을때 값이 변한다면 음의 사이클 존재

    //모든 간선 한번 검사(E)
    for (int j = 0; j< arr.size(); j++) {
        for (int j = 0; j < arr.size(); j++) {
            int x = arr[j].x;
            int y = arr[j].y;
            int count = arr[j].count;

            if (weight[x] != INF && weight[y] > weight[x]+count)
                cout << '음의 사이클 발생';
        }
    }
}
```

# 

## SPFA(Shortest Path Faster Algorithm)

- 사용  
위의 두 알고리즘과 마찬가지로 하나의 정점에서 다른 정점들까지의 최단거리를 구해줌  
벨만포드을 개선한 알고리즘! 아이러니하게도 다익스트라보다 더 빠른 시간복잡도를 가짐  
**<pr style="color:red">벨만포드</pr>** 는 모든 간선에 대해 업데이트하지만   
**<pr style="color:red">SPFA</pr>** 는 바뀐 정점과 연결된 간선에 대해서만 업데이트  
**시간 복잡도** O(E + V) => 최악의 경우 O(VE)  
>더 이상 업데이트할 노드가 없다면 한번의 서칭이면 끝남,  
즉, 모든 노드가 N-1번 업데이트를 진행한다면 최악의 경우가 되는 셈

- 원리  
벨만포드의 원리인 비둘기 집의 원리와 같음

- SPFA 코드
```cpp
#include<iostream>
#include<queue>
#include<vector>
using namespace std;
//N은 정점 갯수, M은 간선 갯수
int N, M;

//N개의 정점에 first에 연결된 노드, second에 가중치
vector < pair<int,int> > arr[N];

//각 정점을 몇번 들렸는지 세줌, N이상 돌면 음의 사이클로 판명
int cycle[N] = {0, };

//각 정점 최단거리를 저장
int weight_count[N] = {INF, };

//queue안에 들어있는가
bool inQ[N] = {false, };

int main(void) {
    queue<int> q;

    //시작정점 값 넣어주기
    weight_count[start] = 0;
    inQ[start] = true;
    cycle[start]++;
    q.push(start);

    while( !q.empty() ) {
        start_v = q.front();
        q.pop();

        //큐에서 빼냈으니 큐에 없다고 저장
        inQ[start_v] = false;

        //연결된 정점 확인
        for (auto a : arr[start_v]) {
            int next_v = a.first;
            int weight = a.second;

            
            if(weight_count[next_v] > weight_count[start_v] +  weight) {

                //갱신 값이 더 좋다면 갱신해주기 
                weight_count[next_v] = weight_count[start_v] +  weight);

                // 큐 안에 있다는 것은 애초에 그 길로 갔다는 뜻 - 방문횟수에 증가시키지 않음
                
                //큐 안에 없다면 큐에 넣어주고 방문한 횟수(큐에 들어간 횟수, 갱신) 증가시키기
                if (inQ[next_v] == false) {
                    cycle[next_v]++;

                    //방문한 횟수가 N번을 넘어서면은 음의 사이클이 있는 것
                    if(cycle[next_v] >= N) {
                        cout << '음의 사이클 존재'
                        return;
                    }

                    q.push(next_v);
                    inQ[next];
                }
            }
        }
    }
}
```





