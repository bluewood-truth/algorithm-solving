## Leetcode 2. Add Two Numbers [🔗](https://leetcode.com/problems/add-two-numbers/)

두 개의 음수가 아닌 정수를 나타내는 **non-empty** 연결 리스트가 주어진다. 숫자는 **역순으로 정렬**되어 저장되어 있다. 그리고 각 노드는 각 자리의 숫자를 나타낸다. 두 수를 더한 결과를 연결 리스트로 반환하라.

두 수는 해당 수가 0일 경우를 제외하고는 선행 제로(leading zero)는 없다고 가정한다.

#### Example

```
Input: l1 = [2,4,3], l2 = [5,6,4]
Output: [7,0,8]
Explanation: 342 + 465 = 807.
```

```
Input: l1 = [0], l2 = [0]
Output: [0]
```

```
Input: l1 = [9,9,9,9,9,9,9], l2 = [9,9,9,9]
Output: [8,9,9,9,0,0,0,1]
```

<br>

### 한 쪽 연결리스트에 합산

* 로직
  * 반환하기 위한 노드를 하나 선언해둔다.
  * l1이 None이 아닌 동안 다음을 수행한다.
    * l1의 값에 이전 자릿수에서 받아올림한 값을 더한다.
    * 만약 l2가 존재한다면:
      * l1의 값에 l2의 값을 더하고 l2는 다음 노드로 이동한다.
      * 만약 l2가 None이 아니고 l1의 다음 노드가 None이라면, 둘을 스왑한다.
        (l1을 기준으로 루프 중이므로 l1을 항상 더 긴 연결리스트로 유지하기 위함)
    * l1의 값을 10으로 나눈 몫을 받아올림 값으로, 나머지를 l1의 값으로 한다.
    * 만약 l1의 다음 노드가 None이고 받아올림 값이 존재한다면, l1의 다음 노드를 새로 생성하고 그 값을 받아올림 값으로 한다. 그 후 루프를 종료한다. 
    * l1의 다음 노드가 None이 아니라면 l1은 다음 노드로 이동한다.

* python

```python
class Solution:
    def addTwoNumbers(self, l1: ListNode, l2: ListNode) -> ListNode:
        added = l1
        upper = 0
        while l1:
            l1.val += upper
            if l2:
                l1.val += l2.val
                l2 = l2.next
                if l2 and not l1.next:
                    l1.next, l2 = l2, l1.next
            
            upper, l1.val = divmod(l1.val, 10)
            if not l1.next and upper:
                l1.next = ListNode(upper)
                break
            
            l1 = l1.next
        
        return added
```

<br>

### 새로운 연결 리스트에 합산

시간복잡도와 공간복잡도는 동일하지만 가독성이 훨씬 좋다.

* 로직
  * 합산할 새 연결리스트 노드를 선언한다.
  * l1, l2, 받아올림 값이 존재하는 동안 다음을 수행한다.
    * l1이 존재하면 l1의 값을 합산 노드 값에 더하고 l1은 다음 노드로 이동한다.
    * l2가 존재하면 l2의 값을 합산 노드 값에 더하고 l2는 다음 노드로 이동한다.
    * 합산 노드의 값을 10으로 나눈 몫을 받아올림 값으로, 나머지를 합산 노드의 값으로 한다.
    * (루프의 시작지점에서) 합산 노드의 다음 노드를 생성하고 그 값을 받아올림 값으로 한다. 그리고 합산 노드를 다음 노드로 이동한다.
      (루프의 시작지점에서 합산 노드의 다음 노드 생성과 이동이 이뤄지는 이유는, 루프의 끝에서 할 경우 합산 노드에 leading zero가 발생하기 때문이다.)
  * (루프동안 최소 한 번 다음 노드로 이동했으므로) 최초에 선언한 head 노드의 다음 노드를 반환한다.
  
* python

```python
class Solution:
    def addTwoNumbers(self, l1: ListNode, l2: ListNode) -> ListNode:
        added = ListNode(0)
        added_head = added
        
        upper = 0
        while l1 or l2 or upper:
            added.next = ListNode(upper)
            added = added.next
            
            if l1:
                added.val += l1.val
                l1 = l1.next
            if l2:
                added.val += l2.val
                l2 = l2.next
            
            upper, added.val = divmod(added.val, 10)
            
        return added_head.next
```

