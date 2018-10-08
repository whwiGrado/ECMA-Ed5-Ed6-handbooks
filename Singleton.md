# 单例模式

### es5 惰性单例实现

```javascript
// 单例构造函数
function CreateSingleton (name) {
    this.name = name;
    this.getName();
};

// 获取实例的名字
CreateSingleton.prototype.getName = function() {
    console.log(this.name)
};
// 单例对象
var Singleton = (function(){
    var instance;
    return function (name) {
        if(!instance) {
            instance = new CreateSingleton(name);
        }
        return instance;
    }
})();
```

### es6 惰性单例实现

```
// 负责创建DIV的基本功能
class CreateSingleton {
    constructor(name) {
        this.name = name;
        this.getName();
    }
    getName() {
        console.log(this.name)
    }
}
// 负责管理单例
class ProxysingletonCreateSingleton {
    constructor(name) {
        return ProxysingletonCreateSingleton.getInstance(name);
    }
    static getInstance(name) {
        if(!ProxysingletonCreateSingleton.instance) {
            ProxysingletonCreateSingleton.instance = new CreateSingleton(name)
        }
        return ProxysingletonCreateSingleton.instance;
    }
}
```
