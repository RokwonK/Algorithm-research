# Mo's_Algorithm

질의에 업데이트가 없을때, 쿼리들을 미리 받아 정렬해(오프라인 쿼리) 질의들을 처리하는 방식.  
값에 대한 수정이 없으므로 쿼리의 순서는 상관이 없음. 사용자가 처리하게 쉽게 정렬해 원하는 순서대로 처리함.  

</br>

## 원리
앞에서 사용한 구간을 최대한 재사용해 시간적 효율성을 가짐.  
평방 분할 아이디어를 사용해 그룹을 나누고 순서를 배치함.  
- 평방 분할처럼 각각의 원소가 가상의 그룹을 가짐 √N개.
- 앞에서 사용한 구간을 최대한 재사용한다는 것은 어떤 구간질의 안에 포함되는 구간질의를 먼저 처리한다는 뜻.
- 각 구간의 시작 index를 확인해 그룹이 빠른 순서대로 정렬 / 같다면 끝index가 빠른 순으로 정렬
- 이렇게 정렬된 쿼리는 시작그룹순으로 단조증가, 마지막인덱스 순으로 단조증가함.
- 두 개의 포인터(시작과 끝)를 가지고 이전 쿼리의 위치에서 다음 쿼리의 위치로 포인터의 위치를 바꾸면서 처리.

1. 이제 쿼리를 보면 앞의 쿼리를 처리하고 다음 쿼리에서 볼때
    - 이전 쿼리와 시작index의 그룹이 겹친다면
        - 그 그룹내에서만 움직이므로 O(√N)이 걸림
        - 끝index는시작index그룹이 같은 쿼리 안에서는 항상 단조 증가함 O(N)
    - 이전 쿼리와 시작index 그룹이 다름(큼)
        - 시작index가 최대 N만큼 차이날 수 있음 그럼 O(N)이 걸림.
        - 끝 index는 항상 단조 증가 이므로 O(N-(앞의 그룹 수)*√N)  

즉, 시작index의 변화는 처음부터 끝까지(정렬된 쿼리순으로 볼때, 그룹의 수가 작아지는 일은 없음) `총 O(N + Q√N)`  
마지막 index의 변화는 총 N+(N-√N)+(N-2√N)... `총 O(N√N)`  
`총 시간 복잡도 O( (N+Q)√N )`이 걸림

</br>

## Code
```cpp
#include <iostream>
#include <algorithm>
#include <cstring>
#include <vector>
#include <cmath>
#define MAX 100001

using namespace std;

int sq;

// 쿼리를 정렬
struct info{
    int index,s,e;
    bool operator<(const info& b) const {
        // 시작index(s)의 그룹 순으로
        if (s/sq != b.s/sq) return (s/sq) < (b.s/sq);
        // index그룹이 같다면 마지막 index가 증가하는 순으로
        return e < b.e;
    }
};

int arr[MAX];
int q_answer[MAX];
int appear[MAX*10];
vector<info> q;

int main(void) {
    ios_base :: sync_with_stdio(0);
    cin.tie(0);

    int N;
    cin >> N;
    sq = sqrt(N);

    for (int i = 1; i <= N; i++) cin >> arr[i];

    int M;
    cin >> M;

    for (int i = 0; i < M; i++) {
        int a, b;
        cin >> a >> b;
        q.push_back({i,a,b});
    }

    sort(q.begin(), q.end());
    int s = q[0].s;
    int e = q[0].e;
    int answer = 0;
    // 맨 처음 쿼리를 미리 처리
    for (int i = s; i <= e; i++) {
        if (appear[arr[i]] == 0) answer++;
        appear[arr[i]]++;
    }
    q_answer[q[0].index] = answer;


    for (int i = 1; i < M; i++) {
        int ns = q[i].s;
        int ne = q[i].e;

        //이전 쿼리와 비교해서 s와 e를 바꿈
        while(s < ns) {
            appear[arr[s]]--;
            if (appear[arr[s]] == 0) answer--;
            s++;
        }
        while(s > ns) {
            --s;
            appear[arr[s]]++;
            if (appear[arr[s]] == 1) answer++;
        }
        while(e < ne) {
            ++e;
            appear[arr[e]]++;
            if (appear[arr[e]] == 1) answer++;
        }
        while(e > ne) {
            appear[arr[e]]--;
            if (appear[arr[e]] == 0) answer--;
            e--;
        }
        q_answer[q[i].index] = answer;
    }

    for (int i = 0; i < M; i++) cout << q_answer[i] << '\n';

    return 0;
}
```


