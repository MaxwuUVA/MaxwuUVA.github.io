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
        //O(n)  
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
给定一个数组，删除给定的元素并返回一个没有该元素的数组，但是要你返回这个数组的长度len，也就是说[0,..len]中间不能有val元素
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

### 119 Pascal's Triangle II
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
# Binary Search
## 茫茫辞海我要找到你
### 243 Shortest Word Distance
```java
class Solution {
    public int shortestDistance(String[] words, String word1, String word2) {       
        //only need to keep index
        int t1 = -1,t2 = -1,min = words.length+1;
        for(int i = 0;i< words.length;i++){
            if(words[i].equals(word1)){
                t1 = i;
            }
            if(words[i].equals(word2)){
                t2 = i; 
            }
            if(t1 != -1 && t2 != -1){
                min = Math.min(min,Math.abs(t1-t2));
            }
        }
        return min;
    }
}
```
这道题的思路非常简单因为最短的距离必然出现在按序iterate的路径上，所以我们只需要跟踪两个单词所在的位置，这里用了一个trick就是为了避免index冲突，我们将两个index初始化均为-1。时间复杂度为O(n);


### 244 Shortest Word Distance II
```java
class WordDistance {
    
    private Map<String,List<Integer>> map = new HashMap<>();
    
    public WordDistance(String[] words) {
        
        for(int i = 0;i<words.length;i++){
            if(!map.containsKey(words[i])){
                List<Integer> list = new ArrayList<>();
                list.add(i);
                map.put(words[i],list);
                
            }
            else{
                map.get(words[i]).add(i);
            }
        }
        
        
    }
    
    public int shortest(String word1, String word2) {
        
        int min = Integer.MAX_VALUE;
        for( int i = 0,j = 0;i < map.get(word1).size() && j< map.get(word2).size();){
            int indexi = map.get(word1).get(i);
            int indexj = map.get(word2).get(j);
            min  = Math.min(min,Math.abs(indexi-indexj));
            if(indexi < indexj){
                i++;
            }
            else{
                j++;
            }
        }
       
        return min;
    }
}

/**
 * Your WordDistance object will be instantiated and called as such:
 * WordDistance obj = new WordDistance(words);
 * int param_1 = obj.shortest(word1,word2);
 */
```
这道题叫我们设计一个对象，实现243中的方法并且call repeatly time，所以最简单的优化就是用一个哈希表来存储所有的单词，用一个list保存所有出现过这个单词的位置，然后对这两个单词出现的所有位置求最小值；
时间复杂的为O(n+m)n,m为两个单词出现的次数，利用空间环取了每次遍历word array的时间


### 245 Shortest Word Distance III
```java
class Solution {
    public int shortestWordDistance(String[] words, String word1, String word2) {
        
        int t1 = -1,t2 = -1,min = words.length+1;
        for(int i = 0;i<words.length;i++){
            if(words[i].equals(word1)){
               
                    t1 = i;
                
                    
            }
            if(words[i].equals(word2)){
                if(word1.equals(word2)){
                    t1 = t2;
                }//swap
                    t2 = i;
            }
            if(t1 != -1 && t2 != -1){
                min = Math.min(min,Math.abs(t1-t2));
            }
        }
        return min;
        
    }
}
```
对第一题只改了一个条件，就是单词可以相同了，这里用到一个小trick就是当两个单词相同时，我们用指针找到两个单词时，对他们进行swap操作，保证两个单词不在一个位置。

## 查重
### 217 Contains Duplicate
```java
class Solution {
    public boolean containsDuplicate(int[] nums) {
        
        HashMap<Integer,Integer> map = new HashMap<>();
        
        for(int i = 0; i < nums.length;i++){
            
            if(map.containsKey(nums[i])){
                
                return true;
                
            }
            else map.put(nums[i],i);
            
        }
        
        return false;
        
    }
}
```
利用hashmap非常简单
### 219 Contains Duplicate II
```java
class Solution {
    public boolean containsNearbyDuplicate(int[] nums, int k) {
        
        Set<Integer> set = new HashSet<>();//set(k) find k+1 element in set
        for(int i = 0;i<nums.length;i++)
        {
            if(set.contains(nums[i])) return true;
            else set.add(nums[i]); 
            
            if(set.size() > k) set.remove(nums[i-k]);
            
        }
        return false;
    }
}
```
维护一个size为k的set，然后find这个set里面有没有duplicate

很少考

### 220 Contains Duplicate III
```java
class Solution {
    public boolean containsNearbyAlmostDuplicate(int[] nums, int k, int t) {
        
        /* need to control the set at the low - t to high + t range k*/
        
        TreeSet<Integer> set = new TreeSet<>();
        for(int i = 0;i<nums.length;i++){
            
            
            if( set.ceiling(nums[i]) != null && (long)set.ceiling(nums[i]) - (long) nums[i] <= t) return true; // exist v-t<i<v+t
            if( set.floor(nums[i]) != null && (long)nums[i] - (long) set.floor(nums[i]) <= t) return true;
            
            set.add(nums[i]);
            
            if(set.size() > k ) set.remove(nums[i-k]);//delete the i-k keep set size = k;
            
        }
        
        return false;
        
    }
}
```
利用了treeset的ceiling和flooring的性质，找到大于
很少考
### 跳跳跳
## 55 Jump Game
```java
class Solution {
    public boolean canJump(int[] nums) {
        //dp
        //维护一个长度与数组相同的dp数组
        //dp[i]存能到达的最远距离
        //如果前面的都不能到达i那么返回false
        int[] dp = new int[nums.length];
      
        dp[0] = nums[0];
      
        for(int i = 1;i < nums.length;i++){
            dp[i] = Math.max(i+nums[i],dp[i-1]);
            if(dp[i-1] < i){
                return false;
            }
        }
        return true;
    }
}
```
找出是否能跳到终点，子问题叠加就是是否能到每一个节点，那每一个子问题就是，判断前面节点是否能跳到下一个节点，而且解决这种子问题不影响最后的结果，我们可以用贪心算法的递推方式，把节点中能达到的最远距离保存，然后只需要判断前一个节点能否到达下一个节点即可。



### 45 Jump Game II
```java
class Solution {
    public int jump(int[] nums) {
        //bfs 遍历
        //图的最短距离
        if(nums.length == 0) return 0;
        Deque<Integer> queue = new LinkedList<>();
        int count = 0;
        int top = 0;
        queue.add(top);
        boolean[] visited = new boolean[nums.length];
        while(!queue.isEmpty()){
            top = queue.poll();
            if(top+nums[top] >= nums.length-1){
                    //corner case 已经在最后一个节点上就直接输出count
                    return top == nums.length-1?count:count+1;
             } 
            int tmp = top+1+nums[top+1];
            queue.add(top+1);
            for(int i = top+1;i <= top + nums[top];i++){
                
                if(visited[i] == false && i+nums[i] > tmp){
                    queue.poll();
                    queue.add(i);
                    tmp = i+nums[i];
                    visited[i] = true;
                }

            }
            count++;
        }
        return count;
    }
}
```
```java
class Solution {
    public int jump(int[] nums) {
        
        //if(last + nums[last] > i){ dp[i] = dp[last]+1}
        //和word break那题有异曲同工之妙
        int[] dp = new int[nums.length];
        dp[0] = 0;
        int last = 0;
        for(int i = 1;i < nums.length;i++){
           for(int j = last;j < i;j++){
               if(j+nums[j] >= i){
                 dp[i] = dp[j]+1;
                 last = j;
                 break;
               }
          }
        }
        return dp[nums.length-1];
    }
}
```
最直观的方法就是利用bfs求图的最短距离，当然这道题讨论最优解需要用贪心算法，先用动态规划想法很容易想出递推公式为dp[i] = dp[j]+1;其中j在[0,i)之间最先满足j+nums[j] > i，然后再想因为i是递增的所以满足的j实际上也是递增的，所以j的范围实际上是[上一个j，i）。这样我们就可以得到一个为O(n)时间复杂度的算法。


### 股票全家桶
## 121 Best Time to Buy and Sell Stock
```java
class Solution {
    public int maxProfit(int[] prices) {  
        if(prices == null || prices.length == 0){
               return 0;
        }
        //array
        //one pass
         int min = Integer.MAX_VALUE;
         int res = 0;
         for(int i = 0;i < prices.length;i++){
             //find min 
             min = Math.min(min,prices[i]);
             //find res (candidate = prices[i]-min)
             res = Math.max(res,prices[i]-min);
         }
         return res;
    }
}
```
股票只需买一次，这道题实际上可以说有贪心算法的思想，就是寻找当前元素前最小的元素，之后让后面的元素和这个元素相减，而后面的元素如果小于最小元素，则替换掉他，因为即使解出现在后面也与前面元素无关。


### 122 Best Time to Buy and Sell Stock II
```java
class Solution {
    public int maxProfit(int[] prices) {
        if(prices == null || prices.length == 0){
                return 0;
        }
        //greedy
        //证明假如存在一个n，m
        //1,n,m升序，且m-1大于分别greedy方式 m-n+n-1=m-1 和greedy方式相等
        //所以不存在这样的方式
        //所以最优解为找到一个比他大的元素就卖掉
        int res = 0,tmp = prices[0];
        for(int i = 0;i < prices.length;i++){
            if(prices[i] > tmp){
               res += prices[i] - tmp;
            }
            tmp = prices[i];
        }
        return res;
    }
}

```
这道题也很简单，也是用了贪心算法的思想one pass，只要后面的值比前面的大，就产生交易，如果比前面的小，就不买前一个，买后一个，所以只要每次把tmp替换成上一个元素，然后比较现在这个元素和前一个的差值就可以了。


### 123 Best Time to Buy and Sell Stock III
```java
class Solution {
    public int maxProfit(int[] prices) {

        if(prices.length == 0) return 0;

        //array
        //系列中最复杂的一道题
        //设定两个dp数组
        //第一个代表第i天交易j次得到的最大值
        //第二个代表全局在第i天交易j次得到的最大值
        int n = prices.length;
        int[][] global = new int[n][3];
        int[][] local = new int[n][3];
        //递推公式
        // diff = prices[i] - prices[i-1]
        //local[i][j] = max(global[i-1][j-1]+max(diff,0),local[i-1][j]+diff)
        //global[i][j] = max(local[i][j],global[i-1][j])
        for(int i = 1;i < n;i++){
            int diff = prices[i] - prices[i-1];
            for(int j = 1; j <= 2;j++){
                local[i][j] = Math.max(global[i-1][j-1]+Math.max(diff, 0), local[i-1][j]+diff);
                global[i][j] = Math.max(local[i][j], global[i-1][j]);
            }

        }
        return global[n-1][2];
    }
}
```
O(n)spaceO(n)
具体内容注释里面已经提到，实际上是典型的local，global型的动态规划类型题，取到当前值和全局对比，后面总结动态规划类型时会再总结一遍。local[i][j]=max(global[i-1][j-1]+max(diff,0),local[i-1][j]+diff)
可以这样来理解：
第 i 天卖第 j 支股票的话，一定是下面的一种：

1. 今天刚买的
那么 Local(i, j) = Global(i-1, j-1)
相当于啥都没干

2. 昨天买的
那么 Local(i, j) = Global(i-1, j-1) + diff
等于Global(i-1, j-1) 中的交易，加上今天干的那一票

3. 更早之前买的
那么 Local(i, j) = Local(i-1, j) + diff
昨天别卖了，留到今天卖


### 188 Best Time to Buy and Sell Stock IV
```java
class Solution {
    public int maxProfit(int k,int[] prices) {

        if(prices.length == 0) return 0;

        //array
        //系列中最复杂的一道题
        //设定两个dp数组
        //第一个代表第i天交易j次得到的最大值
        //第二个代表全局在第i天交易j次得到的最大值
        
        //递推公式
        // diff = prices[i] - prices[i-1]
        //local[i][j] = max(global[i-1][j-1]+max(diff,0),local[i-1][j]+diff)
        //global[i][j] = max(local[i][j],global[i-1][j])
        //用通用公式MLE，所以k > prices.length时退化为贪心算法
        if( k >= prices.length){
               return maxprofitgreedy(prices);
        }
        int n = prices.length;
        int[][] global = new int[n][k+1];
        int[][] local = new int[n][k+1];
        for(int i = 1;i < n;i++){
            int diff = prices[i] - prices[i-1];
            for(int j = 1; j <= k;j++){
                local[i][j] = Math.max(global[i-1][j-1]+Math.max(diff, 0), local[i-1][j]+diff);
                global[i][j] = Math.max(local[i][j], global[i-1][j]);
            }
        }
        return global[n-1][k];
    }
    private int maxprofitgreedy(int[] prices){
        int maxprofit = 0,tmp = prices[0];
        for(int i = 1;i < prices.length;i++){
            if(prices[i] - tmp < 0){
                tmp = prices[i];
            }
            else{
                maxprofit += prices[i] - tmp;
                tmp = prices[i];
            }
        }
        return maxprofit;
    }
}
```
由上一题的递推公式可以很轻松的解决这道题，但是注意如果k > prices.length的话，会产生mle，所以这时候要退化成第二题的贪心算法来解决。


### 309 Best Time to Buy and Sell Stock with Cooldown
```java
class Solution {
    public int maxProfit(int[] prices) {
          
         if(prices == null || prices.length == 0){
               return 0;  
         }
         //array
         //dp
         int len = prices.length;
         int[] buy = new int[len];
         int[] sell = new int[len];
          
         sell[0] = 0;
         buy[0] = -prices[0];
         for(int i = 1; i < prices.length;i++){
             buy[i] = Math.max((i > 2?sell[i-2]:0)-prices[i],buy[i-1]);
             sell[i] = Math.max(buy[i-1]+prices[i],sell[i-1]);
         }
         return sell[len-1];
    }
}
```
这道题给定的条件是，卖出需要一个休息期再买，所以前一个操作为买的最大值是buy[i] = sell[i-2]-prices[i]或者buy[i-1]
卖的比较直观sell[i] = buy[i-1]+prices[i]或者sell[i-1]


## 看你能装多少水
### 11 Container With Most Water
```java
class Solution {
    public int maxArea(int[] height) {
        if(height.length == 0){
           return 0;
        }
        //Arrays
        int end = height.length-1;
        int start = 0;
        int max = 0;
        while(start < end){
            max = Math.max(max,Math.min(height[start],height[end])*(end-start));
            if(height[start] < height[end]){
                 start++;
            }
            else{
                 end--;
            }
        }
        return max;
    }
}
```
非常简单的双指针问题，能装水的大小取决于短的那个边，所以只要一直更新短的边就能得到最值。


### 42 Trapping Rain Water
### 好题做5遍
```java
class Solution {
    public int trap(int[] height) {
        //array
        //two pointer
        int left = 0, right = height.length-1,res = 0,cur = 0;
        while(left < right){
            if(height[left] < height[left+1]){
                left++;
                continue;
            }
            if(height[right] < height[right-1]){
                right--;
                continue;
            }
            if(height[left] < height[right]){
                cur = left+1;
                while(height[left] > height[cur]){
                    res += height[left]-height[cur];
                    cur++;
                }
                left = cur;
            }
            else{
                cur = right-1;
                while(height[right] > height[cur]){
                    res += height[right]-height[cur];
                    cur--;
                }
                right = cur;
            }
        }
        return res;
    }
}
```
双指针，非常经典的一道双指针类型题，其双指针解法思想和11差不多，但是需要分析的情况比较多
1.left < left+1 需要更新left
2.right < right-1 需要更新right
3.height[left] < height[right],一直更新left右边指针直到到达大于height[left];
4.height[left] > height[right],一直更新right左边指针直到到达大于height[right];
常犯的错误是用while来更新left和right指针，而这其实会有使得循环中间left > right让ArrayIndex溢出的风险
O(n)级别
也可以用单调栈to do 单调栈
https://www.cnblogs.com/grandyang/p/8887985.html
## 递增怎么找？
### 334 Increasing Triplet Subsequence
```java
class Solution {
    public boolean increasingTriplet(int[] nums) {
         //array
         //想法是找到两个指针small，big，small < big
         //如果存在一个数大于这两个指针，则存在这样的子序列
         //否则没有
         int nums1 = Integer.MAX_VALUE;
         int nums2 = Integer.MAX_VALUE;
         for(int n:nums){
             if(nums1 >= n) nums1 = n;//
             else if(nums2 >= n) nums2 = n;//
             else return true;
         }
        return false;
    }
}

```
找到连续性三个数递增，存在两个数small，big找到一个比这两个数还小的数就返回true，否则返回false

### 128 Longest Consecutive Sequence
```java
class Solution {
        class UnionFind {
           private int count = 0;
           private int[] parent, rank;
           
           public UnionFind(int n) {
               count = n;
               parent = new int[n];
               rank = new int[n];
               for (int i = 0; i < n; i++) {
                   parent[i] = i;
               }
           }    
           public int find(int p) {
               while (p != parent[p]) {
                   parent[p] = parent[parent[p]];    // path compression by halving
                   p = parent[p];
               }
               return p;
           }
           
           public void union(int p, int q) {
               int rootP = find(p);
               int rootQ = find(q);
               if (rootP == rootQ) return;
               if (rank[rootQ] > rank[rootP]) {
                   parent[rootP] = rootQ;
               }
               else {
                   parent[rootQ] = rootP;
                   if (rank[rootP] == rank[rootQ]) {
                       rank[rootP]++;
                   }
               }
               count--;
           }
           
           public int count() {
               return count;
           }
        }
    public int longestConsecutive(int[] nums) {
        UnionFind uf = new UnionFind(nums.length);
        if(nums.length == 0) return 0;
        Map<Integer,Integer> map = new HashMap<>();
        for(int i = 0;i < nums.length;i++){
            if(map.containsKey(nums[i])) continue;
            if(map.containsKey(nums[i]-1)){
                uf.union(i, map.get(nums[i]-1));
            }
            if(map.containsKey(nums[i]+1)){
                uf.union(i, map.get(nums[i]+1));
            }
            map.put(nums[i],i);  
        }
        int[] res = new int[nums.length];
        for(int j = 0;j < nums.length;j++){
            res[uf.find(j)]++;
        }
        int r = 0;
        for(int k = 0;k < nums.length;k++){
            r = Math.max(res[k],r);
        }
        return r;
    }
}

```
让我们找到最长的连续整数序列，但是其中元素不一定是连续的，因为刚做完friend circle所以想的是合并集的方法‘
而一个hashmap的方法也能够满足O(n)
```java
public int longestConsecutive(int[] n){
        if(n == null || n.length == 0){
           return 0;
        }
        Set<Integer> set = new HashSet<>();
        set.addAll(Arrays.stream(n).boxed().collect(Collectors.toList()));
        int res = 1;
        for(int num:n){
           if(set.contains(num)){
              int pre = num - 1;
              int next = num + 1;
              while(set.contains(pre)){
                  set.remove(pre);
                  pre--;
              }
              while(set.contains(next)){
                set.remove(next);
                  next++;
            }
             res = Math.max(res,next-pre-1);
        }
        set.remove(num);
    }
    return res;
}
```
## bucket
### 164 Maximum Gap
```java
class Solution {
    public int maximumGap(int[] nums) {
        if(nums.length == 0) return 0;
        //array
        //bucket sort;
        int min = Integer.MAX_VALUE,max = Integer.MIN_VALUE;
        int len = nums.length;
        for(int n:nums){
            min = Math.min(min,n);
            max = Math.max(max,n);
        }
        //计算出bucket size
        int bucket = (max-min)/len+1;
        //计算bucket 个数
        int count = (max-min)/bucket+1;
        Integer[] ma = new Integer[count];
        Integer[] mi = new Integer[count];
        for(int n:nums){
            //cnt计算n属于第几个bucket
            //ma保存bucket最大值
            //mi保存最小值
            int cnt = (n-min)/bucket;
           if(ma[cnt] == null) ma[cnt]=n;
           if(mi[cnt] == null) mi[cnt]=n;
              ma[cnt] = Math.max(ma[cnt],n);
              mi[cnt] = Math.min(mi[cnt],n);
        }
        int res = 0,pre = 0;
        for(int i = 1;i < count;i++){
            if(ma[i] == null || mi[i] == null) continue;
            res = Math.max(res,mi[i]-ma[pre]);
            pre = i;
        }
        return res;
    }
}

```
Bucket
利用bucketsort的性质，我们不可能在sort的array中在同一个bucket中找到最大最小值，因为同一个bucket中的最大距离为bucket.size-1,而bucketsize为array中最大最小值差除以array中的数量，所以我们要找的一定是一个bucket的最小值和相邻的最大值

### 287 Find the Duplicate Number
```java
class Solution {
    public int findDuplicate(int[] nums) {
        //array
        //two pointer
        //由于重复数字的存在，则所有数字必定形成一个环[1,2,3,4,3] 0->1->2->3->4->3
        //3,4形成闭环，那么用快慢指针先遍历一边取到相同的数字跳出，指针在环内，
        //因为快指针每次走2，慢指针每次走1，快指针走的距离是慢指针的两倍。而快指针又比慢指针多走了一圈。
        //所以 head 到环的起点+环的起点到他们相遇的点的距离 与 环一圈的距离相等。
        //现在重新开始，head 运行到环起点 和 相遇点到环起点 的距离也是相等的，相当于他们同时减掉了 环的起点到他们相遇的点的距离
        int slow = 0,fast = 0;
        while(true){
            slow = nums[slow];
            fast = nums[nums[fast]];
            if(slow == fast) break;
        }
        int pointer = 0;
        while(true){
            slow = nums[slow];
            pointer = nums[pointer];
            if(slow == pointer) break;
        }
        
       return slow;
        
    }
}
```
由于重复数字的存在，则所有数字必定形成一个环[1,2,3,4,3] 0->1->2->3->4->3
3,4形成闭环，那么用快慢指针先遍历一边取到相同的数字跳出，指针在环内，
因为快指针每次走2，慢指针每次走1，快指针走的距离是慢指针的两倍。而快指针又比慢指针多走了一圈。
所以 head 到环的起点+环的起点到他们相遇的点的距离 与 环一圈的距离相等。
现在重新开始，head 运行到环起点 和 相遇点到环起点 的距离也是相等的，相当于他们同时减掉了 环的起点到他们相遇的点的距离.

### 135 Candy
```java
class Solution {
    public int candy(int[] ratings) {
        //array
        //greedy
        //two pass
        if(ratings == null || ratings.length == 0){
            return 0;
        }
        int len = ratings.length;
        int[] rate = new int[len];
        Arrays.fill(rate,1);
        for(int i = 1;i<len;i++){
            if(ratings[i] > ratings[i-1]){
                rate[i] = rate[i-1]+1;
            }
        }
        for(int i = len-1;i >= 1;i--){
            if(ratings[i-1] > ratings[i] && rate[i-1] <= rate[i]){
                rate[i-1] = rate[i]+1;
            }
        }
        int res = 0;
        for(int n:rate){
           res += n;
        }
        return res;
    }
}
```
贪心算法，贪心算法的难点就在于如何判断用贪心算法解题，比较泛泛的概念就是局部的最优解不会影响全局的最优解，就可以用贪心算法，这道题中显然局部的最优解不会影响全局最优解，可以尝试用
证明贪心的正确性用反证法，也就是存在一个点不是最少分配的糖果，但是全局却是最小，这种情况显然不可能，因为如果这个值是高于周围的，那么最小值产生的全值一定小于这个值产生的全职，如果是
低于周围的，那么显然会影响周围的分配使全局更大，所以需要求每个值的最小值
1.该值是低于等于周围两点的赋1
2.该值是高于周围两点，等于高的那一点加1
3.该值是高于周围一点，等于那点加1
但是这样也很难写出动态规划方程，因为我们求不出下一个点的分配糖果值，我们可以将比较分解
1.对所有值赋1
2.顺序，如果大于前一个数，就对分配前一个数多一个的糖果
3.反序，如果大于后一个数，且后一个数分配到不少于当前的糖果，那么就分配后一个数多一个的糖果。
很少考

### 330 Patching Array
```java
class Solution {
    public int minPatches(int[] nums, int n) {
        
        //array
        //greedy
        //find the smallest missing number
        //eg[1,3] the smallest is 2
        //因为前面元素的和范围是[1,2)
        //对这个数组补2
        int res = 0,i = 0,l = nums.length;
        long miss = 1;
        while(miss <= n){
            if( i < l && miss >= nums[i]){
                //miss > nums[i],不需要补任何数字
                //更新miss,范围变成[1,miss+nums[i])
                miss += nums[i++];
            }
            else{
                //nums[i] 大于 miss，我们补miss并更新miss为miss+miss
                //范围变成[1,miss+miss)
                miss+=miss;
                res++;
            }
        }
        return res;
    }
}
```
非常好的一道思维题目对于连续数来说，让我们想一下一个不需要patch的最简单数组，事实上是一个连续数组从[2^0,2^1,..2^n]满足sum of this array number，我们如何判断需要patch呢，就是对于前一个到n的不需要patch数组，如果我们判断它能到达的范围小于我们所设定的missing number值，那么就添加一个patch，然后让范围扩大到missingnumber*2）开区间，因为取不到该值。那现在的关键就是，我们怎么找missing number呢，从上个条件可以知道，这个数组的和就是满足nopatch数组的最大范围，那么我们就设定missing number = sum+1，然后在判断对于后面这个数，是否需要patch就可以
1.missing number = sum+1；
2.判断nums[i+1]是否大于missing number，如果大于说明[0,i+1]是需要patch的数组，因为显然取不到missing number
3.补missing number 然后 missingnumber = missingnumber*2；

很少考


## 难题hold不住
### 4 Median of Two Sorted Arrays
```java
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        
        int l1 = nums1.length,l2=nums2.length,left = (l1+l2+1)/2,right = (l1+l2+2)/2;
        
        //trick:中位数如果left+right 为基数个时 为 （left+right）/2 + 1 个为中位数 
        //为偶数时 （left+right）/ 2 和 （left+right）/2 + 1的平均数
        //我们设left = (l1+l2+1)/2 奇数时和right相等，偶数时 向下取整left = (left+right)/2
        //避免了分奇偶讨论的情况；
        return (findKth2Array(nums1,nums2,left,0,0)+findKth2Array(nums1,nums2,right,0,0))/2.0;
        
        
       
    }
    private double findKth2Array(int[] num1,int[] num2,int k,int start1,int start2){
        
        //对n1,n2分别取前k/2，比较大小，去掉小的那部分，再对剩下的两个数组做find(k-k/2)操作
        //直到k = 0
        //如果剩下的不足k/2，那么去掉前一个的k/2
        //如果k = 1，比较start1和start2的值就可以
        //如果start > length 那只需要找剩下数组的第k个元素
        
        if(start1 > num1.length - 1) return num2[start2+k-1]; 
        if(start2 > num2.length - 1) return num1[start1+k-1];
        if(k == 1) return num1[start1] < num2[start2] ? num1[start1] : num2[start2];
        if(num1.length - start1 < k/2) return findKth2Array(num1,num2,k-k/2,start1,start2+k/2);
        if(num2.length - start2 < k/2) return findKth2Array(num1,num2,k-k/2,start1+k/2,start2);
        int valnum1 = num1[k/2+start1-1];
        int valnum2 = num2[k/2+start2-1];
        if(valnum1 > valnum2){
            return findKth2Array(num1,num2,k-k/2,start1,start2+k/2);
        }
        else{
            return findKth2Array(num1,num2,k-k/2,start1+k/2,start2);
        }
    }
}
```
经典二分查找，在两个有序数组里查找第k个数，注释里已经说明很清楚，分别找两个数组的第k/2比较，然后去掉最小的，如果一个数组取完了，就直接剩下数组的余下数字个。


### 321 Create Maximum Number
```java
class Solution {
    public int[] maxNumber(int[] nums1, int[] nums2, int k) {
        int l1 = nums1.length,l2 = nums2.length;
        int[] res = new int[k];
        if(l1+l2 < k) return new int[0];
        for(int i = Math.max(0,k-l2);i <= l1 && i <= k;i++){
              int[] candidate = merge(Maxvalue(nums1,i),Maxvalue(nums2,k-i));
              if(greater(candidate,0,res,0)) res = candidate;
        }
        return res;
    }
    public boolean greater(int[] nums1, int i, int[] nums2, int j) {
       while (i < nums1.length && j < nums2.length && nums1[i] == nums2[j]) {
            i++;
            j++;
        }
        return j == nums2.length || (i < nums1.length && nums1[i] > nums2[j]);
    }
    private int[] merge(int[] n1,int[] n2){
        int[] res = new int[n1.length+n2.length];
        int pos = 0,i = 0,j = 0;
        for(i = 0,j = 0;pos < res.length;pos++)
            res[pos] = greater(n1,i,n2,j)?n1[i++]:n2[j++];
        return res;
    }
    private int[] Maxvalue(int[] n,int k){
        //greedy
        int len = n.length;
        int[] res = new int[k];
        for(int i = 0, j = 0;i < len;i++){
            while(len-i+j > k && j > 0 && res[j-1] < n[i]){
                   j--;
            }
            if(j < k) res[j++] = n[i];
        }
        return res;
    }
}
```
这道题包括排序，贪心，是很难的一道题，我们分别对两个数字进行取最值，取最值的方法贪心算法，对当前值与之前的所有值比较。取较大值，取的值加余下值不能大于取数的长度，再对两个数组分别取值，merge俩个数组取得最大值

### 327 Count of Range Sum
```java
class Solution {
    public int countRangeSum(int[] nums, int lower, int upper) {
         //brute force
         int count = 0;
         for(int i = 0;i < nums.length;i++){
            long res = nums[i];
            for(int j = i+1;j < nums.length;j++){
                if(res >= lower && res <= upper) count++;
                res += nums[j];
            }
            if(res >= lower && res <= upper) count++;
         }
         return count;
    }
}
```
bit index tree//segment tree
很少考

### 289 Game of Life
```java
class Solution {
    public void gameOfLife(int[][] board) {
        //2d array
        //状态机
        int[][] dir = {
            {-1,0},{-1,1},{0,1},{1,1},{1,0},{1,-1},{0,-1},{-1,-1}
        };
        int dietodie = 0;
        int livetolive = 1;
        int livetodie = 2;
        int dietolive = 3;
        for(int i = 0;i < board.length;i++){
            for(int j = 0;j < board[0].length;j++){
                int count  = 0;
                for(int k = 0;k < dir.length;k++){
                    int col = i + dir[k][0];
                    int row = j + dir[k][1];
                    if(col >= 0 && col < board.length && row >= 0 && row < board[0].length){
                       if(board[col][row] > 0 && board[col][row] < 3){
                           //如果是live的count++
                           count++;
                       }    
                    }
                }
                if(board[i][j] == 0){
                    if(count == 3){
                        board[i][j] = dietolive;
                    }
                    else{
                        board[i][j] = dietodie;
                    }
                }else if(board[i][j] == 1){
                    if(count == 2 || count == 3){
                        board[i][j] = livetolive;
                    }
                    else{
                        board[i][j] = livetodie;
                    }
                }
            }
        }
       for(int i = 0;i < board.length;i++){
            for(int j = 0;j < board[0].length;j++){
                board[i][j] %= 2;
            }
        }
        
    }
}

```
利用状态机，dietodie = 0，livetolive = 1，livetodie = 2，dietolive = 3，如果周围是1或2，count++，然后遍历所有节点取2的model等于1时为生存

## Interval

### 57 Insert Interval
```java
class Solution {
    public int[][] insert(int[][] intervals, int[] newInterval) {
        //interval
        if(intervals == null 
           || intervals.length == 0 
           || intervals[0].length == 0 
           || newInterval == null 
           || newInterval.length == 0) {
            return new int[][] {newInterval};
        }
        
        List<int []> list = new ArrayList<int[]>();
        int insertPos = 0;
        //merge interval
        for(int i = 0; i < intervals.length; i ++) {
            int [] cur = intervals[i];
            if(newInterval[1] < cur[0]) {
                //newend < start 加入这个interval
                list.add(cur);
            } else if (newInterval[0] > cur[1]) {
                //end < newstart 加入并且++pos；
                list.add(cur);
                insertPos ++;//找到合并的interval的位置
            } else {
                //其他情况合并两个interval
                newInterval[0] = Math.min(cur[0], newInterval[0]);
                newInterval[1] = Math.max(cur[1], newInterval[1]);
            }   
        }
        list.add(insertPos,newInterval);
        return list.toArray(new int[list.size()][2]);
    } 
}
```
O(n),顺序查找可以合并的interval，并且保存这个interval所插入的位置，如果end < newstart 说明位置在下一个；

### 56 Merge Intervals
```java
class Solution {
    public int[][] merge(int[][] intervals) {
        if(intervals.length == 0 || intervals[0].length == 0){
            return new int[0][0];
        }
        //sort intervals
        Arrays.sort(intervals,(a,b)->a[0]-b[0]);
        //create a list store intervals
        List<int[]> res = new ArrayList<>();
        //get a start interval 0;
        int[] newInterval = intervals[0].clone();
        for(int i = 1;i < intervals.length;i++){
            //newinterval[0] > interval[1] || newInterval[1] < interval[0]
            //merge
            if(newInterval[0] > intervals[i][1] || newInterval[1] < intervals[i][0]){
                  res.add(newInterval);
                  newInterval = intervals[i].clone();
            }else{
                  newInterval[0] = newInterval[0] < intervals[i][0] ?  newInterval[0] : intervals[i][0];
                  newInterval[1] = newInterval[1] > intervals[i][1] ?  newInterval[1] : intervals[i][1];
            }
        }
        res.add(newInterval);//corner case
        return res.toArray(new int[res.size()][2]);
    }
}
```
合并interval，先对所有的interval进行排序，然后再判断是否合并interval，有个小trick就是遍历时候遍历到最后会变成遍历不到最后一位，需要跳出循环后再补一位。
merge interval的模版方法
//如果 新的start 大于 找到interval的end 或者 新的end 大于找到的interval的start那么就无需合并，更新新的interval
//否则的话，合并新的interval，找到小的成为新start，找到大的成为新end


### 252 Meeting Rooms
```java
class Solution {
    public boolean canAttendMeetings(int[][] intervals) {  
        //interval  
        Arrays.sort(intervals,(a,b)->(a[0]-b[0]));
        for(int i = 1; i < intervals.length;i++){
            if(intervals[i][0] < intervals[i-1][1]){
                return false;
            }
        }
        
        return true;
        
    }
}
```
排序，然后对所有的时间进行比较，如果有overlap，返回false。


### 253 Meeting Rooms II
```java
class Solution {
    public int minMeetingRooms(int[][] intervals) {
        
        if(intervals.length == 0){
            return 0;
        }
      
        Arrays.sort(intervals,new Comparator<int[]>(){
            public int compare(int[] a,int[] b){
                return a[0]-b[0];
            }
        });
        //按顺序遍历整个数组，因为是以开始时间为排序所以可以按序寻找overlap
        //开始时间，要大于最早结束的会议时间，所以，用一个最小堆来存会议室的结束时间与开始时间比较
        //如果开始时间早于最早结束时间，那么新开一个会议室
        //如果符合，不需要新开，新的结束时间重新推回堆
        PriorityQueue<int[]> heap = new PriorityQueue<>(new Comparator<int[]>(){
            public int compare(int[] a, int[] b){
                return a[1]-b[1];
            }
        });
      
        heap.add(intervals[0]);
      
        for(int i = 1;i < intervals.length;i++){
            int[] tmp = heap.peek();
            if(intervals[i][0] < tmp[1]){
                heap.add(intervals[i]);
            }
            else{
                heap.poll();
                heap.add(intervals[i]);
            }
        }
       
        return heap.size();
    }
}
```
思想实际上是一种贪心算法，以开始时间排序，比较前一个的开始时间和结束时间，实际上是找overlap的最少数量，局部的最少显然是全局的最小值
按顺序遍历整个数组，因为是以开始时间为排序所以可以按序寻找overlap
开始时间，要大于最早结束的会议时间，所以，用一个最小堆来存会议室的结束时间与开始时间比较
如果开始时间早于最早结束时间，那么新开一个会议室
如果符合，不需要新开，新的结束时间重新推回堆

### 352 Data Stream as Disjoint Intervals
```java
class SummaryRanges {

    /** Initialize your data structure here. */
    //https://leetcode.com/problems/data-stream-as-disjoint-intervals/discuss/371756/JAVA-treemap-solution
    //比较清晰的讲解
    TreeMap<Integer,Integer> treemap = new TreeMap<>();
    public SummaryRanges() {
        
    }
    public void addNum(int val) {
        //找到floor
        Integer pre = treemap.floorKey(val);
        //找到ceil
        Integer next = treemap.ceilingKey(val);
        //如果 floor所处的区间[floor,treemap.get(floor)] 包含val，就返回
        if(pre!=null && treemap.get(pre)>=val){
            return;
        }
        //同理，如果next等于val返回
        if(next!=null && next == val){
            return;
        }
        
        int start = val;
        int end = val;
        //如果恰好val是next的前一个数，那更新interval[val,get(next)]
        if(next!=null && next-1==val){
            end = treemap.get(next);
            treemap.remove(next);
        }
        //如果恰好val是get(pre)的后一个数，那更新interval[pre,val]
        if(pre!=null && treemap.get(pre)+1==val){
            start = pre;
            treemap.remove(pre);
        }
        //加入treemap
        treemap.put(start, end);
        //整个过程所有操作为O(nlgn);
    }
    
    public int[][] getIntervals() {
        List<int[]> res = new ArrayList<>();
        for(Integer start:treemap.keySet()){
           res.add(new int[]{start,treemap.get(start)});
        }
      
        return res.toArray(new int[res.size()][2]);
        
    }
}
```
利用TreeMap的性质，好题做5遍
加入的新数分三种情况讨论[a,b]，1在interval[a,b]里，2,等于a-1，或者b+1，3,其他在区间外情况
1.不变
2,扩展interval变成[a-1,b]or[a,b+1]
3,加入新的interval[num,num];
重点在于找到num和a,b的关系,就可以模型化为找[num,num]在一个有序interval中位置的模型,理想的模型就是用treemap,寻找位置的时间复杂的为O(nlgn);

## Counter

### 239 Sliding Window Maximum
```java
class Solution {

    //queue
    //利用双端队列来做
    //利用循环创造一个单调的双端队列
    //单调栈，单调队列
    //优化空间思路，因为对于求一个滑动窗口的最值，最值index前的数是可以排除的
    //所以只保存最值及其之后值的顺序，到index删除掉
    private void inQueue(Deque<Integer> deque,int n,int[] nums){
            //保证queue的单调性
            // deque顺序为 max，second max....[maxindex....n]
            while(!deque.isEmpty() && nums[n] >= nums[deque.peekLast()]){
                 deque.pollLast();
            }
            deque.offer(n);
    }
    public int[] maxSlidingWindow1(int[] nums, int k) {
       //time O(n);each number will push in the deque at most once and poll out at most once, so it's o(n)
       if(nums == null || nums.length == 0){
           return new int[]{};
       }
       int[] res = new int[nums.length-k+1];
       Deque<Integer> deque = new LinkedList<>();
       //建立前k个数的单调队列
       for(int i = 0;i < k-1;i++){
           inQueue(deque,i,nums);
       }
       //维护这个队列
       for(int i = k-1;i < nums.length;i++){   
           inQueue(deque,i,nums);
           //peek为最值的index
           res[i-k+1] = nums[deque.peekFirst()];
           if(deque.peekFirst() == i-k+1){
               //如果index = 当前的数，那么删除
               deque.pollFirst();
           }
       }
       return res;
    }
    //最大堆
    //O(n*(lgn)^2)
    public int[] maxSlidingWindow(int[] nums, int k) {
         if(nums == null || nums.length == 0){
           return new int[]{};
         }
         //index heap 维护一个k大小的堆
         PriorityQueue<Integer> maxheap = new PriorityQueue<>((a,b)->(b-a));
         int[] res = new int[nums.length-k+1];
         int pos = 0;
         for(int i = 0;i < nums.length;i++){
             maxheap.offer(nums[i]);
             if(maxheap.size() == k){
                 res[pos++] = maxheap.peek();
                 maxheap.remove(nums[i-k+1]);
             }
         }
         return res;
    }
}
```
两种做法
第一种是单调队列，求局部最值时，用单调数据结构是一种很巧妙的方法，思路是单调队列存最大值的index，如果新加入的大于队列后面的元素，则一直删除尾部元素，直到遇到比它大的元素，维持单调性，如果滑动数组排除的元素，是队列顶端的最值index，则删除头部元素，然后每一轮存队列头部元素。O(n)级别，
第二种是维护一个最大堆，这个比较容易想，就是维护一个大小为k的最大堆，然后吧滑动窗口排除的元素删除出堆。

### 295 Find Median from Data Stream
```java
class MedianFinder {

    /** initialize your data structure here. */
    //minheap and maxheap
    PriorityQueue<Integer> minheap,maxheap;
    int len;
    public MedianFinder() {
        
        minheap = new PriorityQueue();
        maxheap = new PriorityQueue<>((a,b)->(b-a));
        len = 0;
        
    }
    
    public void addNum(int num) {
       // keep maxheap - minheap <= 1
       if(maxheap.size() == 0 || num <= maxheap.peek()){
            maxheap.offer(num);
       }
       if(num > maxheap.peek()){
           minheap.offer(num);
       }
       if(maxheap.size()-minheap.size() > 1){
           minheap.offer(maxheap.poll());
       }
       if(minheap.size()-maxheap.size() > 1){
           maxheap.offer(minheap.poll());
       }
       len++;
    }
    
    public double findMedian() {
        if(len % 2 == 1){
            return (double) maxheap.size() > minheap.size() ? maxheap.peek():minheap.peek();
        }else{
            return (double)(maxheap.peek()+minheap.peek())/2;
        }
    }
}
```
维护两个堆，最大最小堆，最大堆存数组的左侧，最小堆存数组的右侧，


### 53 Maximum Subarray
```java
class Solution {
    public int maxSubArray(int[] A) {
        //dp
        int n = A.length;
        int[] dp = new int[n+1];
        int max = A[0];
        
        for(int i = 1; i < n+1; i++){
            dp[i] = dp[i-1] > 0 ? dp[i-1]+A[i-1]:A[i-1];
            max = Math.max(max, dp[i]);
        }
        
        return max;
   }
}
```
简单dp,需要注意的是dp保存当前数，因为必须取一个数作为以该节点为终点的最大值
```java
public int maxSubArray(int[] A) {
        //divide conquer
        if(A.length == 0) return 0;
        return helper(A,0,A.length-1);
   }
   public int helper(int[] A,int left,int right){
        if(left > right) return Integer.MIN_VALUE;
        int mid = left+(right-left)/2;
        int left_max = helper(A,left,mid-1);
        int right_max = helper(A,mid+1,right);
        int mid_max = A[mid],t = mid_max;
        //必须从中间开始
        for(int i = mid-1;i >= left;i--){
              t += A[i];
              mid_max = Math.max(mid_max,t);
        }
        t = mid_max;
        for(int i = mid+1;i <= right;i++){
              t += A[i];
              mid_max = Math.max(mid_max,t);
        }
        return Math.max(mid_max,Math.max(left_max,right_max));
   }
}
```
分治法是求两边的最大和于包含中间的最大数组进行比较，时间复杂度nlgn

### 325 Maximum Size Subarray Sum Equals k
```java
class Solution {
    public int maxSubArrayLen(int[] nums, int k) {
        
           int sum = 0,max = 0;
           Map<Integer,Integer> prefix = new HashMap();
           for(int i= 0;i < nums.length;i++){
               sum += nums[i];
               if(sum == k) max = i+1;
               else if(prefix.containsKey(sum-k)){
                   max = Math.max(i-prefix.get(sum-k),max);
               }
               //greedy
               if(!prefix.containsKey(sum)){
                   prefix.put(sum,i);
               }         
           }
      
           return max;
    }
}
```
在表中存入0到i的和值和当前位置，假如说有key为sum-k那么可以理解为当前i值回到获得的位置的数组为sum == k并且长度为i-get(sum-k),需要注意的是，由于sum值可能重合，所以
当然要存入比较小的位置才能用到获得最长的子数组


### 209 Minimum Size Subarray Sum
```java
class Solution {
    public int minSubArrayLen(int s, int[] nums) {
        if(nums.length == 0) return 0;
        //sliding window
        int left = 0,right = 0,min = nums.length+1;
        int sum = nums[0];
        //o(n)
        while(right < nums.length){
            if(sum >= s ) {
                min = Math.min(min,right-left+1);
                sum -= nums[left];
                left++;
            }
            if(sum < s){        
                right++;
                if(right < nums.length)
                {
                    sum += nums[right];//right会溢出需要加条件
                }   
            }
        }
        return min == nums.length+1?0:min;*/
        
    }
}
```
双指针，非常简单
```java
class Solution {
    public int minSubArrayLen(int s, int[] nums) {
      //o(nlgn)
        //二分搜索
        //由于都是正数所以sum一定是增序
        int[] sum = new int[nums.length+1];
        int min = nums.length+1;
        sum[0] = 0;
        for(int i = 1;i < nums.length;i++){
            sum[i] = nums[i-1]+sum[i-1];
        }
        for(int i = 1;i<sum.length;i++){
            int left = i,right = sum.length;
            while(left < right){
                int mid = left+(right-left)/2;
                if(sum[mid]-sum[i-1] < s){
                    left = mid+1;
                }
                else{
                    right = mid;
                }
            }
            
            min = Math.min(left-i+1,min);
            
        }
        return min == nums.length+1?0:min;
    }
}
```
二分搜索
### 238 Product of Array Except Self
```java
class Solution {
    public int[] productExceptSelf(int[] nums) {
        
       /*solve o(n)*/
        int n = nums.length;
        int[] res = new int[n];
        res[0]=1;
        /* 1,2,3->6,3,2
        ->2*3,1*3,2*1 
        
        first loop set res[0]=1
        1,nums[0],nums[1]*nums[0]...
        second loop for res[n-1], use tmp = 1 
        ...nums[n-2]*nums[n-1],nums[n-1],1
        muti two array */
        for(int i = 1;i< n;i++){
            
            res[i] = res[i-1]*nums[i-1];
            
        }
        int tmp = 1;
        for(int j = n-1; j >= 0;j--){
            
            res[j] = tmp*res[j];
            tmp *= nums[j];
    
        }
        
        return res;
    }
}
```
two pass 题目要求不用除法，先从前到后在从后到前，每一位可以得到(one pass)nums[0..i-1]*(two pass)nums[i+1...l]

### 152 Maximum Product Subarray
```java
class Solution {
    public int maxProduct(int[] nums) {
        //dp
        int[] max = new int[nums.length];
        int[] min = new int[nums.length];
        max[0] = nums[0];
        min[0] = nums[0];
        int res = nums[0];
        for(int i = 1;i < nums.length;i++){
              max[i] = Math.max(nums[i],Math.max(nums[i]*max[i-1],nums[i]*min[i-1]));                   min[i] = Math.min(nums[i],Math.min(nums[i]*max[i-1],nums[i]*min[i-1]));
              res = Math.max(max[i],res);
        }
        return res;
    }
}
```
最大最小dp，记录以每个index为底的最大值和最小值，最大值和最小值能交换，记录每个最大值
### 228 Summary Ranges
```java
class Solution {
    public List<String> summaryRanges(int[] nums) {
        //continuous
        List<String> res = new ArrayList();
        if(nums.length == 0 || nums == null){
            return res;
        }
        int start = Integer.MIN_VALUE,end = Integer.MAX_VALUE;
        for(int i = 0;i < nums.length;i++){
             if(!isContinous(nums[i], end)){
                  if(start == end){
                      res.add(Integer.toString(start));
                  }else if(i != 0){
                      res.add(Integer.toString(start)+"->"+Integer.toString(end));
                  }
                  start = nums[i];
             }
             end = nums[i];
             if(i == nums.length-1){
                if(start == end){
                    res.add(Integer.toString(start));
                }else{
                    res.add(Integer.toString(start)+"->"+Integer.toString(end));
                }
            }
        }
        return res;
    }
    private boolean isContinous(int a,int b){
          return a-b == 1;
    }
}
```


163

Missing Ranges









Sort





88

Merge Sorted Array



75

Sort Colors

### 280 Wiggle Sort
```java
class Solution {
    public void wiggleSort(int[] nums) {
        // do it in O(n) time and/or in-place with O(1) extra space
        // bubble
       if(nums.length == 0) return;
        for(int i = 1;i < nums.length;i++){
            if(i%2 == 1 && nums[i] < nums[i-1]){
                swap(nums,i,i-1);
            }
            if(i%2 == 0 && nums[i] > nums[i-1]){
                 swap(nums,i,i-1);
            }
        }
    }
    private void swap(int[] n,int a,int b){
        int tmp = n[a];
        n[a] = n[b];
        n[b] = tmp;
    }
}
```
直接swap，分奇偶讨论，如果是奇数需要大于前一个数，偶数则反之。
做一下
### 324 Wiggle Sort II
```java
class Solution {
    public void wiggleSort(int[] nums) {
        // do it in O(n) time and/or in-place with O(1) extra space
        int median = findKth(0,nums.length-1,nums.length/2,nums);
        int left = 0,i = 0,n = nums.length,right = n-1;
        while(i <= right){
           if(nums[mapIndex(i,n)] > median){
              swap(nums,mapIndex(left++,n),mapIndex(i++,n));
           }else if(nums[mapIndex(i,n)] < median){
              swap(nums,mapIndex(right--,n),mapIndex(i,n));
           }else{
               i++;
           }
        }
    }
    private int mapIndex(int i,int l){
         return (1+2*i)%(l|1);
    }
    private int findKth(int left,int right,int k,int[] n){
         int s = left+1;
         int e = right;
         if(left >= right) return n[left];
         int pivot = n[left];
         while(s <= e){
            if(n[s++] > pivot) {
               swap(n,--s,e--);
            }
         }
         swap(n,e,left);
         if(e == k) return n[e];
         if(e < k) return findKth(e+1,right,k,n);
         else return findKth(left,e-1,k,n);
    }
    private void swap(int[] n,int a,int b){
        int tmp = n[a];
        n[a] = n[b];
        n[b] = tmp;
    }
}
```
mapIndex + quickselect 
未完待续


