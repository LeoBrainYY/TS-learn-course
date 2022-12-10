# 1.认识TypeScript

##  1. 搭建TypeScript编译环境(tsc: TypeScript Cmpiler)   

TypeScript最终会被编译成JavaScript来运行

```javascript
npm install typescript -g 
// 命令行的方式全局安装
```

```javascript
tsc --version
// 键入之后 如果显示版本 则安装成功
```

#### 通过node运行ts代码时(单文件示例),需要先执行

```
tsc (filename)
```

#### 会形成一个新的js文件，然后再通过node

```
node (filename)
```

去执行文件即可