<!--info-header-start--><h1>JSON Schema to TypeScript <img src="https://img.shields.io/badge/-medium-d9901a" alt="medium"/> <img src="https://img.shields.io/badge/-%23JSON-999" alt="#JSON"/></h1><blockquote><p>by null <a href="https://github.com/aswinsvijay" target="_blank">@aswinsvijay</a></p></blockquote><p><a href="https://tsch.js.org/26401/play" target="_blank"><img src="https://img.shields.io/badge/-Take%20the%20Challenge-3178c6?logo=typescript&logoColor=white" alt="Take the Challenge"/></a> </p><!--info-header-end-->

Implement the generic type JSONSchema2TS which will return the TypeScript type corresponding to the given JSON schema.

Additional challenges to handle:

* additionalProperties
* oneOf, anyOf, allOf
* minLength and maxLength

<!--info-footer-start--><br><a href="../../README.md" target="_blank"><img src="https://img.shields.io/badge/-Back-grey" alt="Back"/></a> <a href="https://tsch.js.org/26401/answer" target="_blank"><img src="https://img.shields.io/badge/-Share%20your%20Solutions-teal" alt="Share your Solutions"/></a> <a href="https://tsch.js.org/26401/solutions" target="_blank"><img src="https://img.shields.io/badge/-Check%20out%20Solutions-de5a77?logo=awesome-lists&logoColor=white" alt="Check out Solutions"/></a> <!--info-footer-end-->

## 答案

```ts
type SimpleType = {
  string: string,
  number: number,
  boolean: boolean,
  object: Record<string, unknown>
  array: unknown[]
}

type HandleSimpleType<T, Type> = T extends { enum: unknown[] } 
? T['enum'][number] 
: Type extends keyof SimpleType 
  ? SimpleType[Type] 
  : never

type Copy<T> = {
  [K in keyof T]: T[K]
}

type HandleProperties<T, Required extends string[] = []> = Copy<{
  [K in keyof T as K extends Required[number] ? K : never]: JSONSchema2TS<T[K]>
} & {
  [K in keyof T as K extends Required[number] ? never : K]?: JSONSchema2TS<T[K]>
}>

type JSONSchema2TS<T> = T extends { properties: infer Properties }
? (T extends { required: infer Required extends string[] } ? HandleProperties<Properties, Required> : HandleProperties<Properties>)
: T extends { items: infer Array }
  ? JSONSchema2TS<Array>[]
  : T extends { type: infer Type }
    ? HandleSimpleType<T, Type>
    : never
```

先将要处理的类型分为简单、复杂类型，带有 `properties` 的对象和带有 `items` 的数组为复杂类型，其他的为简单类型。
然后 `JSONSchema2TS` 就可以简单的分为三步了了，前两步处理复杂类型的对象和数组，如果不是这两个类型，最后一步就可以用简单类型对象 `SimpleType` 返回映射类型就可以了。

```ts
type SimpleType = {
  string: string,
  number: number,
  boolean: boolean,
  object: Record<string, unknown>
  array: unknown[]
}
```

接下来再看一下复杂数组类型，可以发现 `items` 里对应的数组子项类型对象，其实是可以递归的用 `JSONSchema2TS` 处理的，所以这里可以这样处理：

```ts
T extends { items: infer Array }
  ? JSONSchema2TS<Array>[]
  : ...
```

最后再看一下复杂对象类型，这里需要处理 `required` 和 `properties`，`required` 是一个字符串数组，表示 `properties` 中的哪些属性是必须的，所以这里需要用到一个条件类型，将 `properties` 中的属性分为必须和可选两种类型，然后再将两种类型合并为一个对象类型。这里比较重要的代码是：

```ts
type Copy<T> = {
  [K in keyof T]: T[K]
}

type HandleProperties<T, Required extends string[] = []> = Copy<{
  [K in keyof T as K extends Required[number] ? K : never]: JSONSchema2TS<T[K]>
} & {
  [K in keyof T as K extends Required[number] ? never : K]?: JSONSchema2TS<T[K]>
}>
```

`HandleProperties` 将 `properties` 中的属性根据 `Required` 分为必须和可选两种类型，但是这样处理后的类型代码是这样的：

```ts
{ a: number } & { b?: string }
```

不符合要求，所以要将这样的代码进行合并，这里用 `Copy` 来处理，经过它处理后，上面的示例会变成：

```ts
{
    a: number,
    b?: string
}
```
