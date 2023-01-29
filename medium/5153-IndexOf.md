<!--info-header-start--><h1>IndexOf <img src="https://img.shields.io/badge/-medium-d9901a" alt="medium"/> <img src="https://img.shields.io/badge/-%23array-999" alt="#array"/></h1><blockquote><p>by Pineapple <a href="https://github.com/Pineapple0919" target="_blank">@Pineapple0919</a></p></blockquote><p><a href="https://tsch.js.org/5153/play" target="_blank"><img src="https://img.shields.io/badge/-Take%20the%20Challenge-3178c6?logo=typescript&logoColor=white" alt="Take the Challenge"/></a> </p><!--info-header-end-->

Implement the type version of Array.indexOf, indexOf<T, U> takes an Array T, any U and returns the index of the first U in Array T.

```ts
type Res = IndexOf<[1, 2, 3], 2>; // expected to be 1
type Res1 = IndexOf<[2,6, 3,8,4,1,7, 3,9], 3>; // expected to be 2
type Res2 = IndexOf<[0, 0, 0], 2>; // expected to be -1
```


<!--info-footer-start--><br><a href="../../README.md" target="_blank"><img src="https://img.shields.io/badge/-Back-grey" alt="Back"/></a> <a href="https://tsch.js.org/5153/answer" target="_blank"><img src="https://img.shields.io/badge/-Share%20your%20Solutions-teal" alt="Share your Solutions"/></a> <a href="https://tsch.js.org/5153/solutions" target="_blank"><img src="https://img.shields.io/badge/-Check%20out%20Solutions-de5a77?logo=awesome-lists&logoColor=white" alt="Check out Solutions"/></a> <!--info-footer-end-->

## 答案
```ts
type RealEqual<T, U> = T extends U ? U extends T ? true : false : false

type IndexOf<T, U, Count extends number[] = []> = T extends [infer A, ...infer B] 
? (RealEqual<A, U> extends true ? Count['length'] : IndexOf<B, U, [...Count, 1]>)
: -1
```
题目总体上不难，难点是在比较两个数上。
```ts
type TestA<A, B> = A extends B ? true : false
type TestB<A, B> = B extends A ? true : false

type a = TestA<1, number> // true
type b = TestB<1, number> // false
```
比如 `1` 和 `number` 进行比较，首先 `1 extends number` 是相等的，但反过来不行。所以为了证明两个数真正相等，必须颠倒位置比较两次才能得到结果。