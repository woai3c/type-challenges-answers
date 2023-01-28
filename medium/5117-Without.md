<!--info-header-start--><h1>Without <img src="https://img.shields.io/badge/-medium-d9901a" alt="medium"/> <img src="https://img.shields.io/badge/-%23union-999" alt="#union"/> <img src="https://img.shields.io/badge/-%23array-999" alt="#array"/></h1><blockquote><p>by Pineapple <a href="https://github.com/Pineapple0919" target="_blank">@Pineapple0919</a></p></blockquote><p><a href="https://tsch.js.org/5117/play" target="_blank"><img src="https://img.shields.io/badge/-Take%20the%20Challenge-3178c6?logo=typescript&logoColor=white" alt="Take the Challenge"/></a> &nbsp;&nbsp;&nbsp;<a href="./README.zh-CN.md" target="_blank"><img src="https://img.shields.io/badge/-%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87-gray" alt="简体中文"/></a> </p><!--info-header-end-->

Implement the type version of Lodash.without, Without<T, U> takes an Array T, number or array U and returns an Array without the elements of U.

```ts
type Res = Without<[1, 2], 1>; // expected to be [2]
type Res1 = Without<[1, 2, 4, 1, 5], [1, 2]>; // expected to be [4, 5]
type Res2 = Without<[2, 3, 2, 3, 2, 3, 2, 3], [2, 3]>; // expected to be []
```

<!--info-footer-start--><br><a href="../../README.md" target="_blank"><img src="https://img.shields.io/badge/-Back-grey" alt="Back"/></a> <a href="https://tsch.js.org/5117/answer" target="_blank"><img src="https://img.shields.io/badge/-Share%20your%20Solutions-teal" alt="Share your Solutions"/></a> <a href="https://tsch.js.org/5117/solutions" target="_blank"><img src="https://img.shields.io/badge/-Check%20out%20Solutions-de5a77?logo=awesome-lists&logoColor=white" alt="Check out Solutions"/></a> <!--info-footer-end-->

## 答案
```ts
// 判断数组 T 是否包含 N
type Include<T, N> = T extends [infer A, ...infer B] 
? (N extends A ? true : Include<B, N>)
: false

// 将 T 和 U 进行比较，看看 U 是否包含 T，或者 U 是否和 T 相等
type Pick<T, U, Result extends any[]> = U extends [infer A, ...infer B] 
? (Include<U, T> extends true ? Result : [...Result, T])
: (T extends U ? Result : [...Result, T])

type Without<T extends any[], U, Result extends any[] = []> = T extends [infer A, ...infer B] 
? Without<B, U, Pick<A, U, Result>>
: Result
```
1. 遍历 T，逐项提取每个数，再和 U 进行比较，不在 U 中或不和 U 相等就把这个数放到 Result 中。
2. 提取的数 A 需要和 U 进行比较。如果 U 是数组，则判断 U 是否包含 A；如果 U 不是 数组，则将 A 和 U 直接进行比较。