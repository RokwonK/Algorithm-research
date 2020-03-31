# SSC(Strongly Connected Component)

직역하면 강화 연결 요소  
***방향 그래프***에서 어떤 그룹에 정점 A->B로 가는 경로가 존재! 그러면 그 그룹 SCC라고 말함  
(나의 정리) 사이클이 있는 부분 그룹


SCC가 되는 그룹은 항상 최대로 정의가 됨  
**서로 다른 SCC**에서 뽑은 두점(A,B)가 A->B / B->A 경로가 동싱 존재하면 안댐  
즉, SCC끼리의 사이클이 있으면 안댐  
각각의 SCC는 독립적이어야한다 => 서로 다른 SCC에 같은 정점이 있으면 안됀다

SCC를 분류하는 방법은 두 가지 (DFS를 기반)  
1. `코사리주 알고리즘`
2. `타잔 알고리즘`

<br/>

## 코사리주 알고리즘

1. 주어지는 방향 그래프와 그 역방향 그래프 둘다 필요함 (컨테이너로 준비)
2. 스패닝트리를 만들어줌(DFS) => 가중치가 없기 때문에 DFS로 충분함 
3. 끝 점부터stack에 정접들을 넣어줌
4. 역방향 그래프를 이용하여 stack에서 pop하면서 그 정점에대한 SSC그룹(정점으로 갈 수 있는 곳을 전부 배열에 넣음)을 구함
5. SSC그룹에 속하면 Visit하면서 다른 그룹에는 속하지 못하게 만들어야 함

<br/>

- `정점 A에서 B로 순방향 그래프로도 갈 수 있고, 역방향 그래프로도 갈 수 있다면 사이클이 존재한다!`  

**이것을 Fact로 만들려면**
> 1. 어떠한 정점부터 SCC 만들지 중요함 so => 순방향 그래프에서 DFS로 stack을 이용해 리프 노드부터 넣는 것(결국 스택의 최상위에는 정점 루트가 들어 가게 될 거임)

> 2. 루트 정점을 기준으로 왼쪽자식부터 우측 자식들 방향으로 사이클 검사, 각 자식의 끝에 리프노드가 았으므로 사이클 확인 가능함 ( 리프노드 부터 검사하면 역방향 그래프에서 사이클 찾을 수가 없음)

```
순방향으로 자신의 밑의 노드에 도달할 수 있는데  
역방향으로 자신에서 출발해서 밑의 노드에 또 도달 할수 있다면 사이클이라는 것임  
이것을 알기 위해서는  
그러기 위해서 자신의 밑의 노드는 스택에 자신보다 밑에 쌓여야함  
so, stack을 이용해서 리프노드부터 바닥에 쌓는 것임
```

<br/>

- 시간복잡도 O(V + E) => DFS 2번이므로
- Code - 백준 2150(Strongly Connected Component)

```cpp
#include<iostream>
#include<algorithm>
#include<vector>
#include<stack>
#include<string.h>

using namespace std;

int V,E;
bool visit[100001];
vector<int> arr[10001];
vector<int> reverse_arr[10001];
vector<int> scc[10001];
stack<int> st;

bool compare(vector<int>& a, vector<int>& b) {
    return a[0] < b[0];
}

// 하나의 정점을 기준으로 사이클있는(SCC)그룹을 만듬
// 하나 하나의 SCC들은 결국 싸이클 없음 싸이클이 있는 것끼리 하나씩 다 묶어놨으므로
void scc_make(int index, int vertex) {
    visit[vertex] = true;
    scc[index].push_back(vertex);
    
    for(auto a : reverse_arr[vertex])
        if (visit[a] == false)
            scc_make(index,a);
}

//루트 정점을 기준으로 리프노드부터 stack에 넣음
void dfs(int vertex) {
    visit[vertex] = true;
    for (auto a : arr[vertex])
        if (visit[a] == false)
            dfs(a);
     
    st.push(vertex);
}

int main(void) {
    
    ios_base :: sync_with_stdio(0);
    cin.tie(0);
    
    int ans = 0;
    
    cin >> V >> E;
    
    for (int i = 0; i < E; i++) {
        int a,b;
        cin >> a >> b;

        //순방향과 역방향 그래프를 만듬
        arr[a].push_back(b);
        reverse_arr[b].push_back(a);
    }
    
    // 순방향 DFS 구하기 => 그래프가 따로따로 나누어져 있을수도 잇으므로 일단 다 돌려봄
    for (int i = 1; i <= V; i++)
        if (visit[i] == false)
            dfs(i);
    
    //visit 재활용
    for (int i = 1; i <= V; i++)
        visit[i] = false;
    
    //scc 만들기
    while (!st.empty()) {
        int vertex = st.top();
        st.pop();
        
        if (visit[vertex] == false)
            scc_make(ans++,vertex);
    }


//*********************************************************//
    //그룸별로 오름차순으로 출력해야해서
    for (int i = 0; i < ans; i++)
        sort(scc[i].begin(), scc[i].begin()+scc[i].size());
    sort( scc, scc+ans, compare);
    
    cout << ans << '\n';
    for (int i = 0; i < ans; i++) {
        for (auto a : scc[i])
            cout << a << ' ';
        cout << -1 << '\n';
    }
    
    
    return 0;
}

```




