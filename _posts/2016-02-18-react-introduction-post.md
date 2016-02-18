## React有什么特别之处？
不像Angular和Ember，使用双向绑定的方式去更新页面HTML，React的核心是组件。在我看来，React比Angular和Ember还要容易上手。它体积更小，和jquery还有其他框架能完美结合。它也非常快捷，因为它使用的是虚拟DOM，仅当页面改变时同步变化（众所周知，访问DOM是web app开发环节最影响效率的一环，因此很有必要提升这一方面的性能）

然而，另一方面，React要花费更多的代码来完成数据绑定，见如下的实例。

## 怎么使用React?
要使用React, 你需要在你的页面包含一个单独的javascript文件。Facebook友好地提供了可以直接访问的CDN
`<script src="http://fb.me/react-0.10.0.min.js"></script>`

你可以访问全局的React对象，它提供了许多有用的方法，具体看如下的实例。写React web app的推荐写法是使用一种将JSX的语法。JSX是一种改进版的javascript，允许用HTML形式的语法直接写React组件。它需要编译成javascript才能被浏览器解释。JSX是首选-你也选择可以用常规的javascript。
废话少说，上实例代码

1. 定时器

 正如我所提到的，react app是由组件构成。创建组件是通过 React.createClass()方法。每个组件都有状态（数据对象），通过render()方法完成自身的渲染，每当状态改变，render()方法都会被调用一次。如下是开发一个简单的定时器：
![](http://i11.tietuku.com/e7f89e8ba241394d.png)

[详细代码请点击] (http://codepen.io/edwinna/pen/BjEQpd)

当创建组件时你可以通过{}包含任意js表达式，以上实例中，我们传递了一个开始时间，每次tick()方法被调用时通过start来计算经过的时间。

2. 导航菜单

通过创建一个导航菜单来了解如何在React中处理点击事件
代码如下：
![](http://i11.tietuku.com/fa01aec1c1c8b1e5.png)

[详细代码请点击](http://codepen.io/edwinna/pen/WrWRQP)

[点击阅读更多](http://facebook.github.io/react/docs/jsx-in-depth.html)

3. 实时查询

人们都讨厌等待，这里是应用React来构建一个实时搜索实验。
代码如下：
![](http://i11.tietuku.com/237007abc52d8c4b.png)
![](http://i11.tietuku.com/49d4334122dcb381.png)
这个实例表明了React另一个重要的概念-更新控制层的不像我们想象中的不会影响到HTML。在这个实例中，忽略掉用户的输入，一旦你设置了文本框的值，文本框会保持不变，除非你通过onChange方法来更新文本框的值。

[详细代码请点击](http://codepen.io/edwinna/pen/zrXwKb)


4. 订单表单

React真正强大的地方在于结合多个组件。React通过引入分离关注点来优雅地组织代码，从而实现代码在不同web程序上的复用。如下是订单表单的实例，用以帮助用户规划预算。代码如下：
![](http://i13.tietuku.com/e1df533f2d58424c.png)
![](http://i13.tietuku.com/e0cc6f861c43b6b0.png)

当多个组件结合时，问题产生了，如何让多个组件通讯。一种方式是在组件初始化的时候把传递的数据写在属性上。这仅在父子组件中传递数据有效。在其他非父子关系的组件通讯中，你可以将父组件上的方法传递给子组件，作为子组件的属性。子组件自身可以调用该方法，并且激活父级上的事件。[点击阅读更多](http://facebook.github.io/react/tips/communicate-between-components.html)

[详细代码请点击](http://codepen.io/edwinna/pen/KVYrbw?editors=0110)

