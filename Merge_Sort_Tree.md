# Merge_Sort_Tree
기존 Segment 트리와 Merge Sort를 사용하는 만듬.  
각각의 노드가 각 구간에 해당하는 모든 원소를 지닌다(Merget Sort와 같음).  


## 원리 
처음 segment트리르 만들때, 각 원소가 구간에 해당한다면 그 구간에 자신의 원소를 추가하면서 트리를 만든다.  
모두 만들어 졌을때, 각 노드는 해당하는 원소를 전부 가지고 있을 것  
예를 들어, 어떤 범위에서 k보다 큰 수를 찾으라고 할때, 그 구간에 포함되는 노드에서 보다 큰 수의 갯수를 리턴해주면 된다.


## Code 

```cpp
#include <iostream>
#include <algorithm>
#include <cstring>
#include <vector>
#define MAX 100001

using namespace std;

vector<int> seg[MAX*4];

void update(int start, int end, int v, int index, int loc) {
    if (start > index || end < index) return;

    seg[loc].push_back(v);
    if (start != end) {
        int mid = (start + end)/2;
        update(start,mid,v,index,loc*2);
        update(mid+1, end, v, index, loc*2+1);
    }
}

int q(int start, int end, int a, int b, int v, int loc) {
    if (start > b || end < a) return 0;
    else if (a <= start && end <= b) {
        int index = upper_bound(seg[loc].begin(), seg[loc].end(), v) - seg[loc].begin();
        if (seg[loc][index] > v) return seg[loc].size() - index;
        else return 0;
    }

    int mid = (start + end) / 2;
    return q(start, mid, a, b, v, loc*2) + q(mid+1, end, a, b, v, loc*2+1);
}


int main(void) {
    ios_base :: sync_with_stdio(0);
    cin.tie(0);

    int N;
    cin >> N;
    int s = 1;
    int e = N;

    for (int i = 1; i <= N; i++){
        int v;
        cin >> v;
        update(s,e,v,i,1);
    }

    for (int i = 1; i < MAX*4; i++) sort(seg[i].begin(), seg[i].end());

    int M;
    cin >> M;

    for (int i = 0; i < M; i++) {
        int a,b,c;
        cin >> a >> b >> c;
        cout << q(s,e,a,b,c,1) << '\n';
    }

    return 0;
}
```