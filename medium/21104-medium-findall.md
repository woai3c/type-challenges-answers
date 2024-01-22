<!--info-header-start--><h1>FindAll <img src="https://img.shields.io/badge/-medium-d9901a" alt="medium"/> <img src="https://img.shields.io/badge/-%23template--literal-999" alt="#template-literal"/> <img src="https://img.shields.io/badge/-%23string-999" alt="#string"/></h1><blockquote><p>by tunamagur0 <a href="https://github.com/tunamagur0" target="_blank">@tunamagur0</a></p></blockquote><p><a href="https://tsch.js.org/21104/play" target="_blank"><img src="https://img.shields.io/badge/-Take%20the%20Challenge-3178c6?logo=typescript&logoColor=white" alt="Take the Challenge"/></a> </p><!--info-header-end-->

Given a pattern string P and a text string T, implement the type `FindAll<T, P>` that returns an Array that contains all indices (0-indexed) from T where P matches.

<!--info-footer-start--><br><a href="../../README.md" target="_blank"><img src="https://img.shields.io/badge/-Back-grey" alt="Back"/></a> <a href="https://tsch.js.org/21104/answer" target="_blank"><img src="https://img.shields.io/badge/-Share%20your%20Solutions-teal" alt="Share your Solutions"/></a> <a href="https://tsch.js.org/21104/solutions" target="_blank"><img src="https://img.shields.io/badge/-Check%20out%20Solutions-de5a77?logo=awesome-lists&logoColor=white" alt="Check out Solutions"/></a> <!--info-footer-end-->

## 答案

```ts
type FindAll<
  T extends string,
  P extends string,
  Result extends number[] = [],
  Len extends any[] = []
> = P extends '' 
? [] 
: T extends `${string}${infer Rest}` 
  ? T extends `${P}${string}` 
    ? FindAll<Rest, P, [...Result, Len['length']], [...Len, 1]> 
    : FindAll<Rest, P, Result, [...Len, 1]>
  : Result
```

1. 逐个字符遍历，然后再判断从当前字符开始是否匹配 `P`
2. 如果匹配 `P`，就把当前索引的长度记录到 `Result`
3. 如果不匹配，就从下一个字符开始继续递归
4. 在 2 3 步的过程中都需要把索引数组 `Len` 长度加 1，用于记录当前的索引位置
