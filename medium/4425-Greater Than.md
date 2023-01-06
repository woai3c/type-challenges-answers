<!--info-header-start--><h1>Greater Than <img src="https://img.shields.io/badge/-medium-d9901a" alt="medium"/> <img src="https://img.shields.io/badge/-%23array-999" alt="#array"/></h1><blockquote><p>by ch3cknull <a href="https://github.com/ch3cknull" target="_blank">@ch3cknull</a></p></blockquote><p><a href="https://tsch.js.org/4425/play" target="_blank"><img src="https://img.shields.io/badge/-Take%20the%20Challenge-3178c6?logo=typescript&logoColor=white" alt="Take the Challenge"/></a> </p><!--info-header-end-->

In This Challenge, You should implement a type `GreaterThan<T, U>` like `T > U`

Negative numbers do not need to be considered.

For example

```ts
GreaterThan<2, 1> //should be true
GreaterThan<1, 1> //should be false
GreaterThan<10, 100> //should be false
GreaterThan<111, 11> //should be true
```

Good Luck!

<!--info-footer-start--><br><a href="../../README.md" target="_blank"><img src="https://img.shields.io/badge/-Back-grey" alt="Back"/></a> <a href="https://tsch.js.org/4425/answer" target="_blank"><img src="https://img.shields.io/badge/-Share%20your%20Solutions-teal" alt="Share your Solutions"/></a> <a href="https://tsch.js.org/4425/solutions" target="_blank"><img src="https://img.shields.io/badge/-Check%20out%20Solutions-de5a77?logo=awesome-lists&logoColor=white" alt="Check out Solutions"/></a> <!--info-footer-end-->

## 答案 
```ts
type GreaterThan<
  T extends number, 
  U extends number, 
  TL extends number[] = [], 
  UL extends number[] = []
> = T extends U 
  ? false
  : (
    TL['length'] extends T 
    ? false 
    : (UL['length'] extends U ? true : GreaterThan<T, U, [...TL, 1], [...UL, 1]>)
  )
```
两个数对比，只有三种情况：
1. `T = U`
2. `T > U`
3. `T < U`

第一种情况直接用 `T extends U` 来判断就可以得出结果，如果相等直接返回结果 `false`。

剩下的两种情况需要靠 `TL` `UL` 来判断，用两个初始长度为 0 的数组来表示。首先，在每一轮的比较中，`TL` `UL` 的值都是相等的，因为它们每次都是同步 + 1。
那么在判断他们不相等后，再对它们进行比较时。由于这两个值相等，那么 `T` 和 `U`，谁的值和这两个数组的值一样，那它的值就是偏小的那一个。`TL` `UL` 在每一轮的比较中都是最小值之一，因为它们是从零开始递增的。

用 `GreaterThan<1, 0>` 这个实际例子来描述一下执行过程：
1. 在第一轮比较中，`TL` `UL` 的值为 0。
2. 由于 `T` 和 `L` 不相等，那么它们中的任意一个和 `TL` `UL` 的值相等，即为较小数。不可能是较大的那一个。

同理，再用其他例子 `GreaterThan<0, 1>` `GreaterThan<2, 1>` `GreaterThan<3, 4>` 代入运算一下，也能得到正常的结果。