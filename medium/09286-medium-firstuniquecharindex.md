<!--info-header-start--><h1>FirstUniqueCharIndex <img src="https://img.shields.io/badge/-medium-d9901a" alt="medium"/> <img src="https://img.shields.io/badge/-%23string-999" alt="#string"/></h1><blockquote><p>by jiangshan <a href="https://github.com/jiangshanmeta" target="_blank">@jiangshanmeta</a></p></blockquote><p><a href="https://tsch.js.org/9286/play" target="_blank"><img src="https://img.shields.io/badge/-Take%20the%20Challenge-3178c6?logo=typescript&logoColor=white" alt="Take the Challenge"/></a> </p><!--info-header-end-->

Given a string s, find the first non-repeating character in it and return its index. If it does not exist, return -1. (Inspired by [leetcode 387](https://leetcode.com/problems/first-unique-character-in-a-string/))

<!--info-footer-start--><br><a href="../../README.md" target="_blank"><img src="https://img.shields.io/badge/-Back-grey" alt="Back"/></a> <a href="https://tsch.js.org/9286/answer" target="_blank"><img src="https://img.shields.io/badge/-Share%20your%20Solutions-teal" alt="Share your Solutions"/></a> <a href="https://tsch.js.org/9286/solutions" target="_blank"><img src="https://img.shields.io/badge/-Check%20out%20Solutions-de5a77?logo=awesome-lists&logoColor=white" alt="Check out Solutions"/></a> <!--info-footer-end-->

## 答案

```ts
type FirstUniqueCharIndex<T extends string, L extends any[] = []> = T extends `${infer A}${infer B}`
? B extends `${any}${A}${any}` 
  ? FirstUniqueCharIndex<B, [...L, A]> 
  : A extends L[number] 
    ? FirstUniqueCharIndex<B, [...L, A]> 
    : L['length']
: -1
```

1. 先查看剩下的字符串 B 是否包含 A
2. 如果包含，则递归调用 `FirstUniqueCharIndex` 查看剩余的字符串 B 中是否有未重复的字符
3. 如果 B 不包含 A，则查看之前比较过的字符串 L 中是否包含 A，如果包含，则重复第二步
4. 不包含则直接返回数组 L 的长度（数组 L 包含了之前遍历过的字符）
