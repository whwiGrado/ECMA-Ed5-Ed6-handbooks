ES6
export 用法

import 用法
- import 命令具有提升效果，是编译阶段执行的，在代码运行之前
// 不会报错
foo();
import { foo } from 'my_module';

- import 命令是静态执行，所以不能使用表达式和变量，这些只有在运行时才能得到结果的语法结构
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

- import 进来的变量是只读的，直接赋值会报错，但改写对象的属性是允许的
import { foo } from 'my_module'

foo = {}; 
// Syntax Error : foo is read-only;

foo.name = 'something';
// 不会报错

- import 整体加载到一个对象，是可以静态分析的，所以不允许运行时改变
import * as circle from './circle';

// 下面两行都是不允许的
circle.foo = 'hello';
circle.area = function () {};

- import 命令是单例模式
import { foo } from 'my_module';
import { bar } from 'my_module';

// 等同于
import { foo, bar } from 'my_module';

- import 变量重命名
import { foo as myFoo } from 'my_module';
//

export { foo as default } from 'my_module';
//

- 目前阶段，通过 Babel 转码，CommonJS 模块的require命令和 ES6 模块的import命令，可以写在同一个模块里面，但是最好不要这样做，因为import在静态解析阶段执行，所以它是一个模块之中最早执行的



export 与 import 的复合写法
- 写法总结

```
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

- ​具体接口组合输出
​export { foo, bar } from 'my_module';​
​// 等同于
import { foo, bar } from 'my_module';​
​export { foo, bar };​
上面代码中，​export​和​import​语句可以结合在一起，写成一行。但需要注意的是，写成一行以后，​foo​和​bar​实际上并没有被导入当前模块，只是相当于对外转发了这两个接口，导致当前模块不能直接使用​foo​和​bar​

- 具体接口改名输出
// 改为具体接口（换名）输出
export { foo as myFoo } from 'my_module';

// 改为默认接口输出
export { foo as default } from 'my_module';
// 等同于
import { foo } from './my_module';
export default foo;

- 整体输出
export * from 'my_module';

- 默认接口输出
export { default } from 'my_module';
