# Notes
## ECMAScript 新特性
* 解决原有语法上的一些问题和不足
* 增强原油语法
* 新内置对象，新方法，新功能
* 新数据类型和数据结构

### 新特性
* let, const, 块作用域
* 数组，对象解构
* 模板字符串
  * 函数标签传入arguments: string[],模板中插入的值0,模板中插入的值1,..
* string 方法
  * str.startWith()
  * str.endWith()
  * str.includes(str_0)
* default arguments
  * 传入 undefined 的时候匹配默认值
  * 默认值排在参数靠后
* 剩余参数
  * 放在参数最后, 返回数组
* 展开数组
  * `...[1,2,3]`
* 箭头函数
  * 箭头函数本身没有 `this`
* 对象字面量
```javascript
const obj = {
  bar,
  fn_name(){
  
  },
  [the_val]: 123
}
```

* `Object.assign()`
* `Object.is()`
  * `Object.is(+0,-0) // false`
  * `Object.is(NaN, NaN) // true`
* `Object.proxy`
  * 对对象操作进行接获处理
  * `Object.reflect`, 对 proxy 对象的默认实现
* `Promise`
* `class`, `static`, `extends`
* `Set`, `Map`, `Symbol`
* `for-of`, 使用 `break` 终止循环
* `Symbol.iterator` 实现 对象的 `for-of` 接口

```javascript
const obj = {
  store: [0,1,2],
  [Symbol.iterator]: function(){
    let index = 0
    let data = this.store
    
    return { // return a iterator
      next: function(){
        return {
          value: data[index],
          done: idnex++ >= data.length
        }
      }
    }
  }
}
```

```javascript
const todos = {
  life: ['eat', 'sleep', 'hit beans'],
  learn: ['English', 'Math', 'Computer'],
  
  [Symbol.iterator]: function *() {
    const all = [...this.life, ...this.learn]
    
    for(const item of all) {
      yied item
    }
  }
}
```

* module
* ES2016
  * `arr.includes(NaN)`
  * `const pow_val = 2 ** 10`
* ES2017
  * `Object.values(o)`
  * `Object.entries(o)`
  * `Object.getOwnPopertyDescriptors()`, 可用来复制对象属性, getter, setter 也可复制
  * `str.padStart(length, delimiter_char)` `str.padEnd(length, delimiter_char)`, 将字符串补齐到指定宽度
  * `async` `await`

## typescript
### concept
* 强类型: 不允许隐式类型转换;
  * 提前发现错误
  * 代码更智能，编码更准确
  * 重构更牢靠
  * 减少类型判断
* 弱类型;
  * 弱类型 没有编译错误, 只能等到执行时才发现; 类型不明确, 逻辑功能不确定;隐式转换导致语法不清晰;
* 静态类型: 变量声明后, 类型被定义且不可修改; 动态类型;

### flow
* 为变量添加类型注解
* 安装
  * `yarn add flow-bin --dev`
  * `// @flow`
  * `yarn flow init` // for configuration
  * `yarn flow` `yarn flow stop`
  
  * 移除类型注解的多余代码
    * `yarn add flow-remove-types --dev`
    * `yarn flow-remove-types . -d dist`
    
    * `yarn add @babel/core @babel/cli @babel/preset-flow --dev`
    * `.babelrc` <- `{ "presets": ["@babel/preset-flow"]}`
    * `yarn babel src -d dist`
    
  * 使用插件 `Flow language support`
  
### Typescript
### performance
* 回收算法
  * 引用计数
  * 标记清除
  * 标记整理
  * 增量标记
* V8 引擎
  * 分代回收
  * 新生代 和 老生代
* 内存性能检测工具
  * 任务管理器
  * timeline
  * memory tab -> heap snapshot; 查看 detached dom
* js 代码优化
  * 全局变量
  * 在作用域内 cache 全局变量
  * prototype 添加实例方法
  * 闭包 `closure_var = null`
  * 避免用对象方法来获取对象值
  * `for` 循环 cache length
  * `forEach` > `for` > `for-in`
  * 添加节点使用 `document.createDocumentFragment()` 作为容器, 一次性添加新 `dom`
  * 用已有节点添加相似节点时, `existed_dom.cloneNode()` 优于直接生成
  * 创建对象时, 直接量优于 `new` 生成的对象
### stack & heap
* 执行栈
  * EC execution context
    * scope chain
    * active object
* 减少判断层级
  * 将 `false` 条件提前, 减少层级
* 减少作用域查找层级
  * 层级越深, 块作用域越长
  * cache 外层变量
* 减少数据读取次数
  * 多次访问同一属性时, cache 对象内部数据
* 字面量与构造式
  * 使用对象字面量创建对象优于 `new` 创建对象
* 减少循环体内活动
  * cache length
  * `while(len--)`
* 减少声明及语句数, 语句合并, 减少执行环境解释语句时间
  * `var a = 1, b = 2;`
  * `return ele.offsetWidth * ele.offsetHeight`
* 惰性函数
```javascript
function addEvent(obj, type, fn) {
 if(obj.addEventListener) {
  addEvent = obj.addEventListener
  obj.addEventListener(type, fn, false)
 } else if (obj.attachEvent) {
  addEvent = obj.attachEvent
  obj.attachEvent('on' + type, fn)
 } else {
  addEvent = obj['on' + type] = fn
 }
}
```
* 事件绑定, 父 dom 代理 子 dom 事件
