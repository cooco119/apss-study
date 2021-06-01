# 1장 - 문제 해결 시작하기

---

## 1. 문제 해결과 프로그래밍 대회

### 기록

- 예제로 나온 록 페스티벌 문제를 풀어보지 않았음 - [FESTIVAL](https://www.algospot.com/judge/problem/read/FESTIVAL)


### 권장 커리큘럼

- 한번 훑고 다시 지나가면서 공부하기

|\#|제목|
|-|-|
|2장|문제 해결 전략|
|3장|코딩과 디버깅|
|4장|알고리즘의 시간 복잡도 분석|
|6장|무식하게 풀기|
|7장|분할 정복|
|8장|동적 계획법|
|18장|선형 자료 구조|
|19장|큐, 스택, 데크|
|21장|트리의 구현과 순회|
|22장|이진 검색 트리|
|23장|우선순위 큐와 힙|
|27장|그래프의 표현과 정의|
|28장|그래프의 깊이 우선 탐색|
|29장|그래프의 너비 우선 탐색|
|30장|최단 경로 알고리즘|

*(첨언) CLRS는 짱짱 책이라고 한다. 학부떄는 안샀지만(...) 이 책 하고 나서 한번 쭉 해봐야곘다.*

---

## 2. 문제 해결 개관

### 문제 해결의 과정

> 1. 문제를 읽고 이해한다.
> 2. 문제를 익숙한 용어로 재정의한다.
> 3. 어떻게 해결할지 계획을 세운다.
> 4. **계획을 검증한다**
> 5. 프로그램으로 구현한다.
> 6. 어떻게 풀었는지 돌아보고, 개선할 방법이 있는지 찾아본다.

- 문제를 잘못 이해하는 실수를 하지 않는다
- *"어떤 부분을 추상화할 것인지를 선택하는 작업과 문제를 재정의하는 방법들에 대한 고찰은 좋은 프로그래머가되기 위해 필수적인 과정입니다"*
- 계획 검증을 지금까지 안해봤다. 알고리즘을 풀던 업무를 하던 우선 부딫히면서 휴리스틱하게 계속 답을 찾아나가는 방식으로 사고해왔던듯, 그걸 고쳐보자
- *"효과적으로 회고를 수행하는 가장 좋은 방법은 문제를 풀 때마다 코드와 함께 자신의 경험을 기록으로 남기는 것"* -> 이 repository의 목적

    - 문제의 간단한 해법
    - 어떤 방식으로 접근했는지
    - 문제의 해법을 찾는 데 결정적이었던 깨달음은 무엇?
    - 한번에 못맞춘 경우 오답 원인

- 다른 사람의 코드를 보는 것도 좋다.

    - 일정 시간 이상으로 안풀릴 경우는 다른 사람 것을 참고하되, **무조건 다시 복기** 해야 한다.

### 문제 해결 전략

- 직관으로 안될 때에는 체계적인 질문으로 접근해보자

#### 체계적인 접근을 위한 질문들

##### 비슷한 문제를 풀어본 적이 있던가?

- 단, 비슷한 문제라고 해서 모두 풀이법이 비슷하지도 않음

##### 단순한 방법에서 시작할 수 있을까?

- 시간과 공간 제약 없이 문제를 해결할 수 있는 가장 단순한 알고리즘 먼저
- 점진적 개선

    <details>

    <summary>예제</summary>

    > N(N <= 30)개의 사탕을 세 명의 어린이에게 가능한 고평하게 나눠주려고 한다. 공평함의 기준은 밭는 사탕의 총 무게가 가장 무거운 아이와 가장 가벼운 어린이간의 차이이다. 사탕의 무게는 모두 20 이하의 정수, 가능한 최소 차이는?

    - 1안 - 모든 방법을 만들어보자
    
        - 3^N개의 조합 수 
        - 중복이 엄청 많을 것! 중복을 줄여보자

    - 2안 - 사탕의 총량을 상태 공간으로 하는 BFS로 풀기

        - 사탕의 무게로 (0, 0, 0) 형식으로 표현
        - 실제 가진 사탕이 다르더라도 무게가 같은 경우 하나의 상태로 취급해 중복 제거

            - 아이 1이 사탕 A, B를 가지던, 사탕 C, D, E를 가지던 두 경우의 무게의 합이 같으면 같은 케이스로 취급

        - 사탕의 최대 무게는 20이므로, 각 아이의 사탕의 범위는 0 ~ 20
        - 상태 방문 여부 저장을 위해서는 (20*N + 1)^3 <= 601^3 (약 2억) 의 공간 필요

    - 3안 - 답이 최대 얼마인지를 예상해보기

        - 가장 많이 받은 아이와 적게 받은 아이의 차이가 20 이상이라 가정할 때, 각 사탕의 최대 무게는 20이므로 사탕을 가장 많이 받은 아이가 가장 적게 받은 아이에게 사탕을 하나 주면 항상 차이는 감소
        - 따라서 차이가 20 이상인 경우는 절대로 최적의 답이 될 수 없음 (그것 보다 적은 차이의 경우가 존재하므로)
        - 넉넉잡아 사탕을 가장 많이 받은 아이가 (20*N)/3 + 20 넘게 사탕을 받는 경우 무시

            - (아마도 사탕 최대 무게 * N개) / 3명 만큼 20을 넘는다 가정한듯
        
        - 이 경우 대략 ((20 * N)/3 + 20)^3 <= 220^3 (대략 1000만)으로 감소

    - 4안 - 문제를 푸는 더 빠른 방법, *누가* 얼마나 받는지는 중요하지 않음

        - 결국 중요한건 '차이'
        - 사탕 총량이 항상 오름차순으로 정렬되어 있는 케이스만 비교
        - 3개 숫자 조합은 3! = 6가지 이므로 전체 경우의 수는 1/6으로 감소 (대략 200만)

    </details>

- 연관 예제

    - [QUADTREE](https://www.algospot.com/judge/problem/read/QUADTREE)

##### 내가 푸는 과정을 수식화 할 수 있을까?

- 손으로 여러 간단한 입력 해보기

- 연관 예제

    - [QUANTIZE](https://www.algospot.com/judge/problem/read/QUANTIZE)
    - [NUMB3RS](https://www.algospot.com/judge/problem/read/NUMB3RS)
    - [RESTORE](https://www.algospot.com/judge/problem/read/RESTORE)
    - [MATCHORDER](https://www.algospot.com/judge/problem/read/MATCHORDER)
    - [POTION](https://www.algospot.com/judge/problem/read/POTION)
    - [TRAPCARD](https://www.algospot.com/judge/problem/read/TRAPCARD)

##### 문제를 단순화할 수 없을까?

- 쉬운 변형판을 먼저 풀어보기

    <details>

    <summary>예제</summary>
    
    >(그림 2.1) 2차원 격자 위에 N개의 점이 있다고 하자. 가로선 혹은 세로선을 따라서만 움직이며 두 점 사이 거리는 (x좌표 차이) + (y좌표 차이)이다. N개의 점이 있을 때 거리의 합이 최소가 되는 새 점의 위치를 찾아봅시다.


    - 단순한 1차원 형태로 풀어보기

        - 직선상의 점이 있을 때 거리의 합을 최소화하는 새 점을 찾기

    - 모든 점을 x축으로 project한 문제 1과 모든 점을 y축으로 project한 문제 2를 풀어서 각각 x, y 값을 알아낸다.

    </details>

- 연관 예제

    - [ASYMTILING](https://www.algospot.com/judge/problem/read/ASYMTILING)
    - [DRAGON](https://www.algospot.com/judge/problem/read/DRAGON)
    - [LUNCHBOX](https://www.algospot.com/judge/problem/read/LUNCHBOX)
    - [CHILDRENDAY](https://www.algospot.com/judge/problem/read/CHILDRENDAY)
    - [LAN](https://www.algospot.com/judge/problem/read/LAN)

##### 그림으로 그려보기

- 연관 예제

    - [STRJOIN](https://www.algospot.com/judge/problem/read/STRJOIN)
    - [NERDS](https://www.algospot.com/judge/problem/read/NERDS)
    - [NERDS2](https://www.algospot.com/judge/problem/read/NERDS2)

##### 수식으로 표현할 수 있을까?

- 연관 예제

    - [WITHDRAWAL](https://www.algospot.com/judge/problem/read/WITHDRAWAL)

##### 문제를 분해할 수 있을까?

- 다루기 쉬운 형태로 문제를 변형하기

    - 문제의 제약조건 분해하기 : 단순한 형태의 조건의 집합으로 분리

    <details>
    <summary>예제</summary>

    > 400m 달리기 경주에 N명의 선수들이 참가, 과거 기록 `best[]`와 `worst[]`가 있다. 각 선수는 지금까지의 기록 범위 내의 성적만을 낸다고 가정할 때, 다음 형태의 신문기사들이 M개 나옴
        1. 선수 i는 선수 j에게 반드시 패배함
        2. 선수 i와 선수 j는 서로 상대에게 이길 수 있음
    다음 기사들의 오류를 확인하고 싶다. 신문 기사들의 목록이 주어질 때, 이 기사들의 주장을 모두 만족시키는 기록의 집합이 존재하는 지 확인

    - (그림 2.2) 각 선수의 기록 범위 (best[i], worst[i])를 직선에 나타냄
    - 요구 조건을 수식으로 표현하고, 2번 조건을 1번 조건으로 변환
    
        - 1번 조건 : `worst[j] < best[i]`
        - 2번 조건을 1번 조건으로 변경하기

            - `!(worst[i]<=best[j])&&!(worst[j]<=best[i])`
                `= (best[j]<worst[i])&&(best[i]<worst[j])`

    - 2N개의 변소 `best[]`와 `worst[]`에 대해 위상 정렬을 수행해 해결 가능
    
    </details>

##### 뒤에서부터 생각해서 문제를 풀 수 있을까?

- 전형적 예시 : 사다리 게임

    - 어떤 칸을 찍어야 원하는 결과를 알 수 있는지!

- 연습 예제

    - [INSERTION](https://www.algospot.com/judge/problem/read/INSERTION)
    - [GALLERY](https://www.algospot.com/judge/problem/read/GALLERY)
    - [SORTGAME](https://www.algospot.com/judge/problem/read/SORTGAME)

##### 순서를 강제할 수 있을까?

- 순서가 없는 문제에 순서 강제하기

    <details>
    <summary>예제</summary>

    > (그림 2.3) 5x5 격자에서 모든 칸의 불을 켜야한다. 한 칸을 클릭하면 상하좌우 인접 칸의 상태가 동시에 변화한다. 클릭 수를 최소화해서 모든 칸에 있는 불을 켜보자.

    - 깨달아야 하는 두가지

        - 어떤 순서로 칸을 클릭하든 상관이 없다 : 각 칸의 상태는 자신과 인접한 칸이 몇 번 클릭되었는지만 중요
        - 한 칸을 두번 이상 클릭할 필요가 없다 : 같은 칸 두번 클릭은 한번도 클릭하지 않은 것과 같다.

    - 모든 테스트 : 2^25

    - 항상 특정 순서대로 칸을 눌러야 한다는 제약을 추가해보자

        - 항상 왼쪽 위에서부터
        - 맨 윗칸을 어떻게 누르느냐에 따라 아래 칸들이 정해짐
        - 2^5

    </details>
    
- 경우의 수를 셀 때에도 유용

    - 중복을 제거하기 위해 순서를 강제하면 좋음

- 연관 예제

    - [BOARDCOVER](https://www.algospot.com/judge/problem/read/BOARDCOVER)
    - [POLY](https://www.algospot.com/judge/problem/read/POLY)
    - [ZIMBABWE](https://www.algospot.com/judge/problem/read/ZIMBABWE)

##### 특정 형태의 답만을 고려할 수 있을까?

- 순서를 강제하기의 연장 : Canonicalization(정규화)

    - 고려할 답 중 형태가 다르지만 결과적으로는 같은 것을 그룹으로 묶고, 그룹의 대표만을 고려하기

        <details>

        <summary>예제</summary>

        > (그림 2.4) 원 A를 움직여 다른 점선 원들을 모두 덮을 수 있는지 판단해라

        - 가능한  A 중심의 좌표가 무한함

        - (그림 2.5) 답이 하나라도 존재하면 두 개 이상의 점선 원과 접한 답이 존재

        </details>

- 연관 예제

    - [PICNIC](https://www.algospot.com/judge/problem/read/PICNIC)
    - [WORDCHAIN](https://www.algospot.com/judge/problem/read/WORDCHAIN)