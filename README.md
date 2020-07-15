# Algorithm-research
내가 공부한 알고리즘(복습 및 원리 익히기)  
각 알고리즘마다 내가 부족한 부분들 정리해 놓음  
취약한 부분을 다시 보고 바로 깨달을 수 있게...

명심! 흐른 시간은 돌아오지 않는다! 후회할 여지를 남기지 말자

## 목차

- [SPFA](#SPFA)
- [Sweeping](#Sweeping)
- [Parameric](#Parameric)
- [Dynamic Programming](#Dynamic-Programming)
- [BitMask](#BitMask)
- [KMP](#KMP)
- [Two_Pointer](#Two_Pointer)
- [SCC](#SCC)
- [LCA](#LCA)
- [Bitonic tour](#Bitonic_tour)
- [Trie](#Trie)
- [CCW](#CCW)
- [Convex_Hull](#Convex_Hull)
- [MCMF](#MCMF)

# 

## SPFA
2020년 2월 23일 일요일

- 시간복잡도 => 평균 : O(E+V) / 최악 : O(EV)
- 최단거리 알고리즘(벨만포드 알고리즘을 개선)
- 다익스트라 대신에 쓰여도 더 괜찮은듯? => 다익스트라가 더 빠름(if문 덕지덕지여서)
- [SPFA.md](./SPFA.md)

#

## Sweeping
2020년 2월 25일 화요일

- 시간복잡도 => O(NlogN) : 라인 스위핑
- 쓸어담듯이 스캔하면서 가능한 값들을 set이나 map에 저장해 부르트포스알고리즘의 시간을 개선
- [Sweeping.md](./Sweeping.md)

#

## Parameric
2020년 2월 28일 금요일

- 시간복잡도 => O(NlogN) : 파라메트릭 서치 => 탐색법 (이분탐색,삼분탐색)
- 범위를 가정 후 범위안에서 최적의 값을 구하는 알고리즘( 최소/최대값 )
- 기존 이분탐색과의 다른점 => 배열의 요소를 기준으로 하는 것이 아닌 수직선상의 값을 기준으로 가정 후 탐색
- [Parametric.md](./Parametric.md)

#

## Dynamic Programming
2020년 3월 2일 월요일  

*수정 - 20/07/03*

- 시간복잡도 => 가지각색
- Top-down / Bottom-up 방식
- 쉽게 보이면서도 활용하기에 가장 어려운 매력적인 알고리즘
- [Dynamic_Programming.md](./Dynamic_Programming.md)

#

## BitMask
2020년 3월 3일 화요일

- 시간, 메모리, 코드를 줄이기 위한 테크닉
- **잘 사용할 줄 안다면 매우 유용함**
- 각 비트(0,1)가 원소의 존재유무 (통상적으로 0이 false 없다는 뜻)
- [BitMask.md](./BitMask.md)

# 

## KMP
2020년 3월 18일 수요일

- 시간 복잡도 : O(N + M)
- 문자열 검색을 보다 효율적으로 하기위한 알고리즘
- 찾고자 하는 문자열이 문자열 안에 있는지를 빠르게 검색
- **실패함수**를 써서 시간을 효율을 높임(접두사와 중복되지 않는 지점부터 검색을 실시함)
- **찾기 즉, 탐색알고리즘이라고 생각**
- 환형으로 이어지는 배열에서 탐색시 좋음
- [String_Search.md](./String_Search.md)

# 

## Two_Pointer
2020년 3월 25일 수요일

- 시간복잡도 : O(N)
- 기본적 개념은 슬라이딩 윈도우와 같은 좌에서 우로 쓸어담으면 확인
- 구간(start~end)이 조건과 부합하는지를 찾는 것 / 확인하기 위해 때에따라 정렬도 필요
- [Two_Pointers.md](./Two_Pointers.md)

#  

## SCC
2020년 3월 31일 화요일 - 코사리주 알고리즘  
2020년 4월 1일 수요일 - 타잔 알고리즘

- 시간복잡도 : O(V + E)
- 스패닝 트리를 구한다음 사이클이있는 것들끼리 부분집합(SCC)을 만듬
- 코사리주 알고리즘과 타잔 알고리즘으로 구현 가능함
- 시간복잡도는 타잔 < 코사리주가 더 많이 듦
- 공간복잡도는 E의 수에 따라 달라짐
- SCC끼리는 사이클이 없으므로 DP, 2-SAT등에 사용할 수 있음
- [SCC.md](./SCC.md)
 
# 

## LCA
2020년 4월 5일 일요일

- 시간복잡도 : O(Nlog N)
- 트리 구조 안에서 두 정점의 최소 공통 조상(서로 처음 만나는 정점)을 구함
- 두 정점사이의 거리 등 트리안에서 두 정점을 이용하는 곳에 사용 하기 좋음
- [LCA.md](./LCA.md)

# 

## Bitonic tour
2020년 7월 6일 금요일

- DP를 이용한 시간 복잡도 : O(N^2)
- TSP(외판원 순회문제)에 일전 조건(순증가 순감소)을 건 방식
- 조건을 검으로써 다항식 시간내에 풀 수 잇음
- [Bitonic_tour.md](./Bitonic_tour.md)

# 

## Trie
2020년 7월 6일 월요일

- 시간 복잡도 : 찾기 -> O(M)
- 문자열을 찾는 알고리즘 중 하나 (ex. 제한된 사전 안에서 문자열 찾기)
- 메모리를 많이 사용하여 시간적 효율성을 가진다.
- [Trie.md](./Trie.md)

# 

## CCW
2020년 7월 8일 화요일

- 기하학 기술
- 두 벡터의 방향성을 이용하여 여러 기하학 풀이에 사용
- 방향 관계를 구할 수 있음
- [CounterClockWise.md](./CounterClockWise.md)

# 

## Convex_Hull
2020년 7월 9일 목요일

- 시간 복잡도 : O(NlogN)
- CCW를 이용하여 최외각 점들을 모아 볼록다각형을 만드는 기술
- [Convex_Hull](./Convex_Hull.md)

# 

## MCMF
2020년 7월 15일 수요일

- 시간 복잡도 : O( (V+E)*f )
- 최대유량이 흐를때 최소비용을 구하는 것
- 각 유량마다 최소비용을 구할때 SPFA를 사용해 시간적 효율성을 얻음
- [MCMF](./MCMF.md)


이후...  
- segment, 펜윅, lazy propagation
- 네트워크 플로우
- 이분 매칭
- 조합론 등 수학
- LCA 
- 고속 퓨리에 변환, 중국인의 나머지 정리, 모듈러 연산
- SCC, Biconnected Component
- 라빈카프
- 아호 코라식
- 병렬 이분 탐색
- 머지소트 트리
- 컨백스 헐 트릭
- 카라츠바 알고리즘
- 허프만 코딩
- 고속 역 제곱근 알고리즘
- 브레젠험 라인 알고리즘

