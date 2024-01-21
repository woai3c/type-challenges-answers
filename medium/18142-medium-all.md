<!--info-header-start--><h1>All <img src="https://img.shields.io/badge/-medium-d9901a" alt="medium"/> <img src="https://img.shields.io/badge/-%23array-999" alt="#array"/></h1><blockquote><p>by cutefcc <a href="https://github.com/cutefcc" target="_blank">@cutefcc</a></p></blockquote><p><a href="https://tsch.js.org/18142/play" target="_blank"><img src="https://img.shields.io/badge/-Take%20the%20Challenge-3178c6?logo=typescript&logoColor=white" alt="Take the Challenge"/></a> </p><!--info-header-end-->

Returns true if all elements of the list are equal to the second parameter passed in, false if there are any mismatches.

For example

```ts
type Test1 = [1, 1, 1]
type Test2 = [1, 1, 2]

type Todo = All<Test1, 1> // should be same as true
type Todo2 = All<Test2, 1> // should be same as false
```

<!--info-footer-start--><br><a href="../../README.md" target="_blank"><img src="https://img.shields.io/badge/-Back-grey" alt="Back"/></a> <a href="https://tsch.js.org/18142/answer" target="_blank"><img src="https://img.shields.io/badge/-Share%20your%20Solutions-teal" alt="Share your Solutions"/></a> <a href="https://tsch.js.org/18142/solutions" target="_blank"><img src="https://img.shields.io/badge/-Check%20out%20Solutions-de5a77?logo=awesome-lists&logoColor=white" alt="Check out Solutions"/></a> <!--info-footer-end-->

## 答案

### 答案一

```ts
type All<T extends unknown[], Target> = T extends [infer A, ...infer B]
? `${A}` extends `${Target}` // 转为字符串是为了比较 any 和 unknown
  ? `${Target}` extends `${A}` // 两次比较是为了比较 1 和 1|2 这种 
    ? [A] extends [Target] // 比较 never，需要用 [] 包起来
      ? All<B, Target> 
      : false
    : false
  : false
: true
```

### 答案二

```ts
type IsEqual<X, Y> = (<T>() => T extends X ? 1 : 2) extends (<T>() => T extends Y ? 1 : 2) 
? true 
: false;

type All<T extends unknown[], Target> = T extends [infer A, ...infer B]
? IsEqual<A, Target> extends true
  ? All<B, Target>
  : false
: true
```

两个答案是差不多的，区别在于判断相等的方法。

```ts
type IsEqual<X, Y> = (<T>() => T extends X ? 1 : 2) extends (<T>() => T extends Y ? 1 : 2) 
? true 
: false;
```

上面的代码为什么能正确判断两个类型是否相等，主要是因为代码里声明的两个自执行箭头函数，ts 会尝试推断一个类型 `T`，使得两个函数类型能够相互兼容。

由于 `T` 是在函数类型内部声明的，它可以被看作是一个“待定”的类型，ts 会在类型检查过程中探索所有可能的 `T` 来确定两个条件类型是否总是产生相同的结果。
如果对于所有可能的 `T`，这两个函数的返回类型都是相同的，那么可以认为 `X` 和 `Y` 是相等的，因此 `IsEqual<X, Y>` 类型会解析为 `true`。
