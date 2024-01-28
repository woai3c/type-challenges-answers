<!--info-header-start--><h1>Triangular number <img src="https://img.shields.io/badge/-medium-d9901a" alt="medium"/> <img src="https://img.shields.io/badge/-%23tuple-999" alt="#tuple"/> <img src="https://img.shields.io/badge/-%23array-999" alt="#array"/> <img src="https://img.shields.io/badge/-%23math-999" alt="#math"/></h1><blockquote><p>by null <a href="https://github.com/aswinsvijay" target="_blank">@aswinsvijay</a></p></blockquote><p><a href="https://tsch.js.org/27152/play" target="_blank"><img src="https://img.shields.io/badge/-Take%20the%20Challenge-3178c6?logo=typescript&logoColor=white" alt="Take the Challenge"/></a> </p><!--info-header-end-->

Given a number N, find the Nth triangular number, i.e. `1 + 2 + 3 + ... + N`

<!--info-footer-start--><br><a href="../../README.md" target="_blank"><img src="https://img.shields.io/badge/-Back-grey" alt="Back"/></a> <a href="https://tsch.js.org/27152/answer" target="_blank"><img src="https://img.shields.io/badge/-Share%20your%20Solutions-teal" alt="Share your Solutions"/></a> <a href="https://tsch.js.org/27152/solutions" target="_blank"><img src="https://img.shields.io/badge/-Check%20out%20Solutions-de5a77?logo=awesome-lists&logoColor=white" alt="Check out Solutions"/></a> <!--info-footer-end-->

## 答案

```ts
type Triangular<N extends number, Result extends number[] = [], Count extends number[] = []> = Count['length'] extends N
? Result['length']
: Triangular<N, [...Result, ...Count, 1], [...Count, 1]>
```

用 `Count` 记录每次相加的数字，用 `Result` 记录累加的和。
