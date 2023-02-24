# 77. Combinations
### way 1: backtracking without prunning
![](./images/20230224175438.png)  
```PYTHON
def combine(self, n: int, k: int) -> List[List[int]]:
    def backtrack(start, n, k, path):
        if len(path) == k:
            res_list.append(path[:])
            return

        for i in range(start, n+1):
            path.append(i)
            backtrack(i + 1, n, k, path)
            path.pop()

    res_list = []
    backtrack(1, n, k, [])
    return res_list
```
### way 2: backtracking with prunning
![](./images/20230224175612.png)  
```PYTHON
def combine(self, n: int, k: int) -> List[List[int]]:
    def backtrack(start, n, k, path):
        if len(path) == k:
            res_list.append(path[:])
            return

        # n = 4, k = 2
        # last_start should be 3
        last_start = n - (k - len(path)) + 1
        for i in range(start, last_start + 1):
            
            path.append(i)
            backtrack(i + 1, n, k, path)
            path.pop()

    res_list = []
    backtrack(1, n, k, [])
    return res_list
```