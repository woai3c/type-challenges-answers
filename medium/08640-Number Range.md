<!--info-header-start--><h1>Number Range <img src="https://img.shields.io/badge/-medium-d9901a" alt="medium"/> </h1><blockquote><p>by AaronGuo <a href="https://github.com/HongxuanG" target="_blank">@HongxuanG</a></p></blockquote><p><a href="https://tsch.js.org/8640/play" target="_blank"><img src="https://img.shields.io/badge/-Take%20the%20Challenge-3178c6?logo=typescript&logoColor=white" alt="Take the Challenge"/></a> </p><!--info-header-end-->

Sometimes we want to limit the range of numbers...
For examples.

```ts
type result = NumberRange<2 , 9> //  | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 
```

<!--info-footer-start--><br><a href="../../README.md" target="_blank"><img src="https://img.shields.io/badge/-Back-grey" alt="Back"/></a> <a href="https://tsch.js.org/8640/answer" target="_blank"><img src="https://img.shields.io/badge/-Share%20your%20Solutions-teal" alt="Share your Solutions"/></a> <a href="https://tsch.js.org/8640/solutions" target="_blank"><img src="https://img.shields.io/badge/-Check%20out%20Solutions-de5a77?logo=awesome-lists&logoColor=white" alt="Check out Solutions"/></a> <!--info-footer-end-->

## 答案

```ts
// 用 L 作为参数创建一个 L 长度的数组
type ConstructArrayByNumber<L extends number, Result extends any[] = []> = Result['length'] extends L ? Result : ConstructArrayByNumber<L, [...Result, 1]>

// 递归
type NumberRange<
  L extends number, 
  H extends number, 
  Result = L, 
  CacheArray extends number[] = ConstructArrayByNumber<L>
> = L extends H 
? Result | L
: NumberRange<[...CacheArray, 1]['length'], H, Result | L,  [...CacheArray, 1]>
```
