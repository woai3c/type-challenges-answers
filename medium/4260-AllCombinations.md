<!--info-header-start--><h1>AllCombinations <img src="https://img.shields.io/badge/-medium-d9901a" alt="medium"/> <img src="https://img.shields.io/badge/-%23template--literal-999" alt="#template-literal"/> <img src="https://img.shields.io/badge/-%23infer-999" alt="#infer"/> <img src="https://img.shields.io/badge/-%23union-999" alt="#union"/></h1><blockquote><p>by 蛭子屋双六 <a href="https://github.com/sugoroku-y" target="_blank">@sugoroku-y</a></p></blockquote><p><a href="https://tsch.js.org/4260/play" target="_blank"><img src="https://img.shields.io/badge/-Take%20the%20Challenge-3178c6?logo=typescript&logoColor=white" alt="Take the Challenge"/></a> &nbsp;&nbsp;&nbsp;<a href="./README.ja.md" target="_blank"><img src="https://img.shields.io/badge/-%E6%97%A5%E6%9C%AC%E8%AA%9E-gray" alt="日本語"/></a> </p><!--info-header-end-->

Implement type ```AllCombinations<S>``` that return all combinations of strings which use characters from ```S``` at most once.

For example:

```ts
type AllCombinations_ABC = AllCombinations<'ABC'>;
// should be '' | 'A' | 'B' | 'C' | 'AB' | 'AC' | 'BA' | 'BC' | 'CA' | 'CB' | 'ABC' | 'ACB' | 'BAC' | 'BCA' | 'CAB' | 'CBA'
```


<!--info-footer-start--><br><a href="../../README.md" target="_blank"><img src="https://img.shields.io/badge/-Back-grey" alt="Back"/></a> <a href="https://tsch.js.org/4260/answer" target="_blank"><img src="https://img.shields.io/badge/-Share%20your%20Solutions-teal" alt="Share your Solutions"/></a> <a href="https://tsch.js.org/4260/solutions" target="_blank"><img src="https://img.shields.io/badge/-Check%20out%20Solutions-de5a77?logo=awesome-lists&logoColor=white" alt="Check out Solutions"/></a> <!--info-footer-end-->

## 答案
```ts
type StringToUnion<S extends string, U extends string = ''> = S extends `${infer A}${infer B}` ? StringToUnion<B, A | U> : U 

type Combinations<
  S extends string,
  U extends string = '',
  K = S
> = [S] extends [never] 
  ? U 
  : (
    K extends S 
    ? Combinations<Exclude<S, K>, U | `${U}${K}`> 
    : never // 这里随意设置什么值，程序走不到这
  )

type AllCombinations<S extends string> = Combinations<StringToUnion<S>>
```
1. 首先将字符串转化为联合，例如将 `'AB'` 转为 `'A' | 'B'`
2. 然后将转化后的联合自己 extends 自己，也就是代码中的 `K extends S`，起到分发的作用。

这里用一个示例 `AllCombinations<'AB'>` 来描述上面的程序如何执行：
1. `StringToUnion` 将 `'AB'` 转为 `'A' | 'B'`
2. `K extends S ` 也就是 `'A' | 'B'` extends `'A' | 'B'`，这里由于分发作用：

```ts
Combinations<Exclude<S, K>, U | `${U}${K}`> // `${U}${K}` 也会有分发作用
```

这段代码会执行两次。把代码用真实的值代替来执行一下，第一次是：
```
1. Combinations<Exclude<'A' | 'B', 'A'>, '' | 'A'>
2. Combinations<Exclude<'B', 'B'>, '' | 'A' | 'AB' | 'B'>
3. '' | 'A' | 'AB' | 'B'
```
第二次：
```
1. Combinations<Exclude<'A' | 'B', 'B'>, '' | 'B'>
2. Combinations<Exclude<'A', 'A'>, '' | 'B' | 'BA' | 'A'>
3. '' | 'B' | 'BA' | 'A'
```
两次执行的结果联合起来，也就是 `'' | 'A' | 'AB' | 'B'` | `'' | 'B' | 'BA' | 'A'`，最后结果为 `'' | 'A' | 'AB' | 'B' | 'BA'`