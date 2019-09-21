---
layout:     post
title:      leetcode 400 数组 篇
subtitle:   
date:       2019-09-21
author:     953max
header-img: 
catalog: 	 true
tags:
    - leetcode
    - 数组
---
# 从Array中remove系列
### 27 Remove Element
```java
class Solution {
    public int removeElement(int[] nums, int val) {      
        int count = 0;
        int index = 0;
        int tmp = 0;
        for(int i = 0;i<nums.length;i++){            
            if(nums[i] == val){                
                count++;
            }
            else{
                tmp = nums[index];//swap
                nums[index] = nums[i];
                nums[i] = tmp;
                index++;
            }
        }
        return nums.length - count;        
    }
}
```
给定一个数组，删除给定的元素并返回一个没有该元素的数字，但是要你返回这个数组的长度len，也就是说[0,..len]中间不能有val元素
更新两个指针，一个遇到val就加1，一个遇到非val，swap然后加1.


### 26 Remove Duplicates from Sorted Array
```java
class Solution {
    public int removeDuplicates(int[] nums) {
        //array duplicate系列
        //two pointer
        //如果i > i-1 i,j一起出发，如果i == i-1 i前进，j不动，等待下一个赋值
        //
        int j = 0;
        for(int i = 0;i < nums.length;i++){
            if( i < 1 || nums[i] > nums[i-1]){
                nums[j] = nums[i];
                j++;
            }
            else{

                continue;

            }
        }
        return j;
    }
}
```
和27差不多，一样是要求删除元素，不过这里是重复元素，然后让我们返回删除后元素的长度，一样是更新两个指针，从i = 1开始，如若nums[i-1] == nums[i],j不动，否则i，j一起前进。


### 80 Remove Duplicates from Sorted Array II
```java
class Solution {
    public int removeDuplicates(int[] nums) {  
        //array
        //two pointer
        if(nums.length <= 2) return nums.length;
        int j = 0,count = 0;
        for(int i = 0;i<nums.length;i++){
           if(nums[i] == nums[j]){  
                count++;  
            }else{
              if(count >= 2) {
                  for(int k=0;k<2;k++)
                  {
                      nums[j++] = nums[i-1];
                  }
                  nums[j] = nums[i];
              }
              if(count < 2) {
                  nums[j++] = nums[i-1];
                  nums[j] = nums[i];
              }
              count = 1;
            }
           }
        for(int n = j; n<nums.length;n++) nums[n] =nums[j];//corner case，由于更新nums[j]的条件是nums[i] != nums[j]所以最后一个元素没有更新到
        return (count>2)?j+2:j+count;//corner case 再更新len加上判断count个数
    }
}
```
第26题的变种，让重复次数最多可以两次，也是更新两个指针，i，j，记录i == j的个数，如果个数大于2，j就更新两个，小于等于2就更新个数个，然后再把j更新到i的值再进行判断；
注意corner case

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
```



41

First Missing Positive



299

Bulls and Cows



134

Gas Station



118

Pascal's Triangle

很少考

119

Pascal's Triangle II

很少考

169

Majority Element

很少考

229

Majority Element II

很少考

274

H-Index



275

H-Index II

Binary Search

243

Shortest Word Distance



244

Shortest Word Distance II



245

Shortest Word Distance III



217

Contains Duplicate



219

Contains Duplicate II

很少考

220

Contains Duplicate III

很少考

55

Jump Game



45

Jump Game II



121

Best Time to Buy and Sell Stock



122

Best Time to Buy and Sell Stock II



123

Best Time to Buy and Sell Stock III



188

Best Time to Buy and Sell Stock IV



309

Best Time to Buy and Sell Stock with Cooldown



11

Container With Most Water



42

Trapping Rain Water



334

Increasing Triplet Subsequence



128

Longest Consecutive Sequence



164

Maximum Gap

Bucket

287

Find the Duplicate Number



135

Candy

很少考

330

Patching Array

很少考







提高





4

Median of Two Sorted Arrays



321

Create Maximum Number

很少考

327

Count of Range Sum

很少考

289

Game of Life









Interval





57

Insert Interval



56

Merge Intervals



252

Meeting Rooms



253

Meeting Rooms II



352

Data Stream as Disjoint Intervals

TreeMap







Counter





239

Sliding Window Maximum



295

Find Median from Data Stream



53

Maximum Subarray



325

Maximum Size Subarray Sum Equals k



209

Minimum Size Subarray Sum



238

Product of Array Except Self



152

Maximum Product Subarray



228

Summary Ranges



163

Missing Ranges









Sort





88

Merge Sorted Array



75

Sort Colors



283

Move Zeroes




6

Wiggle Subsequence



280

Wiggle Sort



324

Wiggle Sort II


