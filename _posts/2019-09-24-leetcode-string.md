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
28

Implement strStr()

14

Longest Common Prefix

58

Length of Last Word

387

First Unique Character in a String

383

Ransom Note

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
        
        List<List<String>> res = new ArrayList<>();
        HashMap<String,List<String>> map = new HashMap<>();
        for(String str:strings){
            int x = str.charAt(0)-'a';
            StringBuilder sb = new StringBuilder();
            for(int j = 0;j < str.length();j++){
                char c = (char)(str.charAt(j)-x);
                if(c < 'a') c += 26;
                sb.append(c);
            }
            String key = sb.toString();
            if(!map.containsKey(key)){
                List<String> list = new ArrayList<>();
                list.add(str);
                map.put(key,list);
            }
            else{
                map.get(key).add(str);
            }
        }
        for(List l:map.values()){
            res.add(l);
        }
        return res;
    }
}
```
与49题的思路相似，找能shift的字符串，则我们需要找到一个key来存这个list，根据题意就是每个char在字母表上移相同的位置，所以我们用每个char减去第一个char再构成一个字符串，所得到的key就是唯一的。注意由于z再移一位为a，所以我们重构key的时候，如果key<'a'需要加26回位。
87

Scramble String

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



68

Text Justification

65

Valid Number

157

Read N Characters Given Read4

158

Read N Characters Given Read4 II - Call multiple times





Substring



76

Minimum Window Substring

30

Substring with Concatenation of All Words

3

Longest Substring Without Repeating Characters

340

Longest Substring with At Most K Distinct Characters

395

Longest Substring with At Least K Repeating Characters

159

Longest Substring with At Most Two Distinct Characters





Palindrome



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
