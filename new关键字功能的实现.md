#### new关键字功能的实现



#### mdn定义

> 创建一个用户定义的对象类型的实例或具有构造函数的内置对象的实例，执行以下步骤

- 创建一个空对象
- 把这个对象的原型链链接到另一个对象的原型上
- 判断返回值，如果是对象则返回这个对象，否则返回this上下文

```js
// 解析new的一些情况

// 1.正常情况
function P (name, age) {
    this.name = name
    this.age = age
}
P.prototype.job = 'cv工程师'

var p = new P('w', 18)
p.name //w
p.age  //18
p.job  //'cv工程师'

// 2.返回值是对象
function P (name, age) {
    this.job = 'cv工程师'
    return {
        name:name,
        age:age
    }
}
P.prototype.like = 'girls'
var p = new P('w', 18)
p.name //w
p.age  // 18
p.job  // undefined
P.like // undefined


// 3.返回值不是对象
function P (name, age) {
    this.name = name
    this.age = age
    return 'girls'
}
P.prototype.like = 'girls'
var p = new P('w', 18)
p.name // w
p.age  // 18
P.like // 'girls'
```

#### myNew函数的实现

> 当返回值是对象且不是null时返回这个返回值，否则返回我们构建的obj

```js
function myNew (target, ...args) {
    var obj = {}
    obj.__proto__ = target.prototype
    let result = target.apply(obj, args)
    return (typeof result === 'object') && result !== null ? result : obj
}
```

