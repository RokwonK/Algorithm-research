# Algorithm-research
내가 공부한 알고리즘(복습 및 원리 익히기)  
각 알고리즘마다 내가 부족한 부분들 정리해 놓음  
취약한 부분을 다시 보고 바로 깨달을 수 있게...

## 목차

- [SPFA](#SPFA)
- [Sweeping](#Sweeping)
- [Parameric](#Parameric)
- [Dynamic Programming](#Dynamic-Programming)
- [BitMask](#BitMask)
- [KMP](#KMP)
- [Two_Pointer](#Two_Pointer)
- [CCW](#CCW)

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

- 시간복잡도 => 가지각색
- 작은 문제부터 풀어 작은문제를 이용해 큰문제를 푸는 방식
- Top-down / Bottom-up 방식 => **Bottom-up 방식** 중요
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

## CCW
2020년 3월 26일 목요일

- 기하학 기술
- 두 벡터의 방향성을 이용하여 여러 기하학 풀이에 사용
- 어디로 꺽이는지 세 점이 어떤 위치에 있는지 알 수 있음
- [CounterClockWise.md](./CounterClockWise.md)

# 

## SSC
2020년 3월 31일 화요일

- 시간복잡도 : O(V + E)
- 스패닝 트리를 구한다음 사이클이있는 것들끼리 부분집합(SCC)을 만듬
- 코사리주 알고리즘과 타잔 알고리즘으로 구현 가능함
- [SSC.md](./SSC.md)
 