# 스택/큐 - 주식가격 [🔗](https://programmers.co.kr/learn/courses/30/lessons/42584)

##### 문제 설명

초 단위로 기록된 주식가격이 담긴 배열 prices가 매개변수로 주어질 때, 가격이 떨어지지 않은 기간은 몇 초인지를 return 하도록 solution 함수를 완성하세요.

##### 제한사항

- prices의 각 가격은 1 이상 10,000 이하인 자연수입니다.
- prices의 길이는 2 이상 100,000 이하입니다.

##### 입출력 예

| prices          | return          |
| --------------- | --------------- |
| [1, 2, 3, 2, 3] | [4, 3, 1, 1, 0] |

## 풀이

주가가 내린 경우에 어떠한 계산이 이루어져야 하므로, 항상 주가가 오르는 경우를 기본으로 삼고 주가 배열의 크기 n에 대해 [n-1, n-2, ..., 0]을 `result`의 초기값으로 삼는다.

이후에는 스택을 이용해 풀이한다. 매번 스택에 해당 주가의 인덱스를 저장하고, 만약 스택의 맨 윗값의 주가보다 현재 주가가 낮다면, 그만큼 `pop()`하여 해당 인덱스와 현재 인덱스의 차이만큼을 주가가 떨어지지 않은 기간으로서 저장한다.

주가가 오를 때는 스택에 쌓기만 하고 주가가 내렸을 때는 현재 주가보다 큰 예전 주가에 대해 한꺼번에 계산을 하는 방식이다.

```python
def solution(prices):
    result = [ i for i in range(len(prices) - 1, -1, -1) ]
    stack = []
    for i, price in enumerate(prices):
        while stack and prices[stack[-1]] > price:
            prev = stack.pop()
            result[prev] = i - prev
            
        stack.append(i)
        
    return result
```

