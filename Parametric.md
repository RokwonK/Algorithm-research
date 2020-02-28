# Parametric Search

탐색 방법의 일종
기존 탐색(이분 탐색)이 **배열의 요소들을 기준으로 나눠** 탐색하는 것이라면
파라매트릭은 **수직선상을 기준으로 값을 나눠** 탐색하는 기법
이분탐색 삼분탐색이 있음

# 

## 파라메트릭 서치(이분 탐색)


설명 1
- 수직선상에 존재하는 최대값에서 반으로 쪼개며 조건에 맞춰 범위설정을 해주는 방식

- 배열안의 요소들은 다 본다. 조건을 만족하면 요소의 유무에 따라 수직선상의 값의 범위를 이분할해 바꿔준다

- 이분 탐색의 응용 - 배열의 요소가 아닌 수직선상에서 조건에 충족하는 (최소, 최대값)을 찾는 것 이 과정에서 배열의 요소들을 훑어보는 것이 필요함

설명 2
- 최적화 문제를 => 결정문제로 바꾸는 것  
(최솟값, 최대값)구하기를 => 가정한 값을 기준으로 탐색해 최적의 답 구하기



단점 - 단조증가/감소 일때만 사용 가능
  

> 예시코드

```cpp
// 심사대의 갯수, 심사받을 인원의 갯수를 주고
// 각 심사대가 한 번 심사할때 심사하는 시간을 주었을때
// 모든 인원이 검사를 받을 때, 최소시간 구하기

int main(void) {
    //심사대 갯수, 심사받을 인원 수
    cin >> N >> M;
    int arr[N];

    //각 심사대의 심사시간
    for(int i =0; i< N; i++)
        cin >> arr[i];

    //수직선상의 최솟값 최댓값 설정해주기
    // 각각 검사대들은 병렬이므로 동시에 다른인원이 사용가능하나
    // 말 그대로 나올 수 있는 최솟값과 최댓값만 구해줌

    //최솟값 - 가장 적은 심사대에서 만 검사를 받음
    int left = arr[min_value] * M; 
    //최댓값 - 가장 긴 심사대에서만 검사를 받음
    int right = arr[max_value] * M;

    // 수직선상을 만들었으니 그 안에서 조건을 만족하는 최솟값을 구함
    while (left <= right) {
        // mid가 최소시간값 이라고 가정
        mid = (left + right) /2;
        sum = 0

        // 현재 가정하는 최솟값에서 각 배열 요소들을 나눔
        // 나눈 값들을 sum에다 차곡차곡 저장
        // why? 심사대는 병렬적으로 사용 가능함
        // 주어진 시간안에 각 심사대는 몇명의 인원을 심사할 수 잇는지를 파악하고 sum에 넣어줌
        for (int i = 0; i < N; i++)
            sum += mid / arr[i];

        // sum 즉, 각 심사대들이 mid 최소시간 안에 심사할 수 있는 인원의 합
        //sum이 인원 수 M보다 크거나 같으면 mid의 값이 아직 최소시간이 아닐 수도 있음
        // 일단 최소시간일 수도 있으니 답에 저장해놓고
        // 더 작은 최소시간을 구하기 위해 최대범위값을 줄이기
        if ( sum >= M) {
            ans = min(ans, mid);
            right = mid-1;
        }
        // mid시간안에 사람들을 다 검사할 수 없음
        // 더 많은 시간이 필요하므로 최소범위값을 늘림
        else
            left = mid+1;
    }

    // 이 문제로 봤을때 수직선상에 해당하는 것은 => 가정할 수 있는 시간의 범위(최소와 최대를 범위로 지정)
    // 1. 구하고자 하는 값을 수직선으로 두고
    // 2. 조건 만족하게 수진선상의 범위를 이분으로 나누면서 탐색
    // 3. 배열의 요소들을 쭉 탐색해 설정한 값이 조건에 부합하냐 안하냐에 따라 이분한 좌측 범위로 갈 것인지 우측 범위로 갈것인지를 판단


}
```
- 시간복잡도 : 이분탐색 O( log(수직선상) )  
but, 정확히 말하자면 배열들을 mid하나마다 한번 씩 쭉 봐야하므로 O( N * log(수직선상) )


## 파라메트릭 서치(삼분 탐색)

- 단조증가/감소 일때는 범위를 좌측이나 우측으로 재설정하기 용이하다

- but, 단조증가가 아니라면?(볼록 함수일 경우) 삼분탐색을 사용한다.

1. 삼등분을 한다 -> 내분점의 공식을 이용하여 각점을 구함
2. 각 점을 기준으로 비교 p와 q로 둠
<div> <img src ="https://mblogthumb-phinf.pstatic.net/MjAxOTAxMDNfMjk2/MDAxNTQ2NDUwMjUzMDMw.YOzzbml1_9tXAQjtxXBq_dClTOI4cyq81qOt0EI6G3gg.zDbIOLxv57j1IEc_EgHnjvg8ECPaqAq5hWtaaGos-rAg.PNG.kks227/2.png?type=w2" style ="width:400px"> </div>

3. 위를 봤을때 처음 초록점을 p, 그 다음 초록점을 q로 둔다 | 가장 최솟값은 구하는 것(파란 점)
4. p와 q를 비교하여 더 작은 것을 가운데 값으로 가지게 범위를 재설정 => 현 상황에서는 q가 p보다 가까우니 left를 p의 지점으로 옮김  
이유 =>p와 q를 비교하여 더 작은 쪽이 어는 범위인지는 알았지만 p와q 사이의 공간은 아직 미지의 공간 so, 탐색할 범위안에 다시 넣어줌

저렇게 볼록함수일 경우에만 가능하다 물결치는 함수에서는 사용 불가능

시간 복잡도 : O(N * log 3/2 (수진선의 크기) ) => O(N * log (수직선의 크기) )

> 예시코드 백준 8986_전봇대

```cpp
// 현재 전봇대들의 위치를 정렬된상태로 x좌료로 주어짐
// 전봇대들 사이의 간격이 모두 일정하도록 전봇대들을 이동하는데 이동하는 총값(합)의 최솟값을 구하는 것

// 파라메트릭 탐색 => 수직선상을 먼저 만듬
// 두 전봇대 사이의 거리를 수직선상으로 잡음


int N;
int max_diff = 0;
int arr[100001];

long long move_sum( int move ) {
    long long sum_diff = 0;

    for (long long i = 1; i < N; i++)
        sum_diff += abs(arr[i] - 1LL*move*i );

    return sum_diff;
}


int main(void) {

    ios_base :: sync_with_stdio(0);
    cin.tie(0);

    long long ans = 1e18;

    cin >> N;
    cin >> arr[0];

    for (int i = 1; i < N; i++) {
        cin >> arr[i];
        // 두 전봇대 사이의 거리 중 가장 떨어진 값을 구함 => 수직선상의 오른쪽 끝
        // 간격이 일정하도록 만드는것 => 최대 나올 수 있는 간격은 위의 값
        max_diff = max( max_diff, arr[i] - arr[i-1] );
    }

    
    //수직선의 왼쪽 끝
    int left = 0;

    //수직산의 오른쪽 끝
    int right = max_diff;


    //삼분 탐색에서 수직선 양 끝 사이에 최소 p,q 즉, 2개의 점이 나올 수 있는 공간이 있어야 됨
    //so, 양 끝을 뺏을 대 3이상이 나와야함
    while (right - left >= 3) {

        // 1/3지점
        int p = (left*2 + right)/3;

        // 2/3지점
        int q = (left + right*2)/3;

        // 각 지점을 기준으로 무엇이 더 최소인지 구해봄
        // 최소인 점 기준으로 번위를 줄임 원래 범위의 2/3화
        if ( move_sum(p) > move_sum(q) )    left = p;
        else    right = q;
    }

    // 줄일대로 줄은(범위 안에 값이 3개 이하) 범위 안에서 최소값을 구해줌
    // 최소값은 이 범위안에 항상 당연 존재
    for (int i = left; i <= right; i++) {
        ans = min(ans,move_sum(i));
    }

    cout << ans;

    return 0;
}
```







