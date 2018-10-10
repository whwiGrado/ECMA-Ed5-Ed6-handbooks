# Object.create(proto, [propertiesObject])
## 参数
### proto
新创建对象的原型对象。

### propertiesObject
可选。如果没有指定为 undefined，则是要添加到新创建对象的可枚举属性（即其自身定义的属性，而不是其原型链上的枚举属性）对象的属性描述符以及相应的属性名称。这些属性对应Object.defineProperties()的第二个参数。

## 返回值
一个新对象，带着指定的原型对象和属性。

## 异常处理
如果propertiesObject参数不是 null 或一个对象，则抛出一个 TypeError 异常。

## Polyfill 实现
前提：
* 第一个参数 proto 不能为 null
* 第二个参数 propertiesObject 不做实现
```javascript
if (typeof Object.create !== 'function') {
  Object.create = function (proto, propertiesObject) {
    if (typeof proto !== 'object' && typeof proto !== 'function') {
      throw new TypeError('Object prototype may only be an Object: ' + proto);
    } else if (proto === null) {
      /*
      * 尽管在 ES5 中 Object.create支持设置为[[Prototype]]为null
      * 但因为那些ECMAScript5以前版本限制，此 polyfill 无法支持该特性
      */
      throw new Error('This browser\'s implementation of Object.create is a shim and doesn\'t support \'null\' as the first argument.');
    }

    if (typeof propertiesObject !== 'undefined') {
      /*
      * 此 polyfill 并未处理 propertiesObject 参数
      */
      throw new Error('This browser\'s implementation of Object.create is a shim and doesn\'t support a second argument.');
    }

    function F() {}
    F.prototype = proto;

    return new F();
  };
}
```

# 与 new 操作符的区别
## 先来看一些 new 操作符的定义与实现
### 使用 new 操作符调用构造函数实际上会经历以下4个步骤：
1. 创建一个新对象
2. 将构造函数的作用域赋给新对象（因此 this 就指向了这个对象）
3. 执行构造函数中的代码（为这个新对象添加属性）
4. 返回新对象
### 模拟实现一个 newFunc 函数来模拟 new 操作符
```javascript
function newFunc (constructor){
    var o = {}; // 创建一个新对象
    o.__proto__ = constructor.prototype; // 绑定构造函数的原型
    constructor.apply(o, Array.prototype.slice.call(arguments, 1)); // 将构造函数的作用域赋给新对象（因此 this 就指向了这个对象）
    return o; // 返回新对象
}
```
### 校验 newFunc 的实现
```javascript
function Person(name, sex) {
   this.name = name;
   this.sex = sex;
}
Person.prototype.getInfo = function() {
   console.log('getInfo: [name:' + this.name + ', sex:' + this.sex + ']');
}

var personA = new Person('john', 'female');
var personB = newFunc(Person, 'jack', 'male');
console.log(personA instanceof Person) // true
console.log(personB instanceof Person) // true
```


function Adult(age) {
 this.age = age;
}
Adult.prototype.say = function(language) { 
    console.log('you say ' + language);
}
