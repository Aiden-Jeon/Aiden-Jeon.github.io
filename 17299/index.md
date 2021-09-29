# 오등큰수



- problem number: 17299
- link: https://www.acmicpc.net/problem/17299  

---

## Define input, output
- Input:
- Output: 

## 설명


## source code 
```python
import sys
from collections import Counter

N = int(sys.stdin.readline())
array = list(map(int, sys.stdin.readline().strip().split(" ")))
cnt = Counter(array)
cnt_array = list(map(lambda x: (cnt[x], x), array))

stack = []
answer = [-1] * N
for i, (val, mapping) in enumerate(cnt_array):
    while stack and cnt_array[stack[-1][0]][0] < val:
        j, v, m = stack.pop()
        answer[j] = mapping
    stack.append([i, val, mapping])

print(*answer)
```

