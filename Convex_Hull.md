# Convex Hull

말 그대로 볼록껍질을 만드는 알고리즘

2차원 평면상 여러 개의 점이 있을 때  
일부(최외각 점)를 이용해 모든 점을 포함하는 **볼록**다각형을 만드는 것

## 그라함 스캔(Graham's scan) 알고리즘

무조건 외곽이 되는 점을 구하고 그 점을 기준으로 함
- 상하좌우 최 외곽에 있는 점 중 하나를 기준으로 함
- 기준이 되는 점과 모든 점 사이의 각도를 구하고 그 각도로 정렬 함
    - 즉, 기준이 된 점 보다 x좌표가 가장 큰거에서 -> 가장 작은거 순서대로 정렬됨


스택과 ccw를 이용하여, 볼록하게 만들 수 있는 점들을 스택에 쌓는다.

- 한 점 기준으로 우측에서 좌측으로 정렬하였기 때문에 
- 각 점을 순회하면서 볼록 껍질을 만듬
- 두 점을 이은 벡터(ab) 와 다음 점 c의 ccw를 구함
    - 반시계방향이라면 제대로 흐르고 있다는 것(정점을 돌아서 원래 기준으로 돌아와야하기 때문) 스택에 푸쉬
    - 시계방향이라면 거꾸로 흐른다는 뜻 => 오목하게 들어갔다는 뜻 
- 스택에 들어가 있는 정점이 다각형을 이루는 점

## Code -> 볼록 다각형을 이루는 점의 좌표들 구하기
```cpp

#include <iostream>
#include <algorithm>
#include <stack>
#include <vector>

using namespace std;


struct info{
    int x, y;
    int px, py;

    //생성자
    info(int a, int b) : x(a),y(b),px(1),py(0) {}
};

vector<info> arr;

bool compare(const info& a, const info& b) {
    // 벡터의 외적과 같음 (y2-y1)*(x3-x1) < (x2-x1)*(y3-y1)
    //                 == (x2-x1)*(y3-y1) - (y2-y1)*(x3-x1)
    // 앞의 항이 커야 반시계 방향임 => 반시계 방향이면 a가 b보다 각도가 작다는 것
    if ( 1LL*a.px*b.py != 1LL*a.py*b.px )
        return 1LL*a.py*b.px < 1LL*a.px*b.py;

    if (a.y != b.y) return a.y < b.y;
    return a.x < b.x;
}

long long ccw(const info& p1, const info& p2, const info& p3) {
    // 벡터의 외적 구하기
    return 1LL*(p2.x-p1.x)*(p3.y-p1.y) - 1LL*(p2.y-p1.y)*(p3.x-p1.x);
}

int main(void) {
    ios_base::sync_with_stdio(0);
    cin.tie(0);

    int N;
    cin >> N;

    for (int i = 0; i < N; i++) {
        int a,b;
        cin >> a >> b;
        arr.push_back(info(a,b));
    }
    // y축 기준으로 같다면 x축 기준으로 가장 작은 것을 구함
    sort(arr.begin(), arr.end(), compare);

    // 0번째 인덱스(기준점)으로 각도(벡터의 외적)를 구하기 위해서 전처리
    for (int i = 1; i < arr.size(); i++) {
        arr[i].px = arr[i].x - arr[0].x;
        arr[i].py = arr[i].y - arr[0].y;
    }

    // 기준점과 각도가 작은 것부터 정렬
    sort(arr.begin()+1, arr.end(), compare);


    // 점의 인덱스를 넣음
    stack<int> s;
    s.push(0);
    s.push(1);

    for (int i = 2; i < arr.size(); i++) {
        while(s.size() >= 2) {
            int second = s.top();
            s.pop();
            int first = s.top();

            if (ccw(arr[first], arr[second], arr[i]) > 0) {
                s.push(second);
                break;
            }
        }
        s.push(i);
    } 

    cout << s.size() << '\n';

    return 0;
}

```