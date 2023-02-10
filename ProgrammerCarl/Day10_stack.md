# 20. Valid Parentheses
Due to the special nature of the stack structure, it is very suitable for solving symmetry matching problems e.g. bracket matching.
```PYTHON
def isValid(self, s: str) -> bool:
    # bracket: first-in/last-out so use stack

    stack = []

    for bracket in s:
        if bracket in "([{":
            stack.append(bracket)
        elif bracket == ")":
            if not stack or stack.pop() != "(":
                return False
        elif bracket == "]":
            if not stack or stack.pop() != "[":
                return False
        else:
            if not stack or stack.pop() != "{":
                return False

    return True if not stack else False
```
Besides, we can return False directly if the length of input `s` is an odd number as a valid parenthesis must be in pair.
```PYTHON
if len(s) % 2 != 0:
    return False
```

# 1047. Remove All Adjacent Duplicates In String
```PYTHON
    def removeDuplicates(self, s: str) -> str:
        # last-in/first-out so use stack
        stack = []
        for i in s:
            if stack:
                last = stack.pop()
                if last == i:
                    continue
                else:
                    stack.append(last)
                    
            stack.append(i)

        return "".join(stack)
```
# 150. Evaluate Reverse Polish Notation

```PYTHON
def evalRPN(self, tokens: List[str]) -> int:
    # last-in/first-out so use stack
    stack = []
    for i in tokens:
        if i not in  "+-*/":
            stack.append(int(i))
        else:
            val2 = stack.pop()
            val1 = stack.pop()

            if i == "+":
                stack.append(val1 + val2)
            elif i == "-":
                stack.append(val1 - val2)
            elif i == "*":
                stack.append(val1 * val2)
            else:
                stack.append(int(val1 / val2))
            
    return stack.pop()
```

