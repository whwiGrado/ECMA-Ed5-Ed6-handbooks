# 构造函数的继承
两个对象都是构造函数，有下列五种方法实现继承。
```
function Animal(scale){
  this.scale = scale;
}
Animal.prototype.species = "动物";

function Cat(name,color){
  this.name = name;
  this.color = color;
}
```
## 方法一：构造函数绑定
```javascript
function Cat(name,color){
  Animal.apply(this, arguments);
}
```

## 方法二：子类 prototype 赋值为父类的实例（有缺陷）
```javascript
Cat.prototype = new Animal();
Cat.prototype.constructor = Cat;
```
缺点：
* 建立 Animal 的实例需要占用额外内存

## 方法三：子类 prototype 赋值为父类 prototype（有缺陷）
```javascript
Cat.prototype = Animal.prototype;
Cat.prototype.constructor = Cat;
```
缺点：
* Cat.prototype 和 Animal.prototype现在指向了同一个对象，那么任何对 Cat.prototype 的修改，都会反映到 Animal.prototype

## 方法四：空函数中介模式
```javascript
function extend(Sub, Super) {
  var F = function(){};
  F.prototype = Super.prototype;
  Sub.prototype = new F();
  Sub.prototype.constructor = Sub;
  // 通过 Sub.uber 属性直接访问 Super 的原型，属于可选备用属性
  Sub.uber = Super.prototype;
}
extend(Cat, Animal);
```

## 方法五：使用深拷贝将父类 prototype 拷贝到子类 prototype 上
```javascript
function extend(Sub, Super) {
  var superP = Super.prototype;
  var subP = Sub.prototype;
  // 使用 jQuery 的深拷贝方法 extend
  jQuery.extend(true, subP, superP);
}
```

# 非构造函数的继承
两个对象都是普通对象，不是构造函数，无法使用构造函数方法实现"继承"，有两种方法实现继承。
```javascript
var Chinese = {
  nation:'中国'
};
var Doctor ={
  career:'医生'
}
```
## 方法一：空函数中介模式
```javascript
function extend(o) {
  function F() {}
  F.prototype = o;
  return new F();
}
var Doctor = extend(Chinese);
Doctor.career = '医生';
```

## 方法二：使用深拷贝将被继承对象的属性拷贝到目标对象上
```javascript
// 使用 jQuery 的深拷贝方法 extend
jQuery.extend(true, Doctor, Chinese);
```



