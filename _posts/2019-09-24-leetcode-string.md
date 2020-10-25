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

242

Valid Anagram
## 相似的字符串总会相遇
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

```

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

3

Longest Substring Without Repeating Characters

340

Longest Substring with At Most K Distinct Characters

395

Longest Substring with At Least K Repeating Characters

159

Longest Substring with At Most Two Distinct Characters





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
