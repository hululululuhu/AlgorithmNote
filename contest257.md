T1:[统计特殊四元组](https://leetcode-cn.com/problems/count-special-quadruplets/)
### 思路
暴力能过50*50*50*50 ≈ 10^7
```python
class Solution:
    def countQuadruplets(self, nums: List[int]) -> int:
        
        n = len(nums)
        ans = 0
        for i in range(n):
            for j in range(i + 1, n):
                for k in range(j + 1, n):
                    tmp = nums[i] + nums[j] + nums[k]
                    for p in range(n - 1, k, -1):
                        if tmp == nums[p]:
                            ans += 1
        return ans
```            
T2：[5864. 游戏中弱角色的数量](https://leetcode-cn.com/problems/the-number-of-weak-characters-in-the-game/)
### 思路
首先肯定是要进行排序的，这里我是从小到大，弱角色的条件时后面的数据中，二维有比他大的，使用一个sortedlist进行维护

### 代码
```python3
from sortedcontainers import SortedList
class Solution:
    def numberOfWeakCharacters(self, nums: List[List[int]]) -> int:
        nums.sort(key=lambda x:(x[0], x[1]))
        a = SortedList()
        for x, y in nums:
            a.add(y)
        n = len(nums)
        i = 0
        ans = 0
        while i < n:
            t = nums[i][0]
            j = i
            while j < n and nums[j][0] == t:
                a.remove(nums[j][1])
                j += 1
            j = i
            while j < n and nums[j][0] == t:
                if a and a[-1] > nums[j][1]:
                    ans += 1
                j += 1
            i = j
        return ans
```
T3:[访问完所有房间的第一天](https://leetcode-cn.com/problems/first-day-where-you-have-been-in-all-the-rooms/)
### 思路
dp[n]表示走到i需要的天数

因为只会往回走，那么相当于，把前面的路走了两次
也就是f[i] = 2*f[i-1]
又因为是从nums[i-1]处开始往回走，所以f[i] = 2*f[i-1] - f[nums[i-1]]
最后，i处也要重复两次
### 代码
```python3
class Solution:
    def firstDayBeenInAllRooms(self, nums: List[int]) -> int:
        n = len(nums)
        f = [0]*n
        f[0] = 1
        for i in range(n-1):
            n = len(nums)
        f = [0]*n
        for i in range(1, n):
            f[i] = 2*f[i-1] - f[nums[i-1]] + 2
        return f[-1] % 1000000007
```
T4：[数组的最大公因数排序](https://leetcode-cn.com/problems/gcd-sort-of-an-array/)
### 解题思路
step1： 将所有具有公共因子的数进行合并，
step2： 连通分量按照索引进行分组
step3： 按照索引进行排序，并还原到对应位置
step4： 和原数组排序后进行比较是否相同


### 代码

```python3
class UF:
    def __init__(self, n: int):
        self.pre = list(range(n))
        self.sz = [1]*n
        self.count = n
    
    def Find(self, x: int) -> int:
        if self.pre[x] != x:
            self.pre[x] = self.Find(self.pre[x])
        return self.pre[x]
    
    def Union(self, x: int, y: int) -> bool:
        root_x = self.Find(x)
        root_y = self.Find(y)
        if root_x == root_y:
            return False
        if self.sz[root_x] > self.sz[root_y]:
            root_x, root_y = root_y, root_x
        self.pre[root_x] = root_y
        self.sz[root_y] += self.sz[root_x]
        self.count -= 1
        return True

    def get_part_size(self, x: int) -> int:
        root_x = self.Find(x)
        return self.sz[root_x]


        
class Solution:
    def gcdSort(self, nums: List[int]) -> bool:
        n = max(nums)
        uf = UF(n + 1)
        for num in nums:
            for x in range(2, int(sqrt(num)) + 1):
                if num % x == 0:
                    uf.Union(num, num // x)
                    uf.Union(num, x)
        dic = defaultdict(list)
        for i, num in enumerate(nums):
            dic[uf.Find(num)].append(i)
        ans = [0]*len(nums)
        for k, v in dic.items():
            tmp = []
            for i in v:
                tmp.append([nums[i], i])
            tmp.sort()
            j = 0
            for num, i in tmp:
                ans[v[j]] = num
                j += 1
        return ans == sorted(nums)
```
