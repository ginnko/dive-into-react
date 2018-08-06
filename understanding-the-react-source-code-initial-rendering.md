
## 关于react的挂载

### 一些概念

**挂载的官方定义**：是初始化React组件的过程。该过程通过创建组件所代表的DOM元素，并将它们插入到提供的container中来实现。

**挂载的解释（见[参考1][bogdan-lyashenko.github.io]）**：将`组件描述（高阶描述）`转换为`HTML（低阶数据）`并将其放入DOM中的过程。这个过程要处理所有的属性，事件监听，内嵌组件以及逻辑。

**虚拟dom（见[参考1][bogdan-lyashenko.github.io]）**：首先虚拟DOM是一个概念，react中没有脚本叫做`virtual dom`。作者认为虚拟dom等于下面三个类：

  - `ReactCompositeComponent`
  - `ReactDOMComponent`
  - `ReactDOMTextComponent`

### 常量说明

1. **_ DEV _：** 定义在`/src/jest/setupEnviroment.js`中。类型为一个字符串，值是`development`，定义在全局对象`global`上的一个属性

### 涉及到的文件

1. `src/isomorphic/classic/element/ReactElement.js`
2. `/src/renderers/dom/client/ReactMount.js`
3. `src/renderers/shared/stack/reconciler/instantiateReactComponent.js`

### 脚本文件

  1. **ReactElement.js**

      - 辅助函数

          `hasValidRef`、`hasValidKey`：

          验证`ref`和`key`是否合法。主要是验证是否存在`ref`和`key`这两个属性以及针对这两个属性是否有报错

          ```javascript
          //首先解析了一下

          const hasOwnProperty = Object.prototype.hasOwnProperty;

          //条件判断
          //要是我来写肯定就写成config.hasOwnProperty('ref')这种形式了，感觉用源码的形式是从效率的角度考虑，可以便面多次解析


          hasOwnProperty.call(config, 'ref')
          ```


          `defineKeyPropWarningGetter`、`defineRefPropWarningGetter`：

          给`ref`、`key`设置warning的地方，合法检查的判断条件之一的`isReactWarning`也是在这儿设置的。

      - 核心函数

        `ReactElement`

        返回一个对象，~~这个对象就是虚拟dom~~，（这个对象不是虚拟DOM）主要的属性包含（见[参考3][掘金]）：

        |属性名称|值|
        |-------|--|
        |$$typeof|Symbol.for('react.element')|
        |type|字符串类型，组件名称|
        |key|DOM结构标识，提升update性能|
        |ref|真实组件的引用|
        |props|子结构相关信息(有则增加children字段/没有为空)和组件属性(如style)|
        |owner|_owner === ReactCurrentOwner.current(ReactCurrentOwner.js),值为创建当前组件的对象，默认值为null。|
        |||

        最后用`Object.freeze`使得这个对象以及对象的`props`不可以被修改。

        `createElement`

        这个函数做了三件事（见[参考2][holmeshe.me]）：

          1. 将`config`中的属性拷贝到`props`；
          2. 将`children`拷贝到`props.children`中；
          3. 将`type.defaultProps`中的属性拷贝进`props`中。

        然后将处理好的变量，传入`ReactElement`函数~~创建一个虚拟dom，并~~返回这个对象。

  2. **ReactMount.js**

      - 核心函数

        `_renderSubtreeIntoContainer`（见[参考3][掘金]）

        - 参数

          |参数|功能|
          |----|---|
          |parentComponent|当前组件的父组件，第一次渲染时为null|
          |nextElement|要插入DOM中的组件|
          |container|要插入的容器，如`document.getElementById('root')`|
          |callback|完成后的回调函数|
          |||

### 挂载过程

  1. 在编译阶段，babel就利用`ReactElement.createElement`将`JSX`转换为`React Element`，所以传入`ReacDOM.render`中的是`React Element`。

### 参考资料

  1. [bogdan-lyashenko.github.io](https://bogdan-lyashenko.github.io/Under-the-hood-ReactJS/stack/languages/chinese/book/Part-0.html)

  2. [holmeshe.me](https://holmeshe.me/understanding-react-js-source-code-initial-rendering-I/)

  3. [掘金](https://juejin.im/post/5983dfbcf265da3e2f7f32de)



  [bogdan-lyashenko.github.io]: https://bogdan-lyashenko.github.io/Under-the-hood-ReactJS/stack/languages/chinese/book/Part-0.html "参考1"

  [holmeshe.me]: https://holmeshe.me/understanding-react-js-source-code-initial-rendering-I/ "参考2"

  [掘金]: https://juejin.im/post/5983dfbcf265da3e2f7f32de "参考3"
