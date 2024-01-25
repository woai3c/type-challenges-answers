<!--info-header-start--><h1>Replace First <img src="https://img.shields.io/badge/-medium-d9901a" alt="medium"/> </h1><blockquote><p>by George Flinn <a href="https://github.com/ProjectFlinn" target="_blank">@ProjectFlinn</a></p></blockquote><p><a href="https://tsch.js.org/25170/play" target="_blank"><img src="https://img.shields.io/badge/-Take%20the%20Challenge-3178c6?logo=typescript&logoColor=white" alt="Take the Challenge"/></a> </p><!--info-header-end-->

Implement the type ReplaceFirst<T, S, R> which will replace the first occurrence of S in a tuple T with R. If no such S exists in T, the result should be T.

<!--info-footer-start--><br><a href="../../README.md" target="_blank"><img src="https://img.shields.io/badge/-Back-grey" alt="Back"/></a> <a href="https://tsch.js.org/25170/answer" target="_blank"><img src="https://img.shields.io/badge/-Share%20your%20Solutions-teal" alt="Share your Solutions"/></a> <a href="https://tsch.js.org/25170/solutions" target="_blank"><img src="https://img.shields.io/badge/-Check%20out%20Solutions-de5a77?logo=awesome-lists&logoColor=white" alt="Check out Solutions"/></a> <!--info-footer-end-->

## 答案

```ts
type ReplaceFirst<T extends readonly unknown[], S, R> = T extends [infer A, ...infer Rest]
? A extends S 
  ? [R, ...Rest] 
  : [A, ...ReplaceFirst<Rest, S, R>]
: []
```

遍历数组，如果当前元素未匹配，则保留当前元素，并且递归遍历剩下的元素。如果当前元素匹配上了 S，则替换为 R，并将剩下的元素直接返回。
