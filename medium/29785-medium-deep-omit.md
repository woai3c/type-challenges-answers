<!--info-header-start--><h1>Deep Omit <img src="https://img.shields.io/badge/-medium-d9901a" alt="medium"/> <img src="https://img.shields.io/badge/-%23omit%20object--keys%20deep-999" alt="#omit object-keys deep"/></h1><blockquote><p>by bowen <a href="https://github.com/jiaowoxiaobala" target="_blank">@jiaowoxiaobala</a></p></blockquote><p><a href="https://tsch.js.org/29785/play" target="_blank"><img src="https://img.shields.io/badge/-Take%20the%20Challenge-3178c6?logo=typescript&logoColor=white" alt="Take the Challenge"/></a> </p><!--info-header-end-->

Implement a type`DeepOmit`, Like Utility types [Omit](https://www.typescriptlang.org/docs/handbook/utility-types.html#omittype-keys), A type takes two arguments.

For example:

```ts
type obj = {
  person: {
    name: string;
    age: {
      value: number
    }
  }
}

type test1 = DeepOmit<obj, 'person'>    // {}
type test2 = DeepOmit<obj, 'person.name'> // { person: { age: { value: number } } }
type test3 = DeepOmit<obj, 'name'> // { person: { name: string; age: { value: number } } }
type test4 = DeepOmit<obj, 'person.age.value'> // { person: { name: string; age: {} } }
```

<!--info-footer-start--><br><a href="../../README.md" target="_blank"><img src="https://img.shields.io/badge/-Back-grey" alt="Back"/></a> <a href="https://tsch.js.org/29785/answer" target="_blank"><img src="https://img.shields.io/badge/-Share%20your%20Solutions-teal" alt="Share your Solutions"/></a> <a href="https://tsch.js.org/29785/solutions" target="_blank"><img src="https://img.shields.io/badge/-Check%20out%20Solutions-de5a77?logo=awesome-lists&logoColor=white" alt="Check out Solutions"/></a> <!--info-footer-end-->

## 答案

```ts
type DeepOmit<T, Key extends string> = T extends Record<string, any> 
? {
    [K in keyof T as K extends Key ? never : K]: Key extends `${infer A}.${infer Rest}` // 分解 person.age.value 前缀
      ? K extends A  // 判断当前 key 和 person.age.value 中的 person 是否相等，相等才能进行下一步，否则 Key 传入一个 abc.age 也能通过测试用例，仓库中提供的测试用例没有考虑这一点
        ? DeepOmit<T[K], Rest> // 前缀 person 如果相等，递归判断子对象，这时可以把前缀 person. 去掉，把剩下的 age.value 传入下一次递归
        : T[K] 
      : T[K]
  }
: T
```
