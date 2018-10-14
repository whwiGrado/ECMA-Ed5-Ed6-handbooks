![prototype](/static/images/prototype.jpg)

1. 所有对象都具有属性 __proto__ 可称为**隐式原型**，指向构造该对象的构造函数的原型（除了对象 Object.prototype）
```javascript
// 特殊情况
Object.prototype.__proto__ === null // true
```

2. 只有构造函数才特有的属性 prototype 可称为**原型对象**，此原型对象有一个 constructor 属性，指回原构造函数
