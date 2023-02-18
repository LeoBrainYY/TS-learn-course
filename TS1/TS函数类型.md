# 1.函数

##### 1.函数的类型

```typescript
// 1.函数作为参数 
function foo () {}

type fooType = () => void

function bar (fn: fooType) {
  fn()
}

bar(foo)

// 2.定义常量的函数类型
type addFnType = (num1: number, num2: number) => number
const add: addFnType = (num1: number, num2: number) => {
  return num1 + num2
}


// 3.函数类型的案例
function calc (n1: number, n2: number, fn: (num1: number, num2: number) => number) {
    return fn(n1, n2)
}

const result1 = calc(10, 20, function(a1, a2) {
  return a1 + a2
})
console.log(result1) // 30

const result2 = calc(10, 20, function(n1, n2) {
  return n1 * n2
})
console.log(result2)
```

##### 2. 参数的可选类型

```typescript
// 可选类型参数必须写在必选类型参数后面
// y -> undefinde | number
function foo (x: number, y?: number) {

}

foo(1)
```

##### 3.参数的默认值

```typescript
function foo (x: number = 20, y: number) {
  console.log(x, y)
  
}

foo(undefined, 20) // 如果有默认值的参数写在第一个，可以传undefined 就会默认使用默认值 
// 一般建议把有默认值的参数写在后面
```

##### 4.剩余参数

```typescript
function sum (initialNum: number, ...nums: number[]) {
  let total = initialNum  
  for (const num of nums) {
    total = total + num
  }
  return total
}

console.log(sum(20, 30))
console.log(sum(20, 30, 40))
console.log(sum(20, 30, 40, 50))
```



##### 5. this相关

```typescript
// this是可以被推导出来 info对象(TypeScript推导出来)
const info = {
  name: "why",
  eating() {
    console.log(this.name + " eating")
  }
}

info.eating() // why eating
```

```typescript
type ThisType = { name: string };

function eating(this: ThisType, message: string) {
  console.log(this.name + " eating", message);
}

const info = {
  name: "why",
  eating: eating,
};

// 隐式绑定
info.eating("哈哈哈"); // why eating 哈哈哈

// 显示绑定
eating.call({name: "kobe"}, "呵呵呵") // kobe eating 呵呵呵
eating.apply({name: "james"}, ["嘿嘿嘿"]) // james eating 嘿嘿嘿
```

##### 6.函数的重载

```typescript
/**
 * 想写一个函数相加两个参数的值
 * 通过联合类型有两个缺点:
 *  1.进行很多的逻辑判断(类型缩小)
 *  2.返回值的类型依然是不能确定
 */
function add(a1: number | string, a2: number | string) {
  if (typeof a1 === "number" && typeof a2 === "number") {
    return a1 + a2
  } else if (typeof a1 === "string" && typeof a2 === "string") {
    return a1 + a2
  }

  // return a1 + a2;
}

add(10, 20)
```

```typescript

// 函数的重载
// 名称相同 参数不同的几个函数 就是函数的重载
function add(num1: number, num2: number): number // Function implementation is missing or not immediately following the declaration.
function add(num1: string, num2: string): string

// 会执行下面的函数体
function add (num1: any, num2: any): any {
  if (typeof num1 === 'string' && typeof num2 === 'string') {
    return num1.length + num2.length
  }
  return num1 + num2
}

const result = add(20, 30) // const result: number
const result1 = add('abc', '123') // const result1: string
console.log(result, result1) // 50 6


// 在函数的重载中 实现函数是不能直接被调用的
// ts会先去定义的函数里面匹配类型 类型匹配才回去执行实现函数
// add({age: 18}, {name: xiaoxin}) // No overload matches this call.

```

##### 7.练习

```typescript
// solution 1
function getLength (args: string | any[]) {
  return args.length
}

console.log(getLength('poiuy')) // 5
console.log(getLength(['123', 1, 3])) // 3

// solution 2
function getLengthOverLoad (args: string) : number
function getLengthOverLoad (args: any[]) : number
function getLengthOverLoad (args: any) {
  return args.length
}

console.log(getLengthOverLoad('abcde')) // 5
console.log(getLengthOverLoad(['123', 1, 3])) // 3
```

