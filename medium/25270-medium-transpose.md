<!--info-header-start--><h1>Transpose <img src="https://img.shields.io/badge/-medium-d9901a" alt="medium"/> <img src="https://img.shields.io/badge/-%23array-999" alt="#array"/> <img src="https://img.shields.io/badge/-%23math-999" alt="#math"/></h1><blockquote><p>by Apollo Wayne <a href="https://github.com/Shinerising" target="_blank">@Shinerising</a></p></blockquote><p><a href="https://tsch.js.org/25270/play" target="_blank"><img src="https://img.shields.io/badge/-Take%20the%20Challenge-3178c6?logo=typescript&logoColor=white" alt="Take the Challenge"/></a> </p><!--info-header-end-->

The transpose of a matrix is an operator which flips a matrix over its diagonal; that is, it switches the row and column indices of the matrix A by producing another matrix, often denoted by A<sup>T</sup>.

```ts
type Matrix = Transpose <[[1]]>; // expected to be [[1]]
type Matrix1 = Transpose <[[1, 2], [3, 4]]>; // expected to be [[1, 3], [2, 4]]
type Matrix2 = Transpose <[[1, 2, 3], [4, 5, 6]]>; // expected to be [[1, 4], [2, 5], [3, 6]]
```

<!--info-footer-start--><br><a href="../../README.md" target="_blank"><img src="https://img.shields.io/badge/-Back-grey" alt="Back"/></a> <a href="https://tsch.js.org/25270/answer" target="_blank"><img src="https://img.shields.io/badge/-Share%20your%20Solutions-teal" alt="Share your Solutions"/></a> <a href="https://tsch.js.org/25270/solutions" target="_blank"><img src="https://img.shields.io/badge/-Check%20out%20Solutions-de5a77?logo=awesome-lists&logoColor=white" alt="Check out Solutions"/></a> <!--info-footer-end-->

## 答案

```ts
type Transpose<M extends number[][], Row = M['length'] extends 0 ? [] : M[0]> = {
  [X in keyof Row]: { // 新矩阵的行数
    [Y in keyof M]: X extends keyof M[Y] ? M[Y][X] : never // 新矩阵的列数
  }
}
```

将矩阵 M 的行列互换。

1. `Row = M['length'] extends 0 ? [] : M[0]` 将 M 第一项的数组长度作为新矩阵的行数
2. 将 M 的长度作为新矩阵中每一项的列数
3. `X extends keyof M[Y] ? M[Y][X] : never` 表示 X 索引是否存在于 `M[Y]` 中
