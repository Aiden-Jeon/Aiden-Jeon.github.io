# 알파벳 찾기



- problem number: 10809
- link: https://www.acmicpc.net/problem/10809  

---

## Define input, output
- Input: 소문자로 이루어진 단어 S
- Output: 각 알파벳이 처음 나온 위치


## source code 
```python
import sys


S = sys.stdin.readline().strip()
result = [-1] * (ord("z") - ord("a") + 1)

for i, string in enumerate(S):
    if result[ord(string) - ord("a")] == -1:
        result[ord(string) - ord("a")] = i

print(*result)
```

