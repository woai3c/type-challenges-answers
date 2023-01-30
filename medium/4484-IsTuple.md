<!--info-header-start--><h1>IsTuple <img src="https://img.shields.io/badge/-medium-d9901a" alt="medium"/> <img src="https://img.shields.io/badge/-%23tuple-999" alt="#tuple"/></h1><blockquote><p>by jiangshan <a href="https://github.com/jiangshanmeta" target="_blank">@jiangshanmeta</a></p></blockquote><p><a href="https://tsch.js.org/4484/play" target="_blank"><img src="https://img.shields.io/badge/-Take%20the%20Challenge-3178c6?logo=typescript&logoColor=white" alt="Take the Challenge"/></a> </p><!--info-header-end-->

Implement a type ```IsTuple```, which takes an input type ```T``` and returns whether ```T``` is tuple type.

For example:

```typescript
type case1 = IsTuple<[number]> // true
type case2 = IsTuple<readonly [number]> // true
type case3 = IsTuple<number[]> // false
```


<!--info-footer-start--><br><a href="../../README.md" target="_blank"><img src="https://img.shields.io/badge/-Back-grey" alt="Back"/></a> <a href="https://tsch.js.org/4484/answer" target="_blank"><img src="https://img.shields.io/badge/-Share%20your%20Solutions-teal" alt="Share your Solutions"/></a> <a href="https://tsch.js.org/4484/solutions" target="_blank"><img src="https://img.shields.io/badge/-Check%20out%20Solutions-de5a77?logo=awesome-lists&logoColor=white" alt="Check out Solutions"/></a> <hr><h3>Related Challenges</h3><a href="https://github.com/type-challenges/type-challenges/blob/main/questions/01042-medium-isnever/README.md" target="_blank"><img src="https://img.shields.io/badge/-1042%E3%83%BBIsNever-d9901a" alt="1042・IsNever"/></a>  <a href="https://github.com/type-challenges/type-challenges/blob/main/questions/01097-medium-isunion/README.md" target="_blank"><img src="https://img.shields.io/badge/-1097%E3%83%BBIsUnion-d9901a" alt="1097・IsUnion"/></a>  <a href="https://github.com/type-challenges/type-challenges/blob/main/questions/00223-hard-isany/README.md" target="_blank"><img src="https://img.shields.io/badge/-223%E3%83%BBIsAny-de3d37" alt="223・IsAny"/></a> <!--info-footer-end-->

## 答案
```ts
type IsTuple<T> = [T] extends [never] 
? false 
: (
  T extends readonly any[] 
  ? (number extends T['length'] ? false : true)
  : false
)
```
`number extends T['length']` 是什么意思？如果是一个 number 类型的数组 `number[]`，那么 `T['length']` 得到的类型为 `number`。如果是 `[1]` 或 `[2]` 之类的元组（固定长度的数组就是一个元组）。那么 `T['length']` 得到的类型为 1 或 2。

在上面的代码中，传入不同的 T 会得到不同的结果。
```ts
type Test<T extends any[]> = number extends T['length'] ? true : false
type A = Test<number[]> // true, number extends number
type B = Test<[1]> // false, number extends 1
```
所以这道题的重点基本上就是理解 `number extends T['length']` 的意思。