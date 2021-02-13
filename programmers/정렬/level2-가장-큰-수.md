# 정렬 - 가장 큰 수 [🔗](https://programmers.co.kr/learn/courses/30/lessons/42746)

##### 문제 설명

0 또는 양의 정수가 주어졌을 때, 정수를 이어 붙여 만들 수 있는 가장 큰 수를 알아내 주세요.

예를 들어, 주어진 정수가 [6, 10, 2]라면 [6102, 6210, 1062, 1026, 2610, 2106]를 만들 수 있고, 이중 가장 큰 수는 6210입니다.

0 또는 양의 정수가 담긴 배열 numbers가 매개변수로 주어질 때, 순서를 재배치하여 만들 수 있는 가장 큰 수를 문자열로 바꾸어 return 하도록 solution 함수를 작성해주세요.

##### 제한 사항

- numbers의 길이는 1 이상 100,000 이하입니다.
- numbers의 원소는 0 이상 1,000 이하입니다.
- 정답이 너무 클 수 있으니 문자열로 바꾸어 return 합니다.

##### 입출력 예

| numbers           | return    |
| ----------------- | --------- |
| [6, 10, 2]        | "6210"    |
| [3, 30, 34, 5, 9] | "9534330" |

## 풀이

얼핏 쉬운 문제라고 생각했는데 별로 쉽지 않았다... 처음에는 정렬 함수의 key 인자를 이용해 비교하려 했는데, 아무리 생각을 해봐도 한쪽이 풀리면 한쪽이 막히는 식으로 답이 나오지 않았다.

그래서 단순한 key가 아니라 두 값을 비교하는 compare 인자의 필요성을 느꼈는데, 찾아보니 다행히도(어쩌면 당연하게도) 파이썬에도 이런 기능이 존재했다.

`functools`의 `cmp_to_key()`는 말 그대로 compare 함수를 정렬의 key 인자로 바꿔주는 함수다. 덕분에 문제가 수월하게 풀렸다. 나같은 경우에는 문자열로 변환한 두 값을 순서를 다르게 이어붙인 다음 비교하는 방식으로 compare 함수를 만들었다. 마지막으로 모든 숫자가 0일 경우에 대한 예외처리를 했다.

```python
import functools

def compare(x:str, y:str) -> bool:
    return 1 if x + y < y + x else -1

def solution(numbers):
    if sum(numbers) == 0:
        return "0"
    
    return "".join(sorted([str(n) for n in numbers], key=functools.cmp_to_key(compare)))
```

