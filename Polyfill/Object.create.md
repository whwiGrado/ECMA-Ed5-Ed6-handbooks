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
