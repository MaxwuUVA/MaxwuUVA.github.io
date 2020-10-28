---
layout:     post
title:      leetcode 400 字符串 篇
subtitle:   
date:       2019-09-24
author:     953max
header-img: 
catalog: 	 true
tags:
    - leetcode
    - 字符串
---
### 28 Implement strStr()
```java
class Solution {
    public int strStr(String haystack, String needle) {
          if(haystack.length() < needle.length()) return -1;
          if(needle.length() == 0) return 0;
          for(int i = 0;i < haystack.length()-needle.length()+1;i++){
              if(haystack.substring(i).startsWith(needle)){
                   return i;
              }
          }
          return -1;
    }
}
```
利用startswith求解，简单

### 14 Longest Common Prefix
```java
class Solution {
    public String longestCommonPrefix(String[] strs) {
        if(strs.length == 0) return "";
        int pos = 0;
        StringBuilder res = new StringBuilder();
        for(char c:strs[0].toCharArray()){
             for(String s:strs){
                  if(s.length()-1 < pos || s.charAt(pos) != c){
                      return res.toString();
                  }
             }
             res.append(c);
             pos++;
        }
        return res.toString();
    }
    
}
```
单指针类型，简单
### 58 Length of Last Word
```java
class Solution {
    public int lengthOfLastWord(String s) {
        String[] strs = s.split(" ");
        if(s.length() == 0 || strs.length == 0) return 0;
        return strs[strs.length-1].length();
        
    }
}
```
split用法，trim也可以；
### 387 First Unique Character in a String
```java
class Solution {
    public int firstUniqChar(String s) {
        //string
        //map
        //two pass
        if(s.length() == 0) return -1;
        int[] map = new int[26];
        for(char c:s.toCharArray()){
            map[c-'a']++;
        }
        for(int i = 0;i < s.length();i++){
            if(map[s.charAt(i)-'a'] == 1){
                  return i;
            }
        }
        return -1;
        
    }
}
```
two pass 思路很简单，但discuss里面有人讨论这个问题 but it still can be improved, since you read through the whole array twice. Take an example of DNA sequence: there could be millions of letters long with just 4 alphabet letter. What happened if the non-repeated letter is at the end of DNA sequence? This would dramatically increase your running time since we need to scan it again.
更新one pass 方法
```java
 public int firstUniqChar(String s) {
        //string
        //map
        //one pass
        
        if(s.length() == 0) return -1;
        int[] map = new int[26];
        int[] index = new int[26];
        Arrays.fill(index,-1);
        for(int i = 0;i < s.length();i++){
            if(map[s.charAt(i)-'a'] == 0){
                  map[s.charAt(i)-'a']++;
                  index[s.charAt(i)-'a'] = i;
            }else{
                  index[s.charAt(i)-'a'] = -1;
            }
        }
        int res = s.length();
        for(int j = 0;j < 26;j++){
            if(index[j] != -1){
                res = Math.min(index[j],res);
            }
        }
        return res == s.length() ? -1:res;
        
    }
}
```
用另一个数组保存次数为1的index，并比较，可以把复杂度压缩到O(n+26);
### 383 Ransom Note

344

Reverse String

151

Reverse Words in a String

186

Reverse Words in a String II

345

Reverse Vowels of a String

205

Isomorphic Strings

293

Flip Game

294

Flip Game II

290

Word Pattern
## 相似的字符串总会相遇
### 242 Valid Anagram
```java
class Solution {
    public boolean isAnagram(String s, String t) {
        char[] cs = s.toCharArray();
        char[] ct = t.toCharArray();
        Arrays.sort(cs);
        Arrays.sort(ct);
        return String.valueOf(cs).equals(String.valueOf(ct));
    }
}
```
简单
### 49 Group Anagrams
```java
class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        
        List<List<String>> res = new ArrayList<>();
        Map<String,List> map = new HashMap<>(); //用排过序的String来做key
        for(int i = 0; i < strs.length;i++){
            
            List<String> sub = new ArrayList<>();
            sub.add(strs[i]);
            char[] s = strs[i].toCharArray();
            Arrays.sort(s);
            String key = String.valueOf(s);
            if(!map.containsKey(key)) map.put(key,sub);
            else  map.get(key).add(strs[i]);
            
        }
        for(List sub:map.values()) res.add(sub);
        return res;       
    }
}
```
这道题是找有相同字母的string，用排序后的string做key，list做value，每次遇到相同的key就把string插入value的list。
### 249 Group Shifted Strings
```java
class Solution {
    public List<List<String>> groupStrings(String[] strings) {
        Map<String,List<String>> map = new HashMap();
        for(String s:strings){
            String key = generateKey(s);
            map.putIfAbsent(key,new ArrayList());
            map.get(key).add(s);
        }
        List<List<String>> res = new ArrayList();
        for(Map.Entry<String,List<String>> e:map.entrySet()){
            res.add(e.getValue());
        }
        return res;
    }
    public String generateKey(String s){
        char[] ca = s.toCharArray();
        char t = ca[0];
        String keyvalue = "";
        for(int i = 1;i < ca.length;i++){
            int diff = ca[i]-t;
            if(diff < 0) diff += 26;
            keyvalue += diff+"#";
        }
        return keyvalue;
    }
}
```
与49题的思路相似，找能shift的字符串，则我们需要找到一个key来存这个list，根据题意就是每个char在字母表上移相同的位置，所以我们用每个char减去第一个char再构成一个字符串，所得到的key就是唯一的。注意由于z再移一位为a，所以我们重构key的时候，如果key<'a'需要加26回位。
### 87 Scramble String
```java
class Solution {
    public boolean isScramble(String s1, String s2) {
        
        //String
        //recursion
        if(s1.length() != s2.length()) {
            return false;
        }
        if(s1.equals(s2)) {
            return true;
        }
        char[] str1 = s1.toCharArray();
        char[] str2 = s2.toCharArray();
        
        Arrays.sort(str1);
        Arrays.sort(str2);
        String st1 = String.valueOf(str1);
        String st2 = String.valueOf(str2);
        if(!st1.equals(st2)){ 
                  return false;
        }
        for(int i = 1;i < s1.length();i++){
            String s11 = s1.substring(0,i);
            String s21 = s2.substring(0,i);
            String s12 = s1.substring(i);
            String s22 = s2.substring(i);
            if(isScramble(s11,s21) && isScramble(s12,s22)){
                 return true;
            }
            s21 = s2.substring(s2.length()-i);
            s22 = s2.substring(0,s2.length()-i); 
            if(isScramble(s11,s21) && isScramble(s12,s22)){
                 return true;
            }
        }
        return false;
    }
}
```
比较容易想到的是递归做法，对两个字符串分段然后比较前一段和后一段是否符合scramble即可，时间复杂度为exponential complexity O(5^n) 具体证明https://leetcode.com/problems/scramble-string/discuss/29392/Share-my-4ms-c%2B%2B-recursive-solution
```java
class Solution {
    public boolean isScramble(String s1, String s2) {
        //recursive
        //dp
        //区间
        boolean[][][] dp = new boolean[s1.length()][s1.length()][s1.length()+1];
        // dp[i][j][len] = || dp[i][j][k] && dp[i+k][j+k][len-k] || dp[i][j+len-k][k] && dp[i+k][j][len-k];
        for(int len = 1;len <= s1.length();len++){
            for(int i = 0;i <= s1.length()-len;i++){
                for(int j = 0;j <= s1.length()-len;j++){
                    if(len == 1){
                        //当s1[i] == s2[j] 时 dp[i][j][1] 有效 
                        dp[i][j][len] = s1.charAt(i) == s2.charAt(j);
                        continue;
                    }
                    for(int k = 1;k < len && !dp[i][j][len];k++){
                       //匹配到前后均为有效的返回true，均没有匹配到就返回false
                       dp[i][j][len] = (dp[i][j][k] && dp[i+k][j+k][len-k]) || (dp[i][j+len-k][k] && dp[i+k][j][len-k]);
                    }
                }
            }
        }
        return dp[0][0][s1.length()];
    }
}
```
然后是dp的解法，dp解法是一道比较难想的区间dp问题,dp[s1位置][s2位置][长度], dp[i][j][len] = || dp[i][j][k] && dp[i+k][j+k][len-k] || dp[i][j+len-k][k] && dp[i+k][j][len-k];
    1.当s1[i] == s2[j] 时 dp[i][j][1] 有效。
    2. dp[i][j][len] = || dp[i][j][k] && dp[i+k][j+k][len-k] || dp[i][j+len-k][k] && dp[i+k][j][len-k];匹配到前后均为有效的返回true，均没有匹配到就返回false
时间复杂度为O(N^4);
 
179

Largest Number

6

ZigZag Conversion

161

One Edit Distance

38

Count and Say

358

Rearrange String k Distance Apart

316

Remove Duplicate Letters

271

Encode and Decode Strings

168

Excel Sheet Column Title

171

Excel Sheet Column Number

13

Roman to Integer

12

Integer to Roman

273

Integer to English Words

246

Strobogrammatic Number

247

Strobogrammatic Number II

248

Strobogrammatic Number III





提高



### 68 Text Justification

### 65 Valid Number
```java
class Solution {
    public boolean isNumber(String s) {
        if(s == null || s.length() == 0) return false;
        s = s.trim();//remove space
        String[] space = s.split(" ");
        if(space.length > 1) return false;
        s = " "+s+" ";
        String[] strs = s.split("e");
        if(strs.length == 1) return front(strs[0]);
        if(strs.length > 2) return false;
        return front(strs[0]) && back(strs[1]);
    }
    private boolean front(String f){
        if(f == null || f.length() == 1) return false;
        f = f.trim();
        char[] c = f.toCharArray();
        int countDigit = 0,dountCount = 0;
        boolean sign = false;
        for(char ch:c){
            if(ch == '-' || ch == '+'){
                if(sign || countDigit > 0 || dountCount > 0) return false;
                else sign = true;
            }
            else if(!Character.isDigit(ch) && ch != '.'){
                return false;
            }else{
                if(ch =='.'){
                    if(dountCount > 0){
                        return false;
                    }
                    dountCount++;
                }
                else countDigit++;
            }
        }
        return countDigit > 0;
    }
    private boolean back(String b){
        if(b == null || b.length() == 1 ) return false;
        b = b.trim();
        char[] c = b.toCharArray();
        int countDigit = 0;
        boolean sign = false;
        for(char ch:c){
            if(ch == '-' || ch == '+'){
                if(sign || countDigit > 0) return false;
                else sign = true;
            }else if(!Character.isDigit(ch)){
                return false;
            }else{
                countDigit++;
            }
        }
        return countDigit > 0;
    }
}
```
最傻逼的一道题，疯狂卡corner case

### 157 Read N Characters Given Read4
```java
/**
 * The read4 API is defined in the parent class Reader4.
 *     int read4(char[] buf);
 */
public class Solution extends Reader4 {
    /**
     * @param buf Destination buffer
     * @param n   Number of characters to read
     * @return    The number of actual characters read
     */
    public int read(char[] buf, int n) {
        char[] buf4 = new char[4];
        int offset = 0;
        while(true){
            int size = read4(buf4);
            for(int i = 0;i < size && offset < n;i++){
                buf[offset++] = buf4[i];
            }
            if(size == 0 || offset == n){
                return offset;
            }
        }
        
    }
```
注意两个条件，file读完了和已经取得了n个数字，是下一道题的练习
### 158 Read N Characters Given Read4 II - Call multiple times
```java
/**
 * The read4 API is defined in the parent class Reader4.
 *     int read4(char[] buf4); 
 */

public class Solution extends Reader4 {
    /**
     * @param buf Destination buffer
     * @param n   Number of characters to read
     * @return    The number of actual characters read
     */
    private char[] tmpbuf = new char[4];
    private int tfpoint = 0;
    private int tfsize;
    public int read(char[] buf, int n) {
        int p = 0;
        while(p < n){
            if(tfpoint == 0){
                tfsize = read4(tmpbuf);
            }
            if(tfsize == 0) break;
            while(p < n && tfpoint < tfsize){
                buf[p++] = tmpbuf[tfpoint++];
            }
            if(tfpoint >= tfsize){
                tfpoint = 0;
            }
        }
        return p;
    }
}
```




## Substring



### 76 Minimum Window Substring
```java
class Solution {
    public String minWindow(String s, String t) {
        //string
        //sliding window
        if(s.isEmpty()) return s;
        int[] map = new int[256];
        for(char c:t.toCharArray()){
             map[c]++;
        }
        int l = 0,r = 0,cnt = 0,len = s.length()+1;
        String res = "";
        while(r<s.length()){
            if(--map[s.charAt(r)] >= 0){
                cnt++;
            }
            while(cnt == t.length()){
                if(len > r-l+1){
                    len = r-l+1;
                    res = s.substring(l,r+1);
                }
                if(++map[s.charAt(l)] > 0) cnt--;
                l++;
            }
            r++;
        }
        return res;
    }
}
```
给一个s再给一个t求最短的含有t的子字符串，滑动窗口求子字符串，用一个字典保存t，然后对s进行遍历，假如说遇到了足够的元素，再操作左指针向前进，找到最小的长度保存这个子字符串。
### 30 Substring with Concatenation of All Words
```java
class Solution {
    public List<Integer> findSubstring(String s, String[] words) {

        List<Integer> res = new ArrayList<>();
        if(s.isEmpty()|| words.length == 0) return res;
        Map<String,Integer> map1 = new HashMap<>();
        for(String word:words){

            map1.put(word,map1.getOrDefault(word, 0)+1);

        }
        for(int i = 0;i <= s.length()-words.length*words[0].length();i++){
            Map<String,Integer> map2 = new HashMap<>();
            for(int j = 0;j < words.length;j++){
                String cur = s.substring(i+j*words[0].length(),i+(j+1)*words[0].length());
                if(!map1.containsKey(cur)){
                    break;
                }
                map2.put(cur, map2.getOrDefault(cur,0)+1);
                if(map2.get(cur) > map1.get(cur)){
                    break;
                }
                if(j == words.length-1){
                    res.add(i);
                }
            }
            
        }

        return res;

    }
}
```
two map
```java
class Solution {
    public List<Integer> findSubstring(String s, String[] words) {
        //startwith
        List<Integer> res = new ArrayList();
        //corner case
        if(s == null|| words.length == 0) return res;
        int len = words.length*words[0].length();
        for(int i = 0;i < s.length()-len+1;i++){
            if(dfs(new boolean[words.length],s.substring(i,i+len),words)){
                res.add(i);
            }
        }
        return res;
    }
    private boolean dfs(boolean[] visit,String s,String[] words){
        if(s == null || s.length() == 0) return true;
        for(int i = 0;i < words.length;i++){
            if(!visit[i] && s.startsWith(words[i])){
                visit[i] = true;
                return dfs(visit,s.substring(words[i].length()),words);
            }
        }
        return false;
    }
    
}
```
startsWith+dfs
```java

```
trie

### 3 Longest Substring Without Repeating Characters
```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        //map + two pointer
        //greedy
        if (s.length()==0) return 0;
        HashMap<Character, Integer> map = new HashMap<Character, Integer>();
        int max=0;
        for (int i=0, j=0; i<s.length(); ++i){
            if (map.containsKey(s.charAt(i))){
                //left 为最大的重复char前一个字符
                j = Math.max(j,map.get(s.charAt(i))+1);
            }
            map.put(s.charAt(i),i);
            max = Math.max(max,i-j+1);
        }
        return max;
  }
}
```
最优算法是map加上双指针，维护左指针为最大的重复数组之后一个位置，再更新max

### 340 Longest Substring with At Most K Distinct Characters
```java
class Solution {
    public int lengthOfLongestSubstringKDistinct(String s, int k) {              
        if(s == null || s.length() == 0) return 0;
        //string 
        //sliding window改进
        Map<Character,Integer> map = new HashMap<>();
        int res = 0,l = 0,r = 0,len = s.length();
        while(r < len){
            if(map.size() <= k){
                char c = s.charAt(r);
                map.put(c,r);
                r++;
            }
            if(map.size() > k){
                int pointer = len - 1;
                for(int p:map.values()){
                    pointer = Math.min(pointer,p);
                }
                //找到
                char head = s.charAt(pointer);
                map.remove(head);
                l = pointer+1;
            }
            res = Math.max(res,r-l);   
            
        }
        return res; 
        //O(n+k)
    }
}
```
sliding window+hashmap维护一个k长度的map，存入char和位置index找到最小的char删除，更新max
```java
class Solution {
  public int lengthOfLongestSubstringKDistinct(String s, int k) {
    int n = s.length();
    if (n*k == 0) return 0;

    // sliding window left and right pointers
    int left = 0;
    int right = 0;
    // hashmap character -> its rightmost position 
    // in the sliding window
    LinkedHashMap<Character, Integer> hashmap = new LinkedHashMap<Character, Integer>(k + 1);

    int max_len = 1;

    while (right < n) {
      Character character = s.charAt(right);
      // if character is already in the hashmap -
      // delete it, so that after insert it becomes
      // the rightmost element in the hashmap
      if (hashmap.containsKey(character))
        hashmap.remove(character);
      hashmap.put(character, right++);

      // slidewindow contains k + 1 characters
      if (hashmap.size() == k + 1) {
        // delete the leftmost character
        Map.Entry<Character, Integer> leftmost = hashmap.entrySet().iterator().next();
        hashmap.remove(leftmost.getKey());
        // move left pointer of the slidewindow
        left = leftmost.getValue() + 1;
      }

      max_len = Math.max(max_len, right - left);
    }
    return max_len;
  }
}
```
LinkedListMap 有序字典法
### 395 Longest Substring with At Least K Repeating Characters

### 159 Longest Substring with At Most Two Distinct Characters





### Palindrome



125

Valid Palindrome

266

Palindrome Permutation

5

Longest Palindromic Substring

9

Palindrome Number

214

Shortest Palindrome

336

Palindrome Pairs

131

Palindrome Partitioning

132

Palindrome Partitioning II

267

Palindrome Permutation II





Parentheses



20

Valid Parentheses

22

Generate Parentheses

32

Longest Valid Parentheses

241

Different Ways to Add Parentheses

301

Remove Invalid Parentheses





Subsequence



392

Is Subsequence

115

Distinct Subsequences

187

Repeated DNA Sequences
