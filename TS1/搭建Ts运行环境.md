# 1. TS数据类型

首先需要了解，单文件时(进行练习时创建的一个一个ts文件， 还不太清楚在项目中的情况)，ts的所有文件默认都是在同一个作用域下编译的。如果一个变量名在其他文件下定义了，在当前文件重复定义就会报错。

```javascript
const name: string = '123' 
// Cannot redeclare block-scoped variable 'name'
```

意思就是说不能重复定义变量，需要这样操作一下，报错就会消除。就是会把当前一个ts文件当作单独的作用域。需要注意的是ts代码文件后缀就会是ts。

```javascript
const name: string = '123' 

export {}
```

# 2.搭建运行TS代码环境

通过单文件操作的时候，运行测试ts代码的时候步骤会比较麻烦。我们西先安装一个库，主要目的就是为了节省时间。

```
 npm install ts-node -g
 npm install tslib @types/node -g
```

安装完成之后，运行代码

```
ts-node (filename)
```

