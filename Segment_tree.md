# Segment_tree

보통 일차원 배열에서 특정 범위에 조건을 단 질의에 답하기 위해서 O(N)( 범위만큼의 시간)이 걸린다.  
물론, 구간합이나, 최대 최소 같은 경우는 미리 부분합으로 구할 수 있지만. 도중 배열의 원소값이 바뀐다면?  
update를 하는데 O(1)(해당 원소 바꾸기) 후에 부분합을 다시 구해야하므로 O(N)의 시간이 걸린다.  

**Segment Tree**는 공간복잡도를 약 3배 늘리는 대신 구간에 대한 질의에 logN, update에 logN의 시간이 걸린다.  
원소의 값을 바꾸어도 logN만의 시간만에 원하는 답을 찾을 수 있다.

</br>

## 원리
Segment는 트리 구조로 각 노드가 구간에 해당하는 값을 지니고 있다.  
예를 들어, 길이 8짜리의 배열이 있을때, 이 트리의 리프노드는 각각의 index이다.  
그 리프노드의 부모는 ex) 리프노드 index 1과 2의 부모는 구간 1~2에 해당하는 값을 가지고 있다. 그위는 1~2, 3~4의 부모 1~4이다.  
이런 식으로 처음구간 1~8을 계속 반으로 쪼개어 구간에 해당하는 값을 각 노드가 지닌다. 리프노드에 도달했을 시, 재귀하며 각 구간마다 원하는 값을 채워나가는 것이다.   
위를 통해 알 수 있는 것은
- 리프노드의 구간은 자신 하나다.
- 부모구간은 2개의 자식 구간의 합친 것과 같다.
- 계속 반으로 쪼개면서 내려가기에 트리의 높이는 log(배열의 길이) 수준이다.

이러한 특성 덕분에 질의를 던져도 높이인 log(배열의 길이) 시간 안에 원하는 값을 구할 수 있다.  
또한, 특정 원소를 바꿀때에도 그 원소가 포함되는 구간(log(배열의 길이))만 바꾸면 되기 때문에 아주 유용하다.

</br>

## Code

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

int N, M, K;
long long segment_tree[4000001];
long long arr[4000001];

//Seg ment 만들기
long long make_segment(int index, int start, int end) {

    //리프 노드에 도달했을때 원소를 집어 넣어줌
	if (start == end)
		segment_tree[index] = arr[start];
    // 반환하면서 즉, 부모노드는 두개의 자식노드의 합을 가짐(구간 합)
	else
		segment_tree[index] = make_segment(index*2+1, start, (start+end)/2) + make_segment(index*2+2, (start+end)/2+1, end);

	return segment_tree[index];
}

// 특정 구간에 대한 합을 구함
long long part_sum(int i, int start, int end, int left, int right) {

    // 답을 원하는 구간에서 벗어났을 때
    // 구간 합을 구하기 때문에 안 본 것과 마찬가지인 0을 리턴
	if ( left > end || right < start)
		return 0;
    // 원하는 구간안에 속하는 구간을 만났을때
	else if (left <= start && right >= end)
		return segment_tree[i];
    // 자식들을 확인
	else
		return part_sum(i*2+1,start,(start+end)/2, left, right) + part_sum(i*2+2,(start+end)/2+1, end, left, right);
}

// 원소가 바뀌었을때, 
void update_value(int change_i, long long diff, int i ,int start, int end) {
    // 바꾸는 원소가 구간에 안에 속하지 않을때,
	if (change_i < start || change_i > end)
		return;
	
    // 위의 if문에 걸리지 않는다면 원소가 이 구간안에 속함
    // 기존 원소와 바뀐 원소간의 차이만큼 더해줌
	segment_tree[i] += diff;

    // 자식들도 바꿔주기 위해 자식들을 호출
	if (start != end) {
		update_value(change_i, diff, i*2+1, start, (start+end)/2 );
		update_value(change_i, diff, i*2+2, (start+end)/2+1, end );
	}
}

int main() {

	ios_base :: sync_with_stdio(0);
	cin.tie(0);
	cout.tie(0);

	int a,b,c;

	cin >> N >> M >> K;

	for(int i = 0; i < N; i++)
		cin >> arr[i];

	make_segment(0, 0, N-1);

	for(int i = 0; i < M+K; i++) {
		cin >> a >> b >> c;
		if (a == 1) {

			update_value(b-1, c - arr[b-1], 0, 0, N-1);
			arr[b-1] = c;
		}
		else
			cout << part_sum(0, 0, N-1, b-1, c-1) << '\n';
	}

	return 0;
}
```

## 생각
Segment(구간)는 유용한만큼 많이 변형되어서 쓸 수 있음.  
수많은 변형 알고리즘의 기초이므로 제대로 공부하자!

</br>

## 이어지는 알고리즘(공부하자!)
- Penwick Tree
- lazy propagation
- Plane Sweeping
- Merge Sort Tree
- Persistent Segment Tree
- Dynamic Segment Tree
- Mo's algorithm - sqrt decomposition