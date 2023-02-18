## 1.声明

```typescript

// 通过类型别名来声明对象类型
// type InfoType = { name: string, age: number }

// 通过接口方式来声明对象类型
// 可以定义可选类型
// 也可以定义只读属性

interface InfoType {
  name: string
  age: number
  friend?: {
    name: string
  }
}
const obj: InfoType = {
  name: '123',
  age: 18,
  friend: {
    name: 'test'
  }
}
```

## 2.索引类型

```typescript
// 想让类似于下面的对象的类型更加规范 key是number value是string
interface IndexLanguage {
  [index: number]: string
}

const frontLanguage: IndexLanguage = {
  0: 'HTML',
  1: 'JavaScript',
  2: 'NUXT'
}

```

## 3.函数类型

```typescript
// 利用类型别名
// type CalFn = (n1: number, n2: number) => number

// 利用接口
interface CalFn {
  (n1: number, n2: number): number
}

function calc (num1: number, num2: number, calcFn: CalFn) {
  return calcFn(num1, num2)
}

const add: CalFn = (num1, num2) => {
  return num1 + num2
}

console.log(calc(20, 30, add))
```

## 4.接口的继承

```typescript
interface ISwim {
  swimming: () => void
}

interface IFly {
  flying: () => void
}


interface IAction extends ISwim, IFly {

}

const action: IAction = {
  swimming() {

  },
  flying() {
    
  }
}

```

## 5.交叉类型

```typescript
// 一种组合类型的方式: 联合类型
type WhyType = number | string
type Direction = "left" | "right" | "center"


// 另一种组件类型的方式: 交叉类型
type WType = number & string

interface ISwim {
  swimming: () => void
}

interface IFly {
  flying: () => void
}

type MyType1 = ISwim | IFly
type MyType2 = ISwim & IFly

const obj1: MyType1 = {
  flying() {

  }
}

const obj2: MyType2 = {
  swimming() {

  },
  flying() {
    
  }
}
```

## 6.接口的实现

```typescript
interface ISwim {
  swimming: () => void
}

interface IEat {
  eating: () => void
}

// 继承只能实现单继承
// 可以实现多个接口 接口中的方法必须被实现
class Fish implements ISwim, IEat {
  eating () {
    console.log(123)
    
  }
  swimming () {
    console.log('swimming')
  }
  
}

const fish = new Fish()
fish.swimming()
```

```typescript
用途：面向接口编程 API更具通用性
// 继承只能实现单继承
// 可以实现多个接口 接口中的方法必须被实现
class Fish implements ISwim, IEat {
  eating () {
    console.log(123)
    
  }
  swimming () {
    console.log('swimming')
  }
  
}

class Person implements ISwim {
  swimming: () => void
}

const fish = new Fish()
fish.swimming()

function swimAction (swimable: ISwim) {
  swimable.swimming()
}

swimAction(new Person())
// 使用接口这里也可以单独传入一个对象 如果写成FIsh 那么传入对象 必须还要实现eating方法
swimAction({ swimming: function() {}})
```

## 7. interface和type的区别

```
interface可以重复的对某个接口来定义属性和方法
而type定义的是别名 别名是不能重复的
```

```
https://www.typescriptlang.org/docs/handbook/2/everyday-types.html
Official document
```

## 8.字面量赋值

```typescript
interface Person {
  name: string
  age: number
  height: number
}

const info = {
  name: '123',
  age: 18,
  height: 1.88,
  address: 'test'
}

const p: Person = info // 这里不报错 当赋值的为对象的引用时 ts内部会进行freshness(擦除)操作 并不是真的删除了这个属性 而是在类型推导的时候不去忽略他
// 这个属性也是不可读的

// 报错
// const p: Person = {
//   name: '123',
//   age: 18,
//   height: 1.88,
//   address: 'test'
// }
```

## 9.枚举类型

```typescript
// 枚举类型: 将一组可能出现的值，一个个列举出来，定义在一个类型中，这个类型就是枚举类型
// 枚举允许开发者定义一组命名常量 可以是数字 字符串类型

enum Direction {
  LEFT,
  RIGHT,
  TOP,
  BOTTOM
}

function turnDurection (direction: Direction) {
  switch (direction) {
    case Direction.LEFT:
      console.log('TO LEFT')
      break;
    case Direction.RIGHT:
      console.log('TO RIGHT')
      break;
    case Direction.LEFT:
      console.log('TO LEFT')
      break;
    case Direction.TOP:
      console.log('TO TOP')
      break;
      case Direction.BOTTOM:
      console.log('TO BOTTOM')
      break;
      // 写上never之后 必须实现全部的枚举 才不会报错
    default:
      const foo: never = direction
  }
}

turnDurection(Direction.LEFT)
```

## 10.泛型

```typescript
// 类型的参数化
// 定义函数时，不决定这个参数的类型，而是让调用者决定参数的类型
function sum<T> (num1: T): T { 
  return num1
}

sum<number>(1)
sum<{name: string}>({name: 'test'})
sum<any[]>([])
```

```
sum(50) // 这样传入会自动进行类型推导
```

## 10.1 泛型接受类型参数

```typescript
function foo<T, E> (arg1: T, arg2: E) {

}

foo<string, number> ('123', 123)
```

## 10.2 泛型接口

```typescript
interface IPerson<T1, T2> {
  name: T1
  age: T2
}

const p: IPerson<string, number> = {
  name: '213',
  age: 123
}

// 需要注意的时泛型接口不会自动进行类型推导 所以要么写上默认类型 要么在使用接口的时候声明类型
interface IPerson<T1 = number, T2 = string> {
  name: T1
  age: T2
}
```

## 10.3 泛型类

```typescript
class Point<T> {
  x: T
  y: T
  z: T

  constructor (x: T, y: T, z: T) {
    this.x = x
    this.y = y
    this.z = z
  }
}

const p = new Point('1.23', '1.23', '1.23')
const p1 = new Point<string>('1.23', '1.23', '1.23')
const p2: Point<string> = new Point('1.23', '1.23', '1.23')
```

## 10.4 泛型的类型约束

```typescript
interface ILength {
  length: number
}

function getLength<T extends ILength> (arg: T) {
  return arg.length
}

getLength('123')
getLength(['abc', '123'])
getLength({ length: 0 })

```

