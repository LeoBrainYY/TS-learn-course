# 1.JavaScript的类型检测

```javascript
function foo (message) {
  console.log(message.length)
}

foo('hello')
foo('world')
foo() // Cannot read property 'length' of undefined
foo(123)
```

如果这里的foo函数不传入参数 或者传入的 参数为**Number**类型，那么函数就会报错

1. JavsScript对传入的参数类型不校验
2. JavaScript对是否传入参数不进行校验

```javascript
function foo (message) {
  if (message) {
    console.log(message.length)
  }
}

foo(123) // undefined
foo('hello')
foo('world')
```

如果使用严格一点的写法，上述代码可以对于是否传入参数进行校验，但是对于数字类型会是**undefined**的结果
