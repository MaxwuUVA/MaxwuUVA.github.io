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
## 从Array中remove系列
### 27 Remove Element
```java
class Solution {
    public int removeElement(int[] nums, int val) {   
        //array
        //two pointer   
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
}
```
反转数组，O(1)空间的方法应该是如果是移一位将最后一位之前的数swap i和n-1-i然后在对整个数组swap。
## 猜猜需要什么元素
### 41 First Missing Positive
```java
class Solution {
    public int firstMissingPositive(int[] nums) {
        //array
        //设12345678为标准数组
        //i 在 nums[i-1]上 nums[i]在nums[nums[i]-1]上
        for(int i = 0;i < nums.length;i++){
            //trick
            while(nums[i] > 0 && nums[i] <= nums.length && nums[nums[i]-1] != nums[i]){
                //trick
                int tmp = nums[i];
                nums[i] = nums[nums[i]-1];
                //trick
                nums[tmp-1] = tmp;
            }
        }
        for(int j = 0;j < nums.length;j++){
            if(nums[j] != j+1){
                return j+1;
            }
        }
        return nums.length+1;
    }
}
```
找到第一个缺失的正整数，我们assume整个数组是[1,2,3,4,....n]把i放到i-1的位置，缺失的正整数就是n+1，找到一个数如果在[1..n]中间，交换nums[i]和nums[nums[i]-1],若一个数就在本来的位置，则不进入循环。



### 299 Bulls and Cows
```java
class Solution {
    public String getHint(String secret, String guess) {
        int bull = 0, cow = 0;
        Map<Integer,Integer> map = new HashMap<>();
        for(char c:secret.toCharArray()){
            map.put(c-'0',map.getOrDefault(c-'0', 0)+1);//设定map
        }
        for(int i = 0;i < secret.length();i++){
            if(secret.charAt(i) == guess.charAt(i)){
                 bull++;//两相同数位置相同
                 map.put(guess.charAt(i)-'0',map.get(guess.charAt(i)-'0')-1);//map中减掉
                 if(map.get(guess.charAt(i)-'0') < 0){
                     cow--;//如果所有位置都被bull占领，那么cow在前占的位置要替换给bull
                           //如 1 ，0 ， 1 ， 3和 1，1，1，2遍历到第三个时候为1bull1cow但是实际上后面这个1是bull
                 }
            }
            else{
                if(map.containsKey(guess.charAt(i)-'0') && map.get(guess.charAt(i)-'0') > 0){
                    cow++;//相同元素不同位置，cow++；
                    map.put(guess.charAt(i)-'0',map.get(guess.charAt(i)-'0')-1);
                }

            }
        }

        return bull+"A"+cow+"B";
        //O(n) 空间O(n)
    }
}
```
这道题我用就是用题意直接写出来的，可能还需要改进，bull和cow的替换需要注意。

## 数组也贪心，数组也动态
### 134 Gas Station
```java
class Solution {
    public int canCompleteCircuit(int[] gas, int[] cost) {
        //array
        //greedy
        //find candidate
        int[] diff = new int[gas.length];
        for(int i = 0; i < gas.length;i++){
            diff[i] = gas[i] - cost[i];
        }
        int sum = 0,total = 0,res = 0;
        for(int j = 0;j < gas.length;j++){
            sum += diff[j];
            total += diff[j];
            if(sum < 0){
                sum = 0;
                res = j+1;
            }
        }
        return total < 0 ? -1:res;       
    }
}
```
这道题有个前提就是如果存在一个解那么这个解是unique的，可以用greedy算法，怎么证明这个算法的正确性呢，用反证法来求，就是如果我们找到唯一个解，他不是第一个从gas的和>cost的和，那么他前面这个元素依然是解，那么这道题就有两个解，所以我们只需要找到，第一个起始点到终点gas和大于sum和的元素，那么就是如果gassum < cossum,标记sum = 0，res向前进，判断最后的和是否大于0返回res或-1；

## 118 Pascal's Triangle
```java
class Solution {
    public List<List<Integer>> generate(int numRows) {
        //动态规划    
        List<List<Integer>> res = new ArrayList<List<Integer>>();
        List<Integer> ele,pre = new ArrayList<Integer>();
        
        for(int i = 0;i<numRows;i++){    
            ele = new ArrayList<Integer>();
            for(int j = 0;j < i+1;j++){
                if(j == 0 || j == i)  ele.add(1);
                else ele.add(pre.get(j-1)+pre.get(j));
            }
            pre = ele;
            res.add(ele);
        }
        return res;
    }
}
```
和climbing stairs一样是动态规划的感受练习，可以通过题意很简单的写出递推公式dp[i][j] = dp[i-1][j-1]+dp[i-1][j]
首位末尾赋值1；

###119 Pascal's Triangle II
```java
public List<Integer> getRow(int rowIndex) {
        //dp
        List<Integer> res = new ArrayList<>();
        for(int i = 0; i <= rowIndex; i++) {
            res.add(1);
            for(int j = i-1; j > 0; j--) {
                res.set(j, res.get(j-1) + res.get(j));
            }
        }
        return res;
}
```
这次是一维的形式，从后向前避免污染i-1，递推公式是一样的dp[i] = dp[i]+dp[i-1];

## 门罗投票互相伤害
### 169 Majority Element
```java
class Solution {
    public int majorityElement(int[] nums) {       
        Map<Integer,Integer> map = new HashMap<>();
        for(int i=0;i<nums.length;i++){
            map.put(nums[i],map.getOrDefault(nums[i], 0)+1);
            if(map.get(nums[i]) > nums.length/2){
                return nums[i];
            }
        }
        return 1;
    }
}
```
用hash做法，时间复杂度很差，但是很清楚，就是找到大于长度1/2的那个数，

很少考

### 229 Majority Element II
```java
class Solution {
    public List<Integer> majorityElement(int[] nums) {

        //array
        List<Integer> res = new ArrayList<>();
        //moore vote
        //思想是进行两次遍历
        //最多出现两个元素
        //第一次遍历是找到出现次数最多的两次
        int a = 0, b= 0,count1 = 0,count2 = 0;
        for(int n:nums){
            if(a == n) count1++;
            else if(b == n) count2++;
            else if(count1 == 0){a = n;count1 = 1;}
            else if(count2 == 0){b = n;count2 = 1;}
            else{ count1--;count2--;}
        }
        //第二次遍历是找到两个元素出现的次数
        count1 = 0;count2 = 0;
        for(int n:nums){
            if(n == a) count1++;
            else if(n == b) count2++;
        }
        if(count1 > nums.length/3) res.add(a);
        if(count2 > nums.length/3) res.add(b);

        return res;
    }
}
```
实际上上一道题用门罗vote的方式会得到最优解但不是必须，这道题就需要用到门罗投票了，因为它要求用常数级别的空间，我们找到两个最多出现的元素，方法是设定两个count，如果是这两个元素就是累加，不是就减
如果count == 0时，替换该元素，然后再遍历数组得到两个数的个数。
很少考

### 274 H-Index
```java
class Solution {
    public int hIndex(int[] citations) {
        int n = citations.length;
        int[] bucket = new int[n + 1];
        for(int c : citations) {
            if(c >= n) {
                bucket[n]++;
            } else {
                bucket[c]++;
            }
        }
        int count = 0;
        for(int i = n; i >= 0; i--) {
            count += bucket[i];
            if(count >= i) {
                return i;
            }
        }
        return 0;
    }
}
```
这道题用sort非常直观，但实际上最优解是利用bucket完成，实际上这里能用bucket的原因也是因为他有实际上的upperbound也就是这个数组的总长度，也就是说，最多返回的也就是n，那么我们把数组中大于n的值都放在一个bucket里然后小于n的值c放在bucker[c]中间，然后从后往前遍历这个数组，count如果大于了index那么返回这个index就是h-index


### 275 H-Index II
```java
class Solution {
    public int hIndex(int[] citations) {
        //bs
        //target
        //二分搜索
        if(citations.length == 0) return 0;
        int start = 0, end = citations.length;//[0....end)
        while(start < end){
            int mid = start + (end-start)/2;
            if(citations[mid] == citations.length-mid){
                return citations.length-mid;
            }
            if(citations[mid] > citations.length-mid){
                end = mid;
            }
            else if(citations[mid] < citations.length-mid){
                start = mid+1;
            }
        }
        return citations.length-start;
    }
}
```
这回给定的是有序数组，求h-index，看能不能用logn时间找到，用二分查找，我们的target是第一个nums[i] >= len - i,如果在搜索过程中有相等数值，返回，没有则跳出后返回长度减指针
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


