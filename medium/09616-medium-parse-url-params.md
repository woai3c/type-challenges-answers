<!--info-header-start--><h1>Parse URL Params <img src="https://img.shields.io/badge/-medium-d9901a" alt="medium"/> <img src="https://img.shields.io/badge/-%23infer-999" alt="#infer"/> <img src="https://img.shields.io/badge/-%23string-999" alt="#string"/> <img src="https://img.shields.io/badge/-%23template--literal-999" alt="#template-literal"/></h1><blockquote><p>by Anderson. J <a href="https://github.com/andersonjoseph" target="_blank">@andersonjoseph</a></p></blockquote><p><a href="https://tsch.js.org/9616/play" target="_blank"><img src="https://img.shields.io/badge/-Take%20the%20Challenge-3178c6?logo=typescript&logoColor=white" alt="Take the Challenge"/></a> </p><!--info-header-end-->

You're required to implement a type-level parser to parse URL params string into an Union.

```ts
ParseUrlParams<':id'> // id
ParseUrlParams<'posts/:id'> // id
ParseUrlParams<'posts/:id/:user'> // id | user
```

<!--info-footer-start--><br><a href="../../README.md" target="_blank"><img src="https://img.shields.io/badge/-Back-grey" alt="Back"/></a> <a href="https://tsch.js.org/9616/answer" target="_blank"><img src="https://img.shields.io/badge/-Share%20your%20Solutions-teal" alt="Share your Solutions"/></a> <a href="https://tsch.js.org/9616/solutions" target="_blank"><img src="https://img.shields.io/badge/-Check%20out%20Solutions-de5a77?logo=awesome-lists&logoColor=white" alt="Check out Solutions"/></a> <!--info-footer-end-->

## 答案

```ts
type ParseUrlParams<T> = T extends `${any}:${infer A}` 
? A extends `${infer B}/${infer Rest}` 
  ? B | ParseUrlParams<Rest> 
  : A
: never
```

1. 先粗后细，先判断是否符合 `:id` 规则，再细化判断是否符合 `:id/:id` 规则
2. 如果不符合后面细化的规则，则可以直接返回 A，因为 A 不能再细化了，所以只有 A 是 id
