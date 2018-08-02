## [DIY virtual DOM](https://github.com/gimenete/ui-state-sync)

1. 先说下这个仓库使用的第三方库来进行创建虚拟节点、将虚拟节点转化成DOM节点、比较两个虚拟节点以及更新DOM节点使用的函数

    1. h：创建虚拟节点，也就是js标准对象；
    2. createElement：将虚拟节点转成DOM节点；
    3. 使用DOM的api添加上述生成的DOM；
    4. diff：比较两个虚拟节点生成patches；
    5. patch（2出生成的DOM节点，patches），通过这样调用这个函数来更新DOM节点。

2. 分析下Component的初始化函数中的this.tree的值

    ```javascript
    this.tree = h('div', null)

        name = 'div'
        children = []
        props = {}
        attrs = [attributes: {}]

    dom('div', [attributes: {}], [])

    //dom函数就是上面说的h，用来生成虚拟节点（js标准对象）
    //这个虚拟节点就是this.tree的值
    ```

3. 进入`index.js`中，在这个文件的开头引入了`h`。感觉h的使用不仅是直接调用，在`render`方法中`return`的各种`html`的tag依然能被识别而不报错，这就和react的功能一样。感觉这都是h的效果。

4. 初始化分析

    ```javascript
    this.root = obj[id = addressList] //定义包含节点  

    this.tree = dom('div', [attributes: {}], [])
    this.rootNode = createElement(this.tree)
    this.root.appendChild(this.rootNode) //创建虚拟节点并添加进dom中  

    this.state = {addresses: []} //获取初始状态

    this.update()

    <!-- 
    1. 比较render返回的虚拟dom节点和上一次的虚拟dom
    2. 将差异更新到createElement创建的DOM节点上
    3. 将this.tree更新为本次的render返回的虚拟节点
     -->

     this.componentDidMount()

     <!-- 
     1. this.input = <input />
     2. 给this.root设置事件监听，将input输入的文字保存到this.state中并执行this.update()函数
     3. 添加点击事件，对this.state.addresses进行过滤
     -->
    ```

5. 三个方法的写法

    1. setState，update定义在最顶端的原型对象上

    2. render方法定义在实例对象的类上，实质就是返回一个虚拟节点

6. 更新机制

    执行setState函数，

    1. 更新this.state

    2. 执行update,update中获取render返回的对象，比较后更新

7. 问题

    不明白为何render返回的jsx能直接转成虚拟节点，感觉是经过了h函数的处理，但是怎么处理的？在update函数中使用console输出也没有反应？