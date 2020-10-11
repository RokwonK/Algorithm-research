# Sweeping Algorithm(스위핑 기법) / plane미완성

어떠한 선이나 공간을 한쪽에서 부터 싹 쓸어넘긴다는 의미  
스캔하면서 각 요소들에게 **어떠한 행위를 해주는 것**  
결국 정렬 후 for문으로 쓱 스캔하면서 행위를 정의해 주면 됨  
말이 쉽지 행위 정의하는게 무지하게 어렵다

- c++ (lower_bound, upper_bound 사용) => 이분탐색 기술임   
lower_bound(a) = a보다 작은 값 중에서 가장 큰 값 ex) [1,2,3,a,4,5,6] => 3의 위치를 반환
upper_bound(a) = a보다 큰 값 중에서 가장 작은 값 ex) [1,2,3,a,4,5,6] => 4의 위치를 반환


## 라인 스위핑(Line Sweeping Algorithm)

- 말 그대로 라인별로 나누어 쭉 스캔하는 알고리즘  
이차원 좌표평면으로 가정할 때, 라인을 x로 잡음 => x로 정렬  
정렬된 첫 인덱스부터 증가시키며 확인

ex) 가장 가까운 두 점의 거리를 구한다 하면  
처음 0과 1의 인덱스에 해당하는 두 점의 거리를 구함 (ans에 저장해두자)

1. 그러고 인덱스를 증가시키면서 해당 점보다 x값이 뒤에 있는 점들중에 ans보다 가깝게 있는 점이 있는지 확인하는 것이다.  
단순히 이런식이면 각 점마다 위에 있는 점들을 모두 확인하므로 시간 복잡도는 O(n^2)
=> 이제 쓸어넘기고 있으니 **어떠한 행위를 정의**해줄 차례다.
1. 먼저 x값을 기준으로 하고 있으니 (현재값과 다른 점의 x의 값)을 확인 ans보다 크다면 y값의 차는 볼 것도 없다(사진에서의 d가 현 ans)   
=> ans= sqrt(x값차^2 + y값차^2)인데 y값차^2을 해주기도 전에 x값차^2이 이미 ans를 넘어서니 볼 이유가 없음 
1. 이제 y값도 본다 y깂으로 정렬을 해두지 않았기때문에 y값은 위 아래로 ans보다 작은 지 확인해야한다  
=>사진에서 제대로 된 범위는 파란색 화살표를 각 변으로 하는 직사각형 안의 점들 중 가장 작은 값을 ans로 둔다 이 과정을 모든 점마다 반복한다.

<div><img src='https://t1.daumcdn.net/cfile/tistory/99BF7E395A7EB2FE25' style="width:300px;" ></div>

```cpp
//거리의 제곱 계산
int distance_cal(pair<int,int> i, pair<int,int> j) {
    return pow(i.first-j.first,2) + pow(i.second-j.second,2);
}

int main(void) {

    int n;
    int ans = 0;
    int x_limit = 0;

    cin >> n;

    vector< pair<int,int> > arr(n);
    map< pair<int, int>, bool > m;

    for (int i = 0; i < n; i++)
        cin >> arr[i].first >> arr[i].second;

    //x값으로 정렬
    sort( arr.begin(), arr.end() );

    //처음 두 점의 거리를 저장
    ans = distance_cal(arr[0], arr[1]);

    //y중심으로 map에 넣음 정렬 기준이 첫번재 인자이므로 띠로 정렬 기준 선언하는것보다 더 좋음
    m[{arr[0].second, arr[0].first}] = true;
    m[{arr[1].second, arr[1].first}] = true;

    //기준 점 lined으로 정의
    for (int line = 2; line < n; line++) {

        //x값 차로 1차적으로 거르기 => 
        while ( x_limit < line) {

            // (x값 차)^2이 ans 보다 작으면 가능성이 있으므로 (가정하는 것)
            // 뒤에 있는 값들은 당연 (x값 차)가 더 작으므로 그대로 break;
            // 즉, sqrt(ans) >= (x값 차)
            if (pow(arr[x_limit].first - arr[line].first,2) <= ans)
                break;

            // (y값 차)는 볼것도 없이 아웃시키기
            m.erase({arr[x_limit].second, arr[x_limit].first});

            //다음 확인하기 => x_limit을 고정값으로 ++해줘 시간을 줄임
            x_limit++;
        }

        //이제 (x값 차)는 가능성이 있는 것들로 map에구성됨
        //so, (y값 차)도 확인하기

        int y_limit = sqrt(ans); 

        // (y값 차) 기준으로 위아래 다 확인하기
        // y값도 ans를 넘어서면 쓸모없으므로 거르기
        // 이분탐색으로 끝 지점을 찾음
        auto down = m.lower_bound({arr[line].second - y_limit, -INF});
        auto up = m.upper_bound({arr[line].second + y_limit, INF});

        for (auto iter = down; iter != up; ++iter) {
            //key 값 가져오기
            pair<int,int> key = (*iter).first;
            ans = min( ans, distance_cal( arr[line], {key.second, key.first} ) );
        }
        
        // 다음 line을 위해 추가
        m[{arr[line].second, arr[line].first}] = 1;
    }
}
```


## 플레인 스위핑(Plane Sweeping Algorithm)

- 평면별로 나누어 쭉 스캔하는 알고리즘(평면 즉, 넓이 별로 나누는 것)

...