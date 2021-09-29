# 후위 표기식



- problem number: 1918
- link: https://www.acmicpc.net/problem/1918  

---

## Define input, output
- Input:
- Output: 

## 설명


## source code 
```python
import sys


cals = list(sys.stdin.readline().rstrip())
stack = [""]
answer = ""
for c in cals:
    if c in ["*", "/"]:
        while stack and stack[-1] in ["*", "/"]:
            answer += stack.pop()
        stack.append(c)
    elif c in ["+", "-"]:
        while stack and stack[-1] != "(":
            answer += stack.pop()
        stack.append(c)
    elif c == ")":
        while stack and stack[-1] != "(":
            answer += stack.pop()
        _ = stack.pop()
    elif c == "(":
        stack.append(c)
    else:
        answer += c

while stack:
    answer += stack.pop()
 
print(answer)
```

