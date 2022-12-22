# 1. ts中类的使用

##### 1.类的定义

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

##### 2.类的继承

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

