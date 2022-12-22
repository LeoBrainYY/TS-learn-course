# TypeScript类型

1. ### any类型

   ```javascript
   let message: any = '123' // 当进行类型断言
   
   message = 123 // 可以进行任意赋值
   message = {}
   
   ```

2. ### unknow类型

   ```typescript
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

   ```typescript
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

   

   ```typescript
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

   ```javascript
   多种元素的组合 
   // 数组的弊端 没有办却确切知道每一个元素的类型(如下写法)
   const arr1: any[] = ['123', 'qwe', 123]
   
   // 元组的特点 可以知道每一个元素的类型
   const info: [string, number, number] = ['123', 12, 123]
   const name = info[0]
   const age = info[1]
   
   ```

   应用场景

   ```typescript
   // 模仿react的useState学习一下tuple的应用场景
   // hook: useState
   // const [counter, setState] = useState(10) 传入一个参数 返回值第一个是传入的值(10) 第二个返回值是改变传入的值的函数
   // 我们写一个简单的函数去模仿他
   function useState (state: any) {
     let currentState = state
   
     const changeState = (newState: any) => {
       currentState = newState
     }
   
     // const arr: any[] = [currentState, changeState] // 这种写法 下面的counter setCounter获取到的都是any类型 我们想获取更精确的类型 以便操作
     const arr: [any, (newState: any) => void] = [currentState, changeState] //使用元组 定义返回值返回的就是一个函数类型 下面调用函数 就可以看见返回值就是函数类型
   
     return arr
   }
   
   const [counter, setCounter] = useState(10)
   const [title, setTitle] = useState('123')
   ```

   ```typescript
   优化:
   上述代码只是精确了函数的类型 对于传入的参数 返回值还是any类型 我们可以使用泛型进行优化 可以精确传入的参数返回值的类型
   
   // 模仿react的useState学习一下tuple的应用场景
   // hook: useState
   // const [counter, setState] = useState(10) 传入一个参数 返回值第一个是传入的值(10) 第二个返回值是改变传入的值的函数
   // 我们写一个简单的函数去模仿他
   function useState<T> (state: T) {
     let currentState = state
   
     const changeState = (newState: T) => {
       currentState = newState
     }
   
     // const arr: any[] = [currentState, changeState] // 这种写法 下面的counter setCounter获取到的都是any类型 我们想获取更精确的类型 以便操作
     const arr: [T, (newState: T) => void] = [currentState, changeState] //使用元组 定义返回值返回的就是一个函数类型 下面调用函数 就可以看见返回值就是函数类型
   
     return arr
   }
   
   const [counter, setCounter] = useState(10)
   const [title, setTitle] = useState('123')
   ```

   

6. 类型补充

   函数的参数

   ```typescript
   // 函数的参数类型和返回值
   // 开发中一般不写函数的返回值类型(自动推断)
   function sum (num1: number, num2: number): number {
     return num1 + num2
   }
   
   // 匿名函数的参数类型（Anonymous function
   // 通常在定义一个函数时，一般都会加上类型注解
   function foo (message: string) {
   
   }
   
   // 这里的函数可以不写类型注解
   const names = ['123', '456', '789']
   // item类型根据上下文进行推导
   // 上下文中的函数可以不加类型注解
   names.forEach(function(item) {
     console.log(item)
     
   })
   ```

   函数的参数(对象类型)

   ```typescript
   // point -> 对象类型
   // 加上一个问号就表示可选类型 可传可不传 不传就是undefined
   function printPoint (point: {x: number, y:number, z?: number}) {
     console.log(point.x)
     console.log(point.y)
     
   }
   
   printPoint({x: 12, y:34})
   printPoint({x: '12', y:34}) // Type 'string' is not assignable to type 'number'.
   ```

   联合类型(Union Type)

   ```typescript
   
   function printId (id: string | number | boolean) {
     // 使用联合类型需要注意
     // narrow
     if (typeof id === 'string') {
       console.log(id.toUpperCase())
     } else {
       console.log(id)
       
     }
   }
   
   printId('123')
   // 写一个把string字母写错 typescript的报错
   // This comparison appears to be unintentional 
   // because the types '"string" | "number" | "bigint" | "boolean" | "symbol" | "undefined" | "object" | "function"' and '"stirng"' have no overlap
   // 这种比较似乎是无意的，因为类型` "string" | "number" | "bigint" | "boolean" | "symbol" | "undefined" | "object" | "function" `和` "stirng" `没有重叠
   ```

   ```typescript
   // 一个参数本身是可选类型的时候 类似于联合类型的写法 类型|undefined 的联合类型
   // function foo (message?: string) {}
   function foo (message: string | undefined) {} //这个写法必须传入undefined 如果使用上面的写法 不传入参数 打印出来的也是undefined
   
   foo(undefined)
   
   ```

   类型别名

   ```typescript
   // type定义类型别名(type alias)
   type IdType = string | number | boolean
   type PointType = {
     x: number
     y: number
     z?: number
   }
   function pringId (id: IdType) {}
   function printPoint (point: PointType) {}
   
   ```

   类型断言

   ```javascript
   // <img id="xiaoxin"/> // 如果id是在img上面 我们不仅仅会想简单获取HTMLElement 我们就可以使用类型断言
   const el = document.getElementById('xiaoxin') as HTMLImageElement // el默认类型HTMLElement
   el.src='url'
   ```
   
   ```typescript
   class Person {
   
   }
   
   class student extends Person {
     studying() {}
   }
   
   function sayHello (p: Person) {
     // p.studying() // 直接这样调用会报错 编译器只知道我们传入的是一个Person类型
     (p as student).studying()
   }
   
   const stu = new student()
   
   sayHello(stu)
   ```
   
   ```typescript
   const message = 'abc'
   const num: number = (message as any) as number // 强转类型 不推荐使用
   ```
   
   非空类型断言
   
   ```typescript
   // message? -> undefined | string
   function printMessage (message?: string) {
   
       // console.log(message.length) // 'message' is possibly 'undefined'. 如果这样不做处理 代码编译不会通过
   
       // solution 1
       // if (message) {
       //   console.log(message.length)
       // }
   
       // solution 2
       console.log(message!.length) // 非空类型断言
   }
   
   printMessage('1231321')
   ```
   
   字面量类型
   
   ```typescript
   
   const message: 'Hello' = 'Hello' // Hello类型 字面量类型
   
   const num: 123 = 123 
   
   // 字面量类型的意义 结合联合类型
   type Alignment = 'left' | 'right' | 'center'
   let align: Alignment = 'left'
   align = 'right'
   // align= '1234' // Type '"1234"' is not assignable to type '"left" | "right" | "center"'
   
   ```
   
   字面量推理
   
   ```typescript
   
   const info = {
     name: 'xiaoxin',
     age: 18
   }
   
   info.name = 'luffy'
   
   
   type Method = 'GET' | 'POST'
   function request (url: string, method: Method) {}
   
   // solution 1
   // type Request = {
   //   url: string,
   //   method: Method
   // }
   
   // solution 3
   const options = {
     url: 'https://www.xiaoxin.org/abc',
     method: 'POST'
   } as const
   // as const类型推导的结果
   // const options: {
   //   readonly url: "https://www.xiaoxin.org/abc";
   //   readonly method: "POST";
   // }
   
   // solution 1
   // 推荐使用这种方法
   // const options: Request = {
   //   url: 'https://www.xiaoxin.org/abc',
   //   method: 'POST'
   // } as const
   
   request(options.url, options.method) // Argument of type 'string' is not assignable to parameter of type 'Method'
   
   // solution 2 类型断言
   // request(options.url, options.method) // Argument of type 'string' is not assignable to parameter of type 'Method'
   // request(options.url, options.method as Method)
   // 不对options加类型限制 类型推导推导出来options的method是string类型
   
   ```
   
   类型缩小(**Type narrowing**)
   
   1. ##### 在typescript中，检查返回的值type of是一种类型保护。TS对如果typeof操作不同的值进行编码
   
   ```typescript
   
   // 1.typeof
   type IDType = number | string
   function printID (id: IDType) {
     if (typeof id === 'string') {
       console.log(id.toUpperCase()) // (method) String.toUpperCase(): string
     } else {
       console.log(id) // (parameter) id: number
     }
   }
   
   // 2. === == !== 
   type Direction = 'left' | 'right' | 'top' | 'bottom'
   function printDirection (direction: Direction) {
     console.log(direction) // (parameter) direction: Direction // Type inference(类型推导)
     if (direction === 'left') {
       console.log(direction)
     } 
   }
   
   // 3. instanceof
   function printTime (time: string | Date) {
     if (time instanceof Date) {
       console.log(time.toUTCString())
     } else {
       console.log(time)
     }
   }
   
   class Student {
     studying() {}
   }
   
   class Teacher {
     teaching() {}
   }
   
   function work (p: Student | Teacher) {
     if (p instanceof Student) {
       p.studying
     } else {
       p.teaching()
     }
   }
   
   // 4. in
   type Fish = {
     swimming: () => void
   }
   
   type Dog = {
     running: () => void
   }
   
   // 要求传入一个对象函数
   function walk (animal: Fish | Dog) {
     if ('swimming' in animal) {
       animal.swimming() // (parameter) animal: Fish
     } else {
       animal.running() // (parameter) animal: Dog
     }
   }
   
   // 定义要传入的对象函数(字面量对象)
   const fish: Fish = {
     swimming () {
       console.log('swimming')
     }
   }
   
   walk(fish)
   ```
   
   