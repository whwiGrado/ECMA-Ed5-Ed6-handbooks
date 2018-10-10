# 非构造函数的继承
## 方法一：构造函数绑定
```javascript
function Animal(scale){
  this.scale = scale;
}
Animal.prototype.species = "动物";

function Cat(name,color){
  Animal.apply(this, arguments);
  this.name = name;
  this.color = color;
}
```

## 方法二：子类 prototype 赋值为父类的实例（有缺陷）
```javascript
Cat.prototype = new Animal();
Cat.prototype.constructor = Cat;
```
缺点：
* 建立 Animal 的实例需要占用额外内存

## 方法二：子类 prototype 赋值为父类 prototype（有缺陷）
```javascript
Cat.prototype = Animal.prototype;
Cat.prototype.constructor = Cat;
```
缺点：
* Cat.prototype 和 Animal.prototype现在指向了同一个对象，那么任何对 Cat.prototype 的修改，都会反映到 Animal.prototype

## 方法三：空函数中介模式
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

## 方法四：深拷贝
```javascript

function extend(Sub, Super) {
  var superP = Super.prototype;
  var subP = Sub.prototype;
  jQuery.extend(true, subP, superP)
}
```



