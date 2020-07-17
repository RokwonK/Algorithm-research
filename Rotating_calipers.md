# Rotating_calipers

물건의 길이를 재는 물건 캘리퍼스, 
그 것을 돌려가면서(회전하면서) 가장 먼 점을 찾을 때 사용한다고 로테이팅 캘리퍼스라고 부른다.

가장 먼 두 점을 구할 때 사용하기 좋음
- Convex_Hull을 사용하여 볼록 다각형을 이루는 정점을 구한다
- 가장 먼 두 점은 당연(최외각의 두 점일테니) 볼록 다격형을 이루는 점을 사이에 있을 것이다.
- 그 정점들을 한 번 확인하면서 가장 먼 점을 구함
- 두선분 정점 i와 i+1 / 정점 j와 j+1로 이루어진 선분을 기준으로
- i와 j를 원점으로 끌어당겨 i+1과 j+1의 정점 위치를 구한다.
- 원점과 저 두 정점의 ccw를 구한다
- 반시계방향을 이루고 있다면 j를 움직이면서 거리를 구하고, 시계방향이라면 i를 움직여서 구한다
    - 가장 먼 두 점은 볼록 다격형이기 때문에 방향이 반대인 두 정점의 거리일 것이다.(서로 마주보는 선분 일 것이 때문에)

## Code 

```cpp
#include <iostream>
#include <cstring>
#include <algorithm>
#include <stack>
#include <vector>


using namespace std;

struct info{
    int x,y;
    int px,py;

    info(int a, int b):x(a),y(b),px(1),py(0) {}
};

vector<info> arr;

bool compare(const info& a, const info& b) {
    if ( 1LL*a.py*b.px != 1LL*a.px*b.py) return 1LL*a.py*b.px < 1LL*a.px*b.py;
    if (a.y != b.y) return a.y < b.y;
    return a.x < b.x;
}

long long ccw(const info& p1, const info& p2, const info& p3) {
    return 1LL*(p2.x-p1.x)*(p3.y-p1.y)-1LL*(p2.y-p1.y)*(p3.x-p1.x);
}

long long dist(const info& a, const info& b) {
    return 1LL*(a.x-b.x)*(a.x-b.x) + 1LL*(a.y-b.y)*(a.y-b.y);
}

bool check(const info& a, const info& b, const info& c, const info& d) {
    info next = info(b.x-a.x, b.y-a.y);
    info next_next = info(d.x-c.x, d.y-c.y);
    return ccw(info(0,0), next, next_next) >= 0;
}

int main(void) {
    ios_base :: sync_with_stdio(0);
    cin.tie(0);
    
    int T;
    cin >> T;
    while (T--) {
        arr.clear();

        int C;
        long long max_dist = 0.0;
        pair<int,int> start = {0,0};
        pair<int,int> end = {0,0};

        cin >> C;
        for (int i = 0; i < C; i++) {
            int a,b;
            cin >> a >> b;
            arr.push_back(info(a,b));
        }

        sort(arr.begin(), arr.end(), compare);
        for (int i = 1; i < arr.size(); i++) {
            arr[i].px = arr[i].x - arr[0].x;
            arr[i].py = arr[i].y - arr[0].y;
        }
        sort(arr.begin()+1, arr.end(), compare);

        stack<int> s;
        s.push(0);
        s.push(1);

        for (int i =2; i < arr.size(); i++) {
            while(s.size() >= 2) {
                int second = s.top();
                s.pop();
                int first = s.top();
                if (ccw(arr[first], arr[second], arr[i] ) > 0) {
                    s.push(second);
                    break;
                }
            }
            s.push(i);
        }

        vector<int> v(s.size());
        for (int i = v.size()-1; i >= 0; i--) {
            v[i] = s.top();
            s.pop();
        }
        // -----------볼록 다각형을 이루는 정점들 구하기-----------//


        int p = 0;
        for (int i = 0; i < v.size(); i++) {

            // i와 p의 ccw 구하기 서로 방향이 반대면 ccw는 시계방향으로 돌것임
            while(p+1 < v.size() && check(arr[v[i]],arr[v[i+1]],arr[v[p]],arr[v[p+1]]) ) {
                long long part = dist(arr[v[i]], arr[v[p]] );
                if (max_dist < part) {
                    max_dist = part;
                    start = {arr[v[i]].x, arr[v[i]].y};
                    end = {arr[v[p]].x, arr[v[p]].y };
                }
                p++;
            }
            long long part = dist(arr[v[i]], arr[v[p]] );
            if (max_dist < part) {
                max_dist = part;
                start = {arr[v[i]].x, arr[v[i]].y};
                end = {arr[v[p]].x, arr[v[p]].y };
            }
        }
        
        cout << start.first << ' ' << start.second << ' ' << end.first << ' ' << end.second << '\n';
    }

    return 0;
}
```