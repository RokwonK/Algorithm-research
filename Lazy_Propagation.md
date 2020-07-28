# Lazy_Propagation

세그먼트 트리에서 하나의 원소를 수정할 때, O( logN )의 시간이 걸린다.  
그렇다면, 하나의 구간의 값을 바꾼다면? => 기존 세그먼트에서는 O(원소수*logN)만큼의 시간이 걸린다.

**Lazy_Propagtion**을 사용한다면 구간을 전부 바꾸는 시간도 O( logN )만에 바꿀 수가 있다.

</br>

## 원리
Lazy Propagation을 번역한다면 `느린전파`이다.  
말 그대로, 구간을 바꾸는 질의가 들어올때, 전부 한번에 바꾸지 않고 나중에(느리게) 바꾼다는 뜻이다.  
질의를 받았을때,
1. 그 구간에 포함되는 구간을 찾는다.
2. 그 자식으로는 내려가지 않고 해당 구간만 값을 변경한다.
3. 자식들에게는 나중에 너희의 값을 바꾸라는 지시를 내린다(자식의 lazy값에 넣는다)
    - 이렇게 되면 O(log N)(해당 구간이 가장 처음 등장하는 곳들)만 방문한다. (구간에 대한 질의와 같음)
4. 만약 나중에 어느 노드에 방문했을때(질의 같은 것으로 찾을때), 그 노드에 lazy값이 존재한다면, 그 값만큼 자신의 값을 변경하고 위와 똑같이 자신의 자식들에게 lazy값을 넘겨준다.

즉, 질의 받았을때, 값을 당장 바꾸는 것이 아니라 구간에 해당하는 가장 위 부모에게만 값을 전달한다.(어차피 자식들은 부모에 포함하기 때문에 가능), 각 부모들은 자식들에게 너네는 값을 바꿀 의무가 있다는 lazy를 전파해두고, 나중에 그 노드를 방문할 일이 있을때, 값을 변경한다.  
값을 변경하거나 자식들에게 lazy값을 넘겨주는 행위, lazy가 있으면 그 값에 따라 자신의 값을 변경하는 행위는 O(1)이기 때문에 맨 처음 가장 윗 부모가 lazy를 받는 O(logN)이 총 시간 복잡도이다.


## Code
```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#define MAXN 1000010

using namespace std;

typedef struct Seg{
    long long v = 0;
    long long lazy = 0;
}Seg;

Seg tree[3*MAXN];
long long arr[MAXN];

// segment 만들기
long long init(int start, int end, int loc) {
    if (start == end) 
        return tree[loc].v = arr[start];
    
    int mid = (start+end)/2;
    return tree[loc].v = init(start, mid, loc*2) + init(mid+1, end, loc*2+1);
}

// 구간의 값을 바꿀는 함수
void update(int part_s, int part_e, int start, int end, int loc, long long plus) {
    
    // 방문한 노드가 lazy가 존재할 경우
    // lazy에 해당하는 값으로 변경해주고 자식들에게 lazy를 전달
    if (tree[loc].lazy != 0) {
        tree[loc].v += (end-start+1)*tree[loc].lazy;
        if (start != end) {
            tree[loc*2].lazy += tree[loc].lazy;
            tree[loc*2+1].lazy += tree[loc].lazy;
        }
        tree[loc].lazy = 0;
    }


    if(start > part_e || part_s > end) return;
    // 바꾸는 구간에 해당하는 가장 윗 노드(부모 노드)에 바꾸는 값을 전달해주고
    // 그 노드는 자식들에게 lazy로 값을 바꿀게 있다는 것을 전파함
    else if (start >= part_s && part_e >=end) {
        tree[loc].v += (end-start+1)*plus;
        if (start != end) {
            tree[loc*2].lazy += plus;
            tree[loc*2+1].lazy += plus;
        }
        return;
    }

    int mid = (start+end)/2;
    update(part_s, part_e, start, mid, loc*2, plus);
    update(part_s, part_e, mid+1, end, loc*2+1, plus);
    tree[loc].v = tree[loc*2].v + tree[loc*2+1].v;
}

// 구간 합 구하는 함수
long long sum(int part_s, int part_e, int start, int end, int loc) {

    // 각 노드를 방문할때마다 lazy가 잇는 확인해 줘야함
    // 있다면 위와 같이 바꾸고 전파해주는 것
    if (tree[loc].lazy != 0) {
        tree[loc].v += (end-start+1)*tree[loc].lazy;
        if (start != end) {
            tree[loc*2].lazy += tree[loc].lazy;
            tree[loc*2+1].lazy += tree[loc].lazy;
        }
        tree[loc].lazy = 0;
    }

    if(start > part_e || part_s > end) return 0;
    else if (start >= part_s && part_e >=end) 
        return tree[loc].v;
    
    int mid = (start+end)/2;
    return sum(part_s, part_e, start, mid, loc*2)+ sum(part_s, part_e, mid+1, end, loc*2+1);
    
}


int main(void) {
    ios_base :: sync_with_stdio(0);
    cin.tie(0);

    int N,M,K;
    
    cin >> N >> M >> K;

    for (int i = 1; i <= N; i++)
        cin >> arr[i];

    init(1, N, 1);

    for (int i = 0; i < M+K; i++) {
        int a,b,c;
        long long d;
        cin >> a;
        if (a == 1) {
            cin >> b >> c >> d;
            update(b, c, 1, N, 1, d);
        }
        else {
            cin >> b >> c;
            cout << sum(b, c, 1, N, 1) << '\n';
        }

    }

    return 0;
}
```