
## 关于react的挂载

### 一些概念

**挂载的官方定义**：是初始化React组件的过程。该过程通过创建组件所代表的DOM元素，并将它们插入到提供的container中来实现。

**挂载的解释**：将`组件描述（高阶描述）`转换为`HTML（低阶数据）`并将其放入DOM中的过程。这个过程要处理所有的属性，事件监听，内嵌组件以及逻辑。

**虚拟dom（见[参考1][使用react-v15.4.2]）**：首先虚拟DOM是一个概念，react中没有脚本叫做`virtual dom`。作者认为虚拟dom等于下面三个类：

  - `ReactCompositeComponent`
  - `ReactDOMComponent`
  - `ReactDOMTextComponent`

### 常量说明

1. **_ DEV _：** 定义在`/src/jest/setupEnviroment.js`中。类型为一个字符串，值是`development`，定义在全局对象`global`上的一个属性

### 涉及到的文件

1. `src/isomorphic/classic/element/ReactElement.js`
2. `/src/renderers/dom/client/ReactMount.js`

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

      - 主函数

        `ReactElement`

        返回一个对象，~~这个对象就是虚拟dom~~，（这个对象不是虚拟DOM）主要的属性包含：

        |属性名称|值|
        |-------|--|
        |$$typeof|Symbol.for('react.element')|
        |type|字符串类型，组件名称|
        |key||
        |ref||
        |props||
        |owner||
        |||

        最后用`Object.freeze`使得这个对象以及对象的`props`不可以被修改。

        `createElement`

        这个函数做了三件事：

          1. 将`config`中的属性拷贝到`props`；
          2. 将`children`拷贝到`props.children`中；
          3. 将`type.defaultProps`中的属性拷贝进`props`中。

        然后将处理好的变量，传入`ReactElement`函数~~创建一个虚拟dom，并~~返回这个对象。

### 挂载过程

  1. 在编译阶段，babel就利用`ReactElement.createElement`将`JSX`转换为`React Element`，所以传入`ReacDOM.render`中的是`React Element`。

### 参考资料

  1. [使用react-v15.4.2](https://bogdan-lyashenko.github.io/Under-the-hood-ReactJS/stack/languages/chinese/book/Part-0.html)

  2. [使用react-v15.6.2](https://holmeshe.me/understanding-react-js-source-code-initial-rendering-I/)



  [使用react-v15.4.2]: https://bogdan-lyashenko.github.io/Under-the-hood-ReactJS/stack/languages/chinese/book/Part-0.html "参考1"

  [使用react-v15.6.2]: https://holmeshe.me/understanding-react-js-source-code-initial-rendering-I/ "参考2"
