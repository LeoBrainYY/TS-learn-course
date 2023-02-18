# 1. ts中类的使用

## 1.类的定义

```typescript
class Person {
  name: string
  age: number

  constructor (name: string, age: number) {
    this.name = name
    this.age = age
  }

  eating () {
    console.log(this.name + 'eating')
  }
}

// 传入的参数会传到构造器中
const p = new Person('xiaoxin', 18)
console.log(p.name)
console.log(p.age)
p.eating()
```

## 2.类的继承

```typescript
class Person {
  name: string = ""
  age: number = 0

  eating() {
    console.log("eating")
  }
}

class Student extends Person {
  sno: number = 0

  studying() {
    console.log("studying")
  }
}

class Teacher extends Person {
  title: string = ""

  teaching() {
    console.log("teaching")
  }
}

const stu = new Student()
stu.name = "coderwhy"
stu.age = 10
console.log(stu.name)
console.log(stu.age)
stu.eating()


```

```typescript
class Person {
  name: string
  age: number

  constructor(name: string, age: number) {
    this.name = name
    this.age = age
  }

  eating() {
    console.log("eating 100行")
  }
}

class Student extends Person {
  sno: number

  constructor(name: string, age: number, sno: number) {
    // super调用父类的构造器 此处的super必须写在this.sno = sno之前
    super(name, age)
    this.sno = sno
  }

  // 重写父类方法
  eating() {
    console.log("student eating")
    // 调用父类方法
    super.eating()
  }

  studying() {
    console.log("studying")
  }
}

const stu = new Student("why", 18, 111)
console.log(stu.name)
console.log(stu.age)
console.log(stu.sno)

stu.eating()
```

## 3.多态

```typescript
class Animal {
  action () {
    console.log('animal running')
  }
}

class Dog extends Animal {
  action () {
    console.log('dog running')
  }
}

class Fish extends Animal {
  action () {
    console.log('fish swimming')    
  }
}

// 使用重载也可以实现
// function makeActions(animals: Dog[])
// function makeActions(animals: Fish[])
// function makeActions (animals: (Dog | Fish)[]) {
//   animals.forEach(item => {
//     // 执行重写之后的方法
//     item.action()
//   })
// }

function makeActions (animals: Animal[]) {
  animals.forEach(item => {
    // 执行重写之后的方法
    item.action()
  })
}

// 父类引用指向子类对象
// Animal animal = new Dog()

makeActions([new Dog(), new Fish()])
// makeActions([new Fish(), new Dog()])

// 多态的目的是为了写出更具通用性的代码
```

## 4.成员的修饰符

```typescript
class Person {
  private name: string = ''

  getName () {
    return this.name
  }

  setName (newName: string) {
    this.name = newName
  }
}

const p = new Person()
p.setName('xiaoxin')
console.log(p.getName())
```

```typescript
class Person {
  protected name: string = '123'
}

class Student extends Person {
  getName () {
    return this.name
  }
}

const stu = new Student()
console.log(stu.getName()) // 123
```

## 5.只读属性

```typescript
class Person {
  // 只读属性可以在构造器中赋值
  // 属性本身不能进行修改，但是如果他是对象类型，对象中的值是可以修改的
  readonly name: string
  age?: number
  readonly friend?: Person
  constructor (name: string, friend?: Person) {
    this.name = name
    this.friend = friend
  }
}

const p = new Person('xiaoxin', new Person('the one'))
console.log(p.name)
console.log(p.friend?.name)
if (p.friend) {
  p.friend.age = 30
}

// 不可以直接修改friend
// p.friend = new Person('test') // Cannot assign to 'friend' because it is a read-only property.ts
```

## 6. getters-setters

```typescript
class Person {
  // 私有属性命名一般加_
  private _name: string
  constructor (name) {
    this.name = name
  }

  // 访问器
  // setter
  set name (newName) {
    this._name = newName
  }

  // getter
  get name () {
    return this._name
  }
}

const p = new Person('xiaoxin')
p.name = 'xxx'
console.log(p.name)
```

## 7.类的静态成员

```typescript
// 可以直接通过类名访问 也叫做类属性或者类方法
class Student {
  static time: string = '8:00'
}

console.log(Student.time) // 8:00
```

## 8.抽象类

```typescript

// 在定义很多通用接口时。我们通常会让调用者传入父类，通过多态来实现更加灵活的调用方式
// 但是，父类本身可能并不需要对某些方法进行具体的实现，所以父类中的方法，我们可以定义为抽象方法

function makeArea (shape: Shape) {
  return shape.getArea()
}

// 抽象类不能实例化
// 抽象类中的抽象方法必须被子类实现
// 否则该子类必须是一个抽象类 那么该子类就不会能被实例化
abstract class Shape {
  abstract getArea(): number // 规定返回值类型
}

class Rectangle extends Shape {
  private width: number
  private height: number

  constructor (width: number, height: number) {
    super()
    this.width = width
    this.height = height
  }

  getArea() {
    return this.width * this.height
  }
}

class Circle extends Shape {
  private radius: number

  constructor (radius: number) {
    super()
    this.radius = radius
  }

  getArea() {
    return this.radius * this.radius * 3.14
  }
}
const a = new Rectangle(10, 20)
console.log(makeArea(a))
console.log(makeArea(new Circle(10)))

```

## 9. 类的类型

```typescript
class Person {
  name: string = '123'
  eating () {

  }
}

const p1: Person = {
  // 类中的属性和这里的属性相互对应
  name: 'test',
  eating () {

  }
}

function printPerson (person: Person) {
  console.log(person.name)
}

// 两种传参方式
printPerson(new Person())
// 也可以直接传入一个对象
printPerson({ name: 'test', eating() {} })
```

