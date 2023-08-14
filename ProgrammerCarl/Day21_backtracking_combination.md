# 77. Combinations
We use `startIndex` to decide the next available range for backtracking.

<img width="918" alt="20230302122846" src="https://github.com/abc12345d/algorithm_practice/assets/44512722/d5469e4b-6137-406f-8c59-44a3cb2112f4">

### way 1: backtracking without prunning 

```PYTHON
def combine(self, n: int, k: int) -> List[List[int]]:
    def backtrack(startIndex, n, k, path):
        if len(path) == k:
            res_list.append(path[:])
            return

        for i in range(startIndex, n+1):
            path.append(i)
            backtrack(i + 1, n, k, path)
            path.pop()

    res_list = []
    backtrack(1, n, k, [])
    return res_list
```

### way 2: backtracking with prunning

<img width="765" alt="20230224175612" src="https://github.com/abc12345d/algorithm_practice/assets/44512722/82e510e1-2345-420a-9feb-dd72bc182fb1">

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
