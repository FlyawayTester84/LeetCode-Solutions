42 [Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/)

## Monotonic Stack
Water can be trapped between two places `i` and `j` if and only if the heights at both `i` and `j` are greater than the heights between them. This motivates us to use a monotonic stack to keep track of the heights.

The initial thought is to push heights into the stack when the height is decreasing. Let's use `height = [4,2,0,0,1,3,5]` as an example. We first create the stack
 ```
 [ 4 | 2 | 0 | 0 ].
 ```
Next we will encounter `1`, which is greater than the currrent top element `0` in the stack. We pop the stack until we reach a height not less than `1`. At each time, we also add the difference between 1 and the top element on the stack to the final result. This is the amount of water that can be trapped at this place, if we ignore everything beyond current position. We now have the stack
 ```
 [ 4 | 2 ]
 ```
and a total amount of `2` units of water trapped. After we calculated total amount of water that is trapped with the addition of the new height, we fill in the pit with water in it. In other words, we push the minimum of two edges back into the stack. In this case, the left edge is of height `2` and the right `1`, so we push `1` into the stack. Also remember to push the current position into the stack.
 ```
 [ 4 | 2 | 1 | 1 | 1 ].
 ```
 The next position has height `3`, so we repeat the process to get the stack
  ```
 [ 4 | 3 | 3 | 3 | 3 | 3 ]
 ```
 and a total amount of `9` units of water trapped. Finally we have a `5`, which is a problem given our algorithm, since all elements in the stack are than 5. Thus we also need to keep track of the maximum element in the stack, and view `5` as virtually the maximum element when pushing it into the stack. The maximum element is like a wall which prevents the water from flowing to the left of it. The maximum element is `4`, so we have
 ```
 [ 4 | 4 | 4 | 4 | 4 | 4 | 5 ]
 ```
 and a total amount of `14` units of water trapped.
 
 ### Code (Python)
 ```python
 def trap(self, h):
    n = len(h)
    if n <= 2: return 0 # edge cases
    stack = [h[0]]
    out = 0
    curMax = h[0]
    for i in range(1,n):
        if stack[-1] < h[i]:
            count = 0
            new = min(h[i], curMax) # view the new element as the maximum element
            curMax = max(h[i], curMax) # update the maximum element
            while stack[-1] < new:
                out += new - stack.pop()
                count += 1
            for j in range(count): stack.append(new) # fill in the pits
        stack.append(h[i])
    return out
```
