---

layout:     post
title:      leetcode 400 数组 篇
subtitle:   Two Pass
date:       2019-09-21
author:     953max
header-img: 
catalog:      true
tags:
    - leetcode
    - 数组

---

## Two Pass

### 277 Find the Celebrity

```java
/* The knows API is defined in the parent class Relation.
      boolean knows(int a, int b); */

public class Solution extends Relation {
    public int findCelebrity(int n) {
        //array
        //matrix relation[][]      
        for(int i = 0;i< n;i++){
            for(int j = 0;j < n;j++){
                if(!knows(j,i) && j != i){
                    break;//如果一个人不被一个人认识，就淘汰
                }
                if(knows(i,j) && j != i ){
                    break;//然后再看这个人除了自己如果还认识别人就淘汰
                }
                if(j == n-1){
                    return i;//如果这个能迭代到最后，说明这个人是名人
                }
            }
        }
        return -1;
    }
}
```

这道题two pass是最优解，但是我还没有写出来，只写出了O(n^2)方法，基本思路就是先找到候选人，如果一个人不被一个人认识，就淘汰，然后再看这个人除了自己如果还认识别人就淘汰，如果这个能迭代到最后，说明这个人是名人；如果两个循环都没有找到名人，就返回-1.

### 189 Rotate Array

```java
 class Solution {
    public void rotate(int[] nums, int k) {
        //array
        //rotate
      k %= nums.length;
      reverse(nums, 0, nums.length - 1);
      reverse(nums, 0, k - 1);
      reverse(nums, k, nums.length - 1);

   }
   public void reverse(int[] nums, int start, int end) {
       while (start < end) {
           int temp = nums[start];
           nums[start] = nums[end];
           nums[end] = temp;
           start++;
           end--;
     }
   }
}
```

反转数组，O(1)空间的方法应该是如果是移一位将最后一位之前的数swap i和n-1-i然后在对整个数组swap。