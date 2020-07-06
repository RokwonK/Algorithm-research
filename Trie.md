# Trie

사전과 같이 미리 나올 문자열들이 주어지고 그 안에서 문자열을 찾고자 할 때 빠르게(찾는 문자열의 길이만큼의 시간) 찾을 수 있는 알고리즘.

1. 하나의 루트 노드를 만든다.
2. 집어 넣을 문자열의 첫 문자를 확인 후 그 문자로 가는 포인터가 있는지 확인
    - 없다면 새로운 노드를 만든다.
3. 다음 노드로 넘어감(첫 문자는 빼고 그 뒤의 문자열들만 가져감)
4. 2,3을 반복하다가 더 이상 만들 필요가 없다면(문자열에 해당하는 노드를 다 만들었다면) 그 노드가 끝이라는 표식을 하고 return

</br>

hi, hello, hell, good을 집어넣었을 때

*(e)는 끝이라는 표식
<pre>
                HEAD
            /           \
            h           g
          / |           |
         e  i(e)        o
         |              |
         l              o
         |              |
         l(e)           d(e)
         |
         o(e)

</pre>

</br>

## 시간 복잡도

- Trie
    - 만들때 시간복잡도 -> O(NM) : N개의 문자열, 각 문자열마다 길이 M
    - 안에서 문자열을 찾을때 -> O(M) : 찾는 문자열의 길이

- Trie가 아닐때
    - 그냥 문자열 배열 정렬 -> O(NM * logNM)
    - 배열안에서 문자열 찾기 -> O(MlogN)

</br>

## 코드
```cpp

struct trie{
    trie* child[26];
    bool end;

    trie() : end(false) {
        memset(child, 0, sizeof(child));
    }
    ~trie() {
        for (int i = 0; i < 26; i++) delete child[i];
    }

    // 문자열 삽입
    void insert(const string& s) {
        // 앞의 문자들을 타고 내려와서 더 갈 문자가 없으면 끝점이라는 표시
        if (s.size() == 0) end = true;
        // 타고 내려갈 문자의 위치에 생성(있으면 그냥 타고 내려감)
        else {
            int next = s[0]-'A';
            if (child[next] == 0) child[next] = new trie();
            child[next]->insert(string(s.begin()+1, s.end()));
        }
    }

    // 문자열 찾기
    bool search(const string& s) {
        // 찾는 문자열에 다 부합하고 그 곳이 끝점이라는 표시가 있다면 Okay
        if (s.size() == 0) {
            if (end) return true;
            else return false;
        } 
        int next = s[0]-'A';
        if (child[next] == 0) return false;
        else return child[next]->search(string(s.begin()+1, s.end()));
    }

};

```



