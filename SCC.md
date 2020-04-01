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

<br/>
<br/>


## 타잔 알고리즘

DFS를 수행할 때 생성되는 DFS 트리의 간선의 정보를 이용하여 ALL DFS 한번으로 모든 SCC를 구하는 것  
`시간 복잡도 = O(V + E)` 지만 DFS 한 번만에 끝내고 역방향 그래프를 저장할 필요가 없음  
대신에 정점값을 저장하는 배열 1개가 필요함(간선의 개수에 따라 공간복잡도는 차이남)  
BUT, 시간적으로 코사리주 알고리즘보다 좋음

- DFS의 호출 순서에 따라 정점을 stack에 push함  
(한 정점의 스택 위에 있는 값들은 스패닝 트리 기준으로 자식들임)  
자식들의 자식으로 DFS로 넘어가면서 리프노드에서 값을 가지고 

- 맨처음 discover 값은 자신의 정점 번호임(DFS 맨 처음 들르는 것 기준으로 번호를 매겨줌)  
(사이클 즉, discover 값이 있는 곳으로 돌아 왔을때 하나의 SCC가 될 수 있는 것임)  

- 하지만 하나의 SCC안에 사이클이 하나만 존재하라는 법은 없음  
so, 그 사이클 중에서 가장 작은 정점 번호(root에 가깝게)로 SCC를 만들어야 하나의 독립적인 큰SCC가 완성됨  

- so, discover 값이 존재하면서 SCC가 아닌 곳에 왔다면  
(사이클이 있어서 discover가 존재하는 곳에 다시 왔다는 것임) 그 discover 값을 리턴시킴

- 리턴 받은 정점에서는 자신의 discover값과 리턴 받은 값을 비교하여 더 작은 값을 변수에 저장함

- discover값과 리턴 값이 같다면 더 이상의 사이클은 없다는 것!(사이클 없는 리프노드일 경우도 갈 곳이 없으니 두 값이 같음)

- 그 값의 스택 위(그 값의 자식들)은 모두 같은 사이클 안에 있다는 뜻이니 하나의 pop시키며 SCC로 만듬

`위상 정렬`을 이용한 방법 Why?  
- 순서도 있게 차례대로 감
- 순서도에 문제가 있다 => 싸이클이 생김 => 어떻게 아는가? (discover값은 존재하는데 SCC가 아니다)일때
- 싸이클의 첫 루트를 찾음(return값으로 받음)
- return 받은 값과 discover값이 같으면 그 정점까지 사이클이 있다는 것
- 즉 스택에서 그 정점위에 것들은 자신의 자식들이고 그 자식들이 사이클을 만들었다는 것이므로 하나의 SCC를 만들었다는 것

```cpp
#include <iostream>
#include <algorithm>
#include <vector>
#include <stack>
#define MAX_V 10000
using namespace std;

int v, e, , x, y, r, c;

// 자신과 이어지는 가장 끝 정점
int discover[10001];
// 이 정점이 scc에 들어가 있는가
int scc[10001];
// 방향을 집어넣음
vector<int> arr[10001];


vector<int> res[10001];
stack<int> st;

bool cmp(vector<int>& a, vector<int>& b) {
    return a[0] < b[0];
}
int dfs(int here) {
    discover[here] = c++;
    int ret = discover[here];
    st.push(here);

    // 사이클이 하나도 발생안하면 마지막 리프 노드에서는 for문에 들어가지 않으니 각각의 정점이 하나의 SCC가 될거임
    for (int there : vt[here]) {
        //처음 정점을 방문하였다 => 정점에 값을 넣고 자식으로 넘어감
        if (discover[there] == -1)
            ret = min(ret, dfs(there));

        // 정점을 방문하였는데 다시방문했다? 그런데 SCC도 아니다? 그럼 사이클 발생했다는 뜻
        // 그 곳의 값(discover와 return값 중 작은 값)을 가져와 ret에 넣음
        else if (scc[there] == -1)
            ret = min(ret, discover[there]);
    }

    // discover값과 ret값이 같다는 스택에서 그 정점위에는 전부 사이클이 있음 -> 하나의 SCC

    // 사이클이 없으면 맨처음 ret에 discover값을 넣었으므로 같음
    // 그렇다면 스택에서도 이 정점이 가장 위일 거임 그 정점 하나가 SCC가 됨
    if (ret == discover[here]) {
        int t;
        while (t == here) {
            t = st.top();
            st.pop();

            scc[t] = r;
            res[r].push_back(t);
        }
        sort(res[r].begin(), res[r].end());
        r++;
    }
    return ret;
}
int main() {
    scanf("%d%d", &v, &e);
    vt.resize(v + 1);
    for (int i = 0; i < e; i++) {
        scanf("%d%d", &x, &y);
        vt[x].push_back(y);
    }
    memset(discover, -1, sizeof(discover));
    memset(scc, -1, sizeof(scc));
    for (int i = 1; i <= v; i++) {
        if (discover[i] == -1)
            dfs(i);
    }
    sort(res.begin(), res.end(), cmp);
    printf("%d\n", r);
    for (int i = 0; i < r; i++) {
        for (auto h : res[i]) 
            printf("%d ", h);
        printf("-1\n");
    }
    return 0;
}
```


