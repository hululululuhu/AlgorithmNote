[483. 最小好进制](https://leetcode-cn.com/problems/smallest-good-base/)
### 思路分析
数据范围1e18，基本确定了这是一道二分。
关键点是如何二分，题目中提到很关键的一点是全为1，以最小的二进制，最大的数1e18计算，1的个数也不会超过64。
显然，64很小，所以一种思路就是对1的个数进行遍历，然后对多少进制进行二分加速。

- trick1：在判断的时候使用除法判断下一步操作是否会产生溢出。
### 代码
```java
class Solution {
    public int check(long x, long m, long num){
        long ans = 0;
        for(int i = 0; i <= m; i++){
            if(ans > (num - 1) / x ){
                return 1;
            }
            ans = ans*x + 1;
        }

        if(ans == num){
            return 0;
        }
        return ans - num > 0 ? 1 : -1;
    }
    public String smallestGoodBase(String n) {
        long num = Long.parseLong(n);
        for(int i = 53; i >= 1; i--){
            long l = 2, r = num;
            while(l < r){
                long mid = l + (r - l) /2;
                long tmp = check(mid, i, num);
                if(tmp == 0){
                    return String.valueOf(mid);
                }else if(tmp < 0){
                    l = mid + 1;
                }else if(tmp > 0){
                    r = mid ;
                }
            }
        }
        return String.valueOf(0);

    }
}
```
