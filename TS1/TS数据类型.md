# TypeScript类型

1. ### any类型

   ```javascript
   let message: any = '123' // 当进行类型断言
   
   message = 123 // 可以进行任意赋值
   message = {}
   
   ```

2. ### unknow类型

   ```javascript
   function foo () {
     return 'abc'
   }
   
   function bar () {
     return 123
   }
   
   // unknown类型只能赋值给any和unknown类型
   // any类型可以赋值给任意类型
   let flag = true
   let result: unknown
   // let result: any
   if (flag) {
     result = foo()
   } else {
     result = bar()
   }
   
   let message: string = result // Type 'unknown' is not assignable to type 'string
   // 如果result的类型为any 就可以赋值
   console.log(result)
   ```

3. void类型

   ```javascript
   function sum (num1:number, num2:number): void {
     console.log(num1 + num2)
   
     return undefined // 当函数没有返回值类型 void可写可不写 也可以返回undefined
     return null // Type 'null' is not assignable to type 'void'
   }
   
   sum(20, 30)
   ```

4. never类型

   ```javascript
   // 永远没有返回值
   function foo (): never {
     // Endless loop
     while (true) {
   
     }
   }
   
   // Throwing an exception
   function bar (): never {
     throw new Error()
   }
   ```

   

   ```javascript
   使用场景
   // encapsulation core function
   function handleMessage (message: number | string | boolean) {
     switch (typeof message) {
       case 'string':
         console.log('string handle');
         break
       case 'number':
         console.log('number handle');
         break
       // case 'boolean':
         // console.log('boolean handle');
         // break
       default:
         const check: never = message
     }
   }
   
   // 假设这个人看见这个核心函数 会在参数的地方这样加一个(message: number | string | boolean)
   // 但是函数内部没有处理boolean类型 所以写一个default 但是never永远无法赋值 所以就会出现如下报错
   // Type 'boolean' is not assignable to type 'never'.
   // 所以这个人就会知道函数内部需要添加boolean处理方法 并且去添加
   handleMessage(true) // Argument of type 'boolean' is not assignable to parameter of type 'string | number'
   ```

   

5. 元组(tuple)

   ```
   多种元素的组合
   ```

   

6. 