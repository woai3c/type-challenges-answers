<!--info-header-start--><h1>Diff <img src="https://img.shields.io/badge/-medium-d9901a" alt="medium"/> <img src="https://img.shields.io/badge/-%23object-999" alt="#object"/></h1><blockquote><p>by ZYSzys <a href="https://github.com/ZYSzys" target="_blank">@ZYSzys</a></p></blockquote><p><a href="https://tsch.js.org/645/play" target="_blank"><img src="https://img.shields.io/badge/-Take%20the%20Challenge-3178c6?logo=typescript&logoColor=white" alt="Take the Challenge"/></a> &nbsp;&nbsp;&nbsp;<a href="./README.zh-CN.md" target="_blank"><img src="https://img.shields.io/badge/-%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87-gray" alt="简体中文"/></a> </p><!--info-header-end-->

Get an `Object` that is the difference between `O` & `O1`


<!--info-footer-start--><br><a href="../../README.md" target="_blank"><img src="https://img.shields.io/badge/-Back-grey" alt="Back"/></a> <a href="https://tsch.js.org/645/answer" target="_blank"><img src="https://img.shields.io/badge/-Share%20your%20Solutions-teal" alt="Share your Solutions"/></a> <a href="https://tsch.js.org/645/solutions" target="_blank"><img src="https://img.shields.io/badge/-Check%20out%20Solutions-de5a77?logo=awesome-lists&logoColor=white" alt="Check out Solutions"/></a> <!--info-footer-end-->

## 答案
```ts
type Diff<O extends Record<PropertyKey, any>, O1 extends Record<PropertyKey, any>> = {
  [
    key in keyof O | keyof O1 // 遍历 O O1 key 的合集
    as ( // 查看 key 是否同时存在于 O O1
      key extends keyof O 
      ? (key extends keyof O1 ? never :key) 
      : key
    )
  ]: key extends keyof O ? O[key] : O1[key]
}
```
发现一个更简历的答案
```ts
type Diff<O, O1> = {[ key in keyof (O & O1) as key extends keyof (O | O1) ? never : key ]: (O & O1)[key] }
```

```ts
type Foo = {
  name: string
  age: string
}
type Bar = {
  name: string
  age: string
  gender: number
}

type a = keyof (Foo | Bar) // "name" | "age" 这里是与操作，在联合里应该是或操作，这里反过来了
type b = keyof (Foo & Bar) // "name" | "age" | "gender" 这里是或操作，在联合里应该是与操作，这里反过来了
```