# Algorithm-research
내가 공부한 알고리즘(복습 및 원리 익히기)  
각 알고리즘마다 내가 부족한 부분들 정리해 놓음  
취약한 부분을 다시 보고 바로 깨달을 수 있게...

## SPFA
2020년 2월 23일 일요일

- 시간복잡도 => 평균 : O(E+V) / 최악 : O(EV)
- 최단거리 알고리즘(벨만포드 알고리즘을 개선)
- 다익스트라 대신에 쓰여도 더 빠름
- SPFA.md

#

## Sweeping
2020년 2월 25일 화요일

- 시간복잡도 => O(NlogN) : 라인 스위핑
- 쓸어담듯이 스캔하면서 가능한 값들을 set이나 map에 저장해 부르트포스알고리즘의 시간을 개선
- Sweeping.md

#

## Parameric
2020년 2월 28일 금요일

- 시간복잡도 => O(NlogN) : 파라메트릭 서치 => 탐색법 (이분탐색,삼분탐색)
- 범위를 가정 후 범위안에서 최적의 값을 구하는 알고리즘( 최소/최대값 )
- 기존 이분탐색과의 다른점 => 배열의 요소를 기준으로 하는 것이 아닌 수직선상의 값을 기준으로 가정 후 탐색
- Parametric.md

#

## Dynamic Programming
2020년 3월 2일 월요일

- 시간복잡도 => 가지각색
- 작은 문제부터 풀어 작은문제를 이용해 큰문제를 푸는 방식
- Top-down / Bottom-up 방식 => **Bottom-up 방식** 중요
- 쉽게 보이면서도 활용하기에 가장 어려운 매력적인 알고리즘
- Dynamic_Programming.md