# 정렬 - H-Index [🔗](https://programmers.co.kr/learn/courses/30/lessons/42747)

###### 문제 설명

H-Index는 과학자의 생산성과 영향력을 나타내는 지표입니다. 어느 과학자의 H-Index를 나타내는 값인 h를 구하려고 합니다. 위키백과[1](https://programmers.co.kr/learn/courses/30/lessons/42747#fn1)에 따르면, H-Index는 다음과 같이 구합니다.

어떤 과학자가 발표한 논문 `n`편 중, `h`번 이상 인용된 논문이 `h`편 이상이고 나머지 논문이 h번 이하 인용되었다면 `h`의 최댓값이 이 과학자의 H-Index입니다.

어떤 과학자가 발표한 논문의 인용 횟수를 담은 배열 citations가 매개변수로 주어질 때, 이 과학자의 H-Index를 return 하도록 solution 함수를 작성해주세요.

##### 제한사항

- 과학자가 발표한 논문의 수는 1편 이상 1,000편 이하입니다.
- 논문별 인용 횟수는 0회 이상 10,000회 이하입니다.

##### 입출력 예

| citations       | return |
| --------------- | ------ |
| [3, 0, 6, 1, 5] | 3      |

##### 입출력 예 설명

이 과학자가 발표한 논문의 수는 5편이고, 그중 3편의 논문은 3회 이상 인용되었습니다. 그리고 나머지 2편의 논문은 3회 이하 인용되었기 때문에 이 과학자의 H-Index는 3입니다.

## 풀이

H-Index 개념이 자꾸 헷갈리고 이해하기 힘들어서 조금 단순무식하게 풀었다.

1. 인용 횟수에 대한 카운터 객체를 생성한다
2. 인용횟수의 최대값부터 0까지 순회한다
3. `i`번 인용한 논문이 존재하면 총 인용된 논문 편수에 더한다
4. `i`번 이상 인용된 논문이 총 인용된 논문 편수 이상이면 `i`를 `h`값으로서 return한다

```python
import collections

def solution(citations):
    counter = collections.Counter(citations)
    citation_max = max(citations)
    
    total_count = 0
    for i in range(citation_max, -1, -1):
        if i in counter:
            total_count += counter[i]
        if total_count >= i:
            return i
    
    return -1
```

## 풀이2

다른 분의 풀이. `enumerate()`를 이렇게 활용할수도 있구나 하는 생각이 들었다.

문제에서 주어진 `[3, 0 6, 1, 5]`를 예로 들면,

1. `citations`를 내림차순으로 정렬한다. `[6, 5, 3, 1, 0]`
2. 번호가 1부터 시작하는 enumerate 객체로 만든다. ` [(1, 6), (2, 5), (3, 3), (4, 1), (5, 0)]`
   - 각 튜플을 (n, h)라고 하면 n은 h번 이상 인용된 논문 편수를 의미한다.
3. 각 튜플 내에서 최소값을 구한다. 이것이 해당 인용횟수에 대한 `h`값이 된다.
4. `h`의 최대값을 return한다.

```python
def solution(citations):
    citations.sort(reverse=True)
    return max(map(min, enumerate(citations, start=1)))
```