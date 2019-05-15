---
title: vue mixin
url: 181.html
id: 181
categories:
  - 未分类
date: 2019-01-17 19:13:54
tags:
---

原文出处： [SARAH DRASNER](https://css-tricks.com/using-mixins-vue-js/)   译文出处：[众成翻译](http://zcfy.cc/article/using-mixins-in-vue-js-css-tricks-3257.html)   

有一种很常见的情况：有两个非常相似的组件，他们的基本功能是一样的，但他们之间又存在着足够的差异性，此时的你就像是来到了一个分岔路口：我是把它拆分成两个不同的组件呢？还是保留为一个组件，然后通过props传值来创造差异性从而进行区分呢？

两种解决方案都不够完美：如果拆分成两个组件，你就不得不冒着一旦功能变动就要在两个文件中更新代码的风险，这违背了 DRY 原则。反之，太多的props传值会很快变得混乱不堪，从而迫使维护者（即便这个人是你）在使用组件的时候必须理解一大段的上下文，拖慢写码速度。

使用Mixin。Vue 中的Mixin对编写函数式风格的代码很有用，因为函数式编程就是通过减少移动的部分让代码更好理解（引自 [Michael Feathers](https://twitter.com/mfeathers/status/29581296216?lang=en) ）。Mixin允许你封装一块在应用的其他组件中都可以使用的函数。如果使用姿势得当，他们不会改变函数作用域外部的任何东西，因此哪怕执行多次，只要是同样的输入你总是能得到一样的值，真的很强大！

### 基础实例

我们有一对不同的组件，它们的作用是通过切换状态（Boolean类型）来展示或者隐藏模态框或提示框。这些提示框和模态框除了功能相似以外，没有其他共同点：它们看起来不一样，用法不一样，但是逻辑一样。

JavaScript

// 模态框 const Modal = { template: '#modal', data() { return { isShowing: false } }, methods: { toggleShow() { this.isShowing = !this.isShowing; } }, components: { appChild: Child } } // 提示框 const Tooltip = { template: '#tooltip', data() { return { isShowing: false } }, methods: { toggleShow() { this.isShowing = !this.isShowing; } }, components: { appChild: Child } }

1

2

3

4

5

6

7

8

9

10

11

12

13

14

15

16

17

18

19

20

21

22

23

24

25

26

27

28

29

30

31

32

33

34

35

// 模态框

const  Modal  =  {

template:  '#modal',

data()  {

return  {

isShowing:  false

}

},

methods:  {

toggleShow()  {

this.isShowing  =  !this.isShowing;

}

},

components:  {

appChild:  Child

}

}

// 提示框

const  Tooltip  =  {

template:  '#tooltip',

data()  {

return  {

isShowing:  false

}

},

methods:  {

toggleShow()  {

this.isShowing  =  !this.isShowing;

}

},

components:  {

appChild:  Child

}

}

我们可以在这里提取逻辑并创建可以被重用的项：

JavaScript

const toggle = { data() { return { isShowing: false } }, methods: { toggleShow() { this.isShowing = !this.isShowing; } } } const Modal = { template: '#modal', mixins: \[toggle\], components: { appChild: Child } }; const Tooltip = { template: '#tooltip', mixins: \[toggle\], components: { appChild: Child } };

1

2

3

4

5

6

7

8

9

10

11

12

13

14

15

16

17

18

19

20

21

22

23

24

25

26

27

28

const  toggle  =  {

data()  {

return  {

isShowing:  false

}

},

methods:  {

toggleShow()  {

this.isShowing  =  !this.isShowing;

}

}

}

const  Modal  =  {

template:  '#modal',

mixins:  \[toggle\],

components:  {

appChild:  Child

}

};

const  Tooltip  =  {

template:  '#tooltip',

mixins:  \[toggle\],

components:  {

appChild:  Child

}

};

你可以点击这里，查看 Sarah Drasner([@sdras](https://codepen.io/sdras)) 在[CodePen](https://codepen.io)上编写 [Mixin](https://codepen.io/sdras/pen/101a5d737b31591e5801c60b666013db/) 的例子

为了更容易理解Mixin，这个例子故意编写得简单一些。真实应用中Mixin有如下的应用，但是它的作用也不仅限于此：获取视窗和组件的尺寸，采集特定的鼠标事件和图表的基本元素。Paul Pflugradt 有一个关于 [Vue Mixins 的优秀项目](https://github.com/paulpflug/vue-mixins)，值得一提的是它是用 coffeescript 编写的。

### 用法

上面这个codepen的例子并没有告诉我们在一个真实的应用中如何使用Mixin，所以我们看看下面的这个。

你可以按照你喜欢的任意方式设置你的目录结构，但为了结构规整我喜欢新建一个`mixin`目录。我们创建的这个文件含有`.js`扩展名（跟`.vue`相对，就像我们的其他文件），为了使用Mixin我们需要输出一个对象。

![directory structure shows mixins in a folder in components directory](http://jbcdn2.b0.upaiyun.com/2017/07/df4bdb49356f53506124dbd7509a7195.jpg)

接着我们可以在Modal.vue使用这样的写法，来引入这个Mixin:

JavaScript

import Child from './Child' import { toggle } from './mixins/toggle' export default { name: 'modal', mixins: \[toggle\], components: { appChild: Child } }

1

2

3

4

5

6

7

8

9

10

import Child  from  './Child'

import  {  toggle  }  from  './mixins/toggle'

export  default  {

name:  'modal',

mixins:  \[toggle\],

components:  {

appChild:  Child

}

}

即便我们使用的是一个对象而不是一个组件，生命周期函数对我们来说仍然是可用的，理解这点很重要。我们也可以这里使用`mounted()`钩子函数，它将被应用于组件的生命周期上。这种工作方式真的很灵活也很强大。

### 合并

在下面的这个例子，我们可以看到，我们不仅仅是实现了自己想要的功能，并且Mixin中的生命周期的钩子也同样是可用的。因此，当我们在组件上应用Mixin的时候，有可能组件与Mixin中都定义了相同的生命周期钩子，这时候钩子的执行顺序的问题凸显了出来。默认Mixin上会首先被注册，组件上的接着注册，这样我们就可以在组件中按需要重写Mixin中的语句。**组件拥有最终发言权。**当发生冲突并且这个组件就不得不“决定”哪个胜出的时候，这一点就显得特别重要，否则，所有的东西都被放在一个数组当中执行，Mixin将要被先推入数组，其次才是组件。

JavaScript

//mixin const hi = { mounted() { console.log('hello from mixin!') } } //vue instance or component new Vue({ el: '#app', mixins: \[hi\], mounted() { console.log('hello from Vue instance!') } }); //Output in console > hello from mixin! > hello from Vue instance!

1

2

3

4

5

6

7

8

9

10

11

12

13

14

15

16

17

18

19

//mixin

const  hi  =  {

mounted()  {

console.log('hello from mixin!')

}

}

//vue instance or component

new  Vue({

el:  '#app',

mixins:  \[hi\],

mounted()  {

console.log('hello from Vue instance!')

}

});

//Output in console

>  hello from mixin!

>  hello from Vue instance!

如果这两个冲突了，我们看看 Vue实例或组件是如何决定输赢的：

JavaScript

//mixin const hi = { methods: { sayHello: function() { console.log('hello from mixin!') } }, mounted() { this.sayHello() } } //vue instance or component new Vue({ el: '#app', mixins: \[hi\], methods: { sayHello: function() { console.log('hello from Vue instance!') } }, mounted() { this.sayHello() } }) // Output in console > hello from Vue instance! > hello from Vue instance!

1

2

3

4

5

6

7

8

9

10

11

12

13

14

15

16

17

18

19

20

21

22

23

24

25

26

27

28

29

//mixin

const  hi  =  {

methods:  {

sayHello:  function()  {

console.log('hello from mixin!')

}

},

mounted()  {

this.sayHello()

}

}

//vue instance or component

new  Vue({

el:  '#app',

mixins:  \[hi\],

methods:  {

sayHello:  function()  {

console.log('hello from Vue instance!')

}

},

mounted()  {

this.sayHello()

}

})

// Output in console

>  hello from Vue instance!

>  hello from Vue instance!

你可能已经注意到这有两个`console.log`而不是一个——这是因为第一个函数被调用时，没有被销毁，它只是被重写了。我们在这里调用了两次`sayHello()`函数。

### 全局Mixin

当我们使用“全局”来描述Mixin的时候，我们并不是说Mixin能够像filter，在每个组件都能被访问到。只是我们能够在组件通过`mixins:`

访问组件上的Mixin对象。

全局Mixin被注册到了_每个单一组件上_。因此，它们的使用场景_极其_有限并且在使用的时候我们需要非常小心。一个我能想到的用途就是类似于插件，你需要赋予它访问所有东西的权限。但即使在这种情况下，我也对你正在做事情的充满警惕，尤其当你打算为应用增加通能的时候，这样做可能对你来说是个潘多拉的盒子。

为了创建一个全局实例，我们可以把它放在Vue实例之上。在一个典型的 Vue-cli 初始化的项目中，它可能在你的`main.js`文件中。

JavaScript

Vue.mixin({ mounted() { console.log('hello from mixin!') } }) new Vue({ ... })

1

2

3

4

5

6

7

8

9

Vue.mixin({

mounted()  {

console.log('hello from mixin!')

}

})

new  Vue({

...

})