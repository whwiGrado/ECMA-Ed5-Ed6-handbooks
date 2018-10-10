# Module 语法
ES6 的模块自动采用严格模式，不管你有没有在模块头部加上 "use strict";

ES6 模块的设计思想是尽量的静态化，使得编译时就能确定模块的依赖关系，以及输入和输出的变量。CommonJS 和 AMD 模块，都只能在运行时确定这些东西。比如，CommonJS 模块就是对象，输入时必须查找对象属性。
```javascript
// CommonJS模块
let { stat, exists, readFile } = require('fs');

// 等同于
let _fs = require('fs');
let stat = _fs.stat;
let exists = _fs.exists;
let readfile = _fs.readfile;
```
上面代码的实质是整体加载fs模块（即加载fs的所有方法），生成一个对象（_fs），然后再从这个对象上面读取 3 个方法。这种加载称为“运行时加载”，因为只有运行时才能得到这个对象，导致完全没办法在编译时做“静态优化”。

除了静态加载带来的各种好处，ES6 模块还有以下好处。
1. 不再需要UMD模块格式了，将来服务器和浏览器都会支持 ES6 模块格式。
2. 将来浏览器的新 API 就能用模块格式提供，不再必须做成全局变量或者navigator对象的属性。
3. 不再需要对象作为命名空间（比如Math对象），未来这些功能可以通过模块提供。

## export 用法
export命令规定的是对外的接口，必须与模块内部的变量建立一一对应关系。
```javascript
// 下列输出是合法的
export var firstName = 'Michael';

export function multiply(x, y) {
  return x * y;
};

function f() {}
export { f };

var lastName = 'Jackson';
var year = 1958;
export { lastName, year };

function v1() { ... }
function v2() { ... }
export {
  v1 as streamV1,
  v2 as streamV2,
  v2 as streamLatestVersion
};

// 下列输出是不合法的
export 1; // 不是接口，不能直接输出值

var m = 1;
export m; // 不是接口，不能直接输出值

function foo() {
  export default 'bar' // SyntaxError
}
foo() // 处在块级作用域内，不在顶级作用域里
```
export语句输出的接口，与其对应的值是动态绑定关系，即通过该接口，可以取到模块内部实时的值
```javascript
export var foo = 'bar';
setTimeout(() => foo = 'baz', 500);
// 上面代码输出变量foo，值为bar，500 毫秒之后变成baz。

// 这一点与 CommonJS 规范完全不同。CommonJS 模块输出的是值的缓存，不存在动态更新
```

## export default 用法
export default 命令规定的是对外的默认方法，本质是将后面的值，赋给default变量
```javascript
// 下列输出是合法的
export default 1;

var m = 1;
export default m;

// 下列输出是不合法的
export default var firstName = 'Michael';
```

## import 用法
* import 命令具有提升效果，是编译阶段执行的，在代码运行之前
```javascript
// 不会报错
foo();
import { foo } from 'my_module';
```

* import 命令是静态执行，所以不能使用表达式和变量，这些只有在运行时才能得到结果的语法结构
```javascript
// 报错
import { 'f' + 'oo' } from 'my_module';

// 报错
let module = 'my_module';
import { foo } from module;

// 报错
if (x === 1) {
  import { foo } from 'module1';
} else {
  import { foo } from 'module2';
}
```

* import 进来的变量是只读的，直接赋值会报错，但改写对象的属性是允许的
```javascript
import { foo } from 'my_module'

foo = {}; 
// Syntax Error : foo is read-only;

foo.name = 'something';
// 不会报错
```

* import 整体加载到一个对象，是可以静态分析的，所以不允许运行时改变
```javascript
import * as circle from './circle';

// 下面两行都是不允许的
circle.foo = 'hello';
circle.area = function () {};
```

* import 命令是单例模式
```javascript
import { foo } from 'my_module';
import { bar } from 'my_module';

// 等同于
import { foo, bar } from 'my_module';
```

* import 变量重命名
```javascript
import { foo as myFoo } from 'my_module';
//

export { foo as default } from 'my_module';
//
```

* 目前阶段，通过 Babel 转码，CommonJS 模块的require命令和 ES6 模块的import命令，可以写在同一个模块里面，但是最好不要这样做，因为import在静态解析阶段执行，所以它是一个模块之中最早执行的

## export 与 import 的复合写法
* 有复合写法的 import 命令总结
```javascript
// 下面的 import 语句有 export 复合写法，具体写法以及复合写法与正常写法的区别见下文
import { someIdentifierA, someIdentifierB } from 'someModule';
import { someIdentifierA as mySomeIdentifierA } from 'someModule';
import { someIdentifierA as default } from 'someModule';
import * from 'someModule';
import { default } from 'someModule';
import { default as mySomeModule } from 'someModule';

// 下面的 import 语句没有 export 复合写法
import * as someIdentifier from 'someModule';
import someIdentifier from 'someModule';
import someIdentifier, { namedIdentifier } from 'someModule';
```

* 具体接口组合输出
```javascript
export { foo, bar } from 'my_module';
// 等同于
import { foo, bar } from 'my_module';
export { foo, bar };
```
上面代码中，export和import语句可以结合在一起，写成一行。但需要注意的是，写成一行以后，foo和bar实际上并没有被导入当前模块，只是相当于对外转发了这两个接口，导致当前模块不能直接使用foo和bar

* 具体接口改名输出
```javascript
// 改为具体接口（换名）输出
export { foo as myFoo } from 'my_module';

// 改为默认接口输出
export { foo as default } from 'my_module';
// 等同于
import { foo } from './my_module';
export default foo;
```

* 整体输出
```javascript
export * from 'my_module';
```

* 默认接口输出
```javascript
export { default } from 'my_module';
```

## 模块的继承

```javascript
export * from 'circle';
export var e = 2.71828182846;
export default function(x) {
  return Math.exp(x);
}
```
上面代码中的export *，表示再输出circle模块的所有属性和方法。注意，export *命令会忽略circle模块的default方法。然后，上面代码又输出了自定义的e变量和默认方法。

## 跨模块常量
```javascript
// 跨模块输出
// constants/db.js
export const db = {
  url: 'http://my.couchdbserver.local:5984',
  admin_username: 'admin',
  admin_password: 'admin password'
};

// constants/user.js
export const users = ['root', 'admin', 'staff', 'ceo', 'chief', 'moderator'];

// 统一入口
// constants/index.js
export {db} from './db';
export {users} from './users';

// 使用统一入口
import {db, users} from './constants/';
```

## import() 实现 dynamic import

[万岁，浏览器原生支持ES6 export和import模块啦！](https://www.zhangxinxu.com/wordpress/2018/08/browser-native-es6-export-import-module/)

[翻译 | 原生 ECMAScript 模块：动态 import（）](https://juejin.im/entry/58ba3308a22b9d005ede7565)
