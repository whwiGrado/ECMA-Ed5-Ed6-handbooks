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
```javascript
if (typeof Object.create !== 'function') {
  Object.create = function (proto, propertiesObject) {
    if (typeof proto !== 'object' && typeof proto !== 'function') {
      throw new TypeError('Object prototype may only be an Object: ' + proto);
    } else if (proto === null) {
      /*
      * 尽管在 ES5 中 Object.create支持设置为[[Prototype]]为null，但因为那些ECMAScript5以前版本限制，此 polyfill 无法支持该特性
      */
      throw new Error('This browser\'s implementation of Object.create is a shim and doesn\'t support \'null\' as the first argument.');
    }

    if (typeof propertiesObject != 'undefined') {
      throw new Error('This browser\'s implementation of Object.create is a shim and doesn\'t support a second argument.');
    }

    function F() {}
    F.prototype = proto;

    return new F();
  };
}
```
