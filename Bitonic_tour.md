# Bitonic tour

TSP(외판원 순회) 문제
- 모든 정정을 다 거치면서 마지막엔 시작한 부분으로 돌아올 때의 최소값
- 대표적인 NP-hard 문제

</br>

이 문제에 조건을 걸어 다항식 시간내에 풀 수 있다.
- 시작 점과 반환 점이 존재
- 시작점->반환점 증가하는 순으로 / 반환점->시작점 감소하는 순으로 이동하며 모든 정점을 들러야 한다

위와 같은 조건을 건 문제를 **Bitonic tour**라고 한다

> DP를 이용하여 다항식 시간 O(N^2) 안에 풀 수 있음

</br>

## 설명 

- 점의 좌표 : P1 ~ PN

- B(i,j) = Pi 에서 시작해 PN에 도착 후 Pj로 돌아가는 바이토닉 수열

- B(i,N) = i ~ N 까지 일직선으로 이은 것 == 반환점까지 간 것이기 때문에

- 부분 바이토닉 투어가 전체 바이토닉 투어를 이룬다
    > B(i, i+1)  
    = B(i, i+2) + dist(i+1, i+2);  
    = i~i+2 를 간다음 i+2에서 i+1로 감  
    = B(i, i+1) == B(i+1, i) 거꾸로 움직인 것과 같으므로 답은 같음

</br>

## Code

```cpp

double dist(const pair<int,int>& a, const pair<int,int>& b) {
    return sqrt( pow(a.first-b.first,2) + pow(a.second-b.second,2) );
}

double solve(int start, int end) {
    if (start == N-1) return dist(arr[end], arr[N-1]);
    if (end == N-1) return dist(arr[start], arr[N-1]);

    // start는 P(N)을 향해 가는 길 && end는 P(1)을 향해 가는 길
    // dp[start][end] == B(start,end)
    double &value = dp[start][end];
    if (value != 0) return value;

    // 오는 길과 가는 길이 겹치지 않기 위해 
    // 올때와 갈때 사용한 길을 제외한 다음 길 즉, 나온 값들 보다 하나 더 큰 값을 사용
    int next = max(start, end) + 1;

    // 다음 값을 가는 길에 사용할지, 오는 길에 사용할지 둘 중에 작은 것을 사용
    value = min(solve(start, next) + dist(arr[next],arr[end]), 
                solve(next, end) + dist(arr[next],arr[start]));

    return value;
}

```

