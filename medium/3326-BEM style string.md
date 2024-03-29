<!--info-header-start--><h1>BEM style string <img src="https://img.shields.io/badge/-medium-d9901a" alt="medium"/> <img src="https://img.shields.io/badge/-%23template--literal-999" alt="#template-literal"/> <img src="https://img.shields.io/badge/-%23union-999" alt="#union"/> <img src="https://img.shields.io/badge/-%23tuple-999" alt="#tuple"/></h1><blockquote><p>by Songhn <a href="https://github.com/songhn233" target="_blank">@songhn233</a></p></blockquote><p><a href="https://tsch.js.org/3326/play" target="_blank"><img src="https://img.shields.io/badge/-Take%20the%20Challenge-3178c6?logo=typescript&logoColor=white" alt="Take the Challenge"/></a> </p><!--info-header-end-->

The Block, Element, Modifier methodology (BEM) is a popular naming convention for classes in CSS.

For example, the block component would be represented as `btn`, element that depends upon the block would be represented as `btn__price`, modifier that changes the style of the block would be represented as `btn--big` or `btn__price--warning`.

Implement `BEM<B, E, M>` which generate string union from these three parameters. Where `B` is a string literal, `E` and `M` are string arrays (can be empty).

<!--info-footer-start--><br><a href="../../README.md" target="_blank"><img src="https://img.shields.io/badge/-Back-grey" alt="Back"/></a> <a href="https://tsch.js.org/3326/answer" target="_blank"><img src="https://img.shields.io/badge/-Share%20your%20Solutions-teal" alt="Share your Solutions"/></a> <a href="https://tsch.js.org/3326/solutions" target="_blank"><img src="https://img.shields.io/badge/-Check%20out%20Solutions-de5a77?logo=awesome-lists&logoColor=white" alt="Check out Solutions"/></a> <!--info-footer-end-->

## 答案

```ts
type Helper<Symbol extends string, T extends string[]> = T extends [] ? '' : `${Symbol}${T[number]}` 
type BEM<B extends string, E extends string[], M extends string[]> = `${B}${Helper<'__', E>}${Helper<'--', M>}`
```

上述答案有两个知识点比较重要。

一、`T[number]` 数组中的每个索引都是数字，所以用 `number` 表示 T 中的每一项。`T[number]` 遍历整个 T 获得的联合类型。

```ts
type Combination<T extends unknown[]> = T[number]
type Test = Combination<['foo', 'bar', 'baz'] // "foo" | "bar" | "baz"
```

二、在类型模板字符串中，联合类型 `${a}${b}` 拼接时，a 和 b 是具有分发行为的，这个表达式中 a 和 b 会交叉相乘。例如:

```ts
type Name = 'jack' | 'tom'
type Age = 18 | 19 

// 结果为
type Result = `${Name}_${Age}` // "jack_18" | "jack_19" | "tom_18" | "tom_19"
```
