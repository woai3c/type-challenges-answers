<!--info-header-start--><h1>IsUnion <img src="https://img.shields.io/badge/-medium-d9901a" alt="medium"/> </h1><blockquote><p>by null <a href="https://github.com/bencor" target="_blank">@bencor</a></p></blockquote><p><a href="https://tsch.js.org/1097/play" target="_blank"><img src="https://img.shields.io/badge/-Take%20the%20Challenge-3178c6?logo=typescript&logoColor=white" alt="Take the Challenge"/></a> </p><!--info-header-end-->

Implement a type `IsUnion`, which takes an input type `T` and returns whether `T` resolves to a union type.

For example:
  
  ```ts
  type case1 = IsUnion<string>  // false
  type case2 = IsUnion<string|number>  // true
  type case3 = IsUnion<[string|number]>  // false
  ```


<!--info-footer-start--><br><a href="../../README.md" target="_blank"><img src="https://img.shields.io/badge/-Back-grey" alt="Back"/></a> <a href="https://tsch.js.org/1097/answer" target="_blank"><img src="https://img.shields.io/badge/-Share%20your%20Solutions-teal" alt="Share your Solutions"/></a> <a href="https://tsch.js.org/1097/solutions" target="_blank"><img src="https://img.shields.io/badge/-Check%20out%20Solutions-de5a77?logo=awesome-lists&logoColor=white" alt="Check out Solutions"/></a> <hr><h3>Related Challenges</h3><a href="https://github.com/type-challenges/type-challenges/blob/main/questions/01042-medium-isnever/README.md" target="_blank"><img src="https://img.shields.io/badge/-1042%E3%83%BBIsNever-d9901a" alt="1042・IsNever"/></a>  <a href="https://github.com/type-challenges/type-challenges/blob/main/questions/00223-hard-isany/README.md" target="_blank"><img src="https://img.shields.io/badge/-223%E3%83%BBIsAny-de3d37" alt="223・IsAny"/></a>  <a href="https://github.com/type-challenges/type-challenges/blob/main/questions/04484-medium-istuple/README.md" target="_blank"><img src="https://img.shields.io/badge/-4484%E3%83%BBIsTuple-d9901a" alt="4484・IsTuple"/></a> <!--info-footer-end-->

## 答案
```ts
// 题解：https://github.com/type-challenges/type-challenges/issues/1140
type IsUnion<T, C = T> = (T extends T ? (C extends T ? true : false) : never) extends true ? false : true
```
`T exnteds T` 相当于 js 中的 `for (const item of [1, 2, 3])`，只不过有一点不同的是： `T extends T` 会把遍历后的值赋值给 T。从上面 js 的例子来看，T 的值分别为 `1, 2, 3`。

经过遍历后，T 的值已经改变，所以需要通过 C 来拿到它原来的数据。

下面是原答案中提供的拆解示例：
```ts
=> IsUnion<string|number, string|number>
=> (string|number extends string|number ? string|number extends string|number ? true : unknown : never) extends true ? false : true
=> (
  (string extends string|number ? string|number extends string ? true : unknown : never) |
  (number extends string|number ? string|number extends number ? true : unknown : never)
) extends true ? false : true
=> (
  (string|number extends string ? true : unknown) |
  (string|number extends number ? true : unknown)
) extends true ? false : true
=> (
  (
    (string extends string ? true : unknown) |
    (number extends string ? true : unknown)
  ) |
  (
    (string extends number ? true : unknown) |
    (number extends number ? true : unknown)
  )
) extends true ? false : true
=> (
  (
    (true) |
    (unknown)
  ) |
  (
    (unknown) |
    (true)
  )
) extends true ? false : true
=> (true|unknown) extends true ? false : true
=> (unknown) extends true ? false : true
=> true
```
还有一点不明白的是，为什么 `(true|unknown) extends true ? false : true` 可以推导出 `(unknown) extends true ? false : true`。
经过测试 `true|unknown` 会把 `true` 给消除掉，留下 `unknown`，但是目前暂不清楚原因。