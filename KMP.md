### 代码
```java
class Solution {
    private int[] getNext(String p){
        int n = p.length();
        int j = 0;
        int[] p_next = new int[n];
        Arrays.fill(p_next, 0);
        for(int i = 1; i < n; i++){
            while(j > 0 && p.charAt(i) != p.charAt(j)) j = p_next[j-1];
            if(p.charAt(i) == p.charAt(j)) j += 1;
            p_next[i] = j;
        }
        return p_next;
    }
    public int strStr(String s, String p) {
        int[] p_next = getNext(p);
        int i = 0, j = 0;
        while(i < s.length() && j < p.length()){
            if(s.charAt(i) == p.charAt(j)){
                i ++;
                j ++;
            }else{
                if(j > 0){
                    j = p_next[j-1];
                }else{
                    i ++;
                }
            }
        }
        return j == p.length() ? i - p.length() : -1;
    }
}
```
