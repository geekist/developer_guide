
# 一、Vue 介绍

Vue.js是一款JavaScript前端框架，旨在更好地组织与简化Web开发。Vue所关注的核心是MVC模式中的视图层，同时，它也能方便地获取数据更新，并通过组件内部特定的方法实现视图与模型的交互。




# 二、Vue安装

## 2.1 直接下载安装vue

下载vue.js文件，放置在工程目录文件夹下面

在html文档中引入vue.js
```html
  <script src="vue.js" type= "text/javascript" charset="utf-8"></script>
```

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>VueTest</title>
    <script src="vue.js" type= "text/javascript" charset="utf-8"></script>
</head>

<body>

  <div id="app">{{ message }}</div>

  <script>
      // 初始化 Vue 实例
      let vm = new Vue({
        el: "#app",
        data: {
            message: "欢迎来到Vue的世界",
        }
      });
  </script>

</body>
</html>
```

## 2.2 用cdn的方式安装vue

在html文档中引入vue.js
```html
    <script src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.js"></script>
```
```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>VueTest</title>
<body>

</body>
</html>
```


```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>VueTest</title>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.js"></script>
</head>

<body>

  <div id="app">{{ message }}</div>

  <script>
      // 初始化 Vue 实例
      let vm = new Vue({
        el: "#app",
        data: {
            message: "欢迎来到Vue的世界",
        }
      });
  </script>

</body>
</html>
```

## 2.3 用vue-cli方式安装vue

vue-cli脚手架是一个vue开发框架，可以帮助我们快速地生成示例代码、搭建本地环境，也可以更新依赖的版本等，避免了每个开发者自行调整开发环境、打包逻辑等配置。Vue cli还提供了对 Babel、TypeScript、ESLint、PostCSS、PWA、单元测试和 End-to-end 测试提供开箱即用的支持。

Vue CLI 致力于将 Vue 生态中的工具基础标准化。它确保了各种构建工具能够基于智能的默认配置即可平稳衔接，这样你可以专注在撰写应用上，而不必花好几天去纠结配置的问题。
```shell
#今日工作区域
cd vue_demo

#安装脚手架
npm install -g @vue/cli

#脚手架生成 vue 项目，同时会自动安装依赖
vue create vue-cli-demo
```
vue-cli会自动在vue_demo/vue_cli_demo下生成vue项目。

![](./assets/vue-3.png)

运行npm run build即可启动vue

# 三、Vue对象和生命周期

## 3.1 vue文件的基本内容

Vue.js 的核心是一个允许采用简洁的模板语法来声明式地将数据渲染进 DOM 的系统。vue将文件分为视图和脚本部分

视图部分：

```html
<div id="app">
  {{ message }} <!--左右双大括号引入变量-->
</div>
```
脚本部分：

```js
//定义一个全局的vue变量，将对象的element绑定识图元素的id
var app = new Vue({
  el: '#app',
  data: {
    message: 'Hello Vue!'
  }
})
```

## 3.2 vue对象

每个 Vue 应用都是通过用 Vue 函数创建一个新的 Vue 实例开始的。开发中经常会使用 vm (ViewModel 的缩写) 这个变量名表示 Vue 实例。
```js
// new Vue返回一个Vue实例
var vm = new Vue({
  // 选项
});
```
选项是一个配置对象，描述了vue绑定的dom元素和数据。当一个 Vue 实例被创建时，它将 data 对象中的所有的 property 加入到 Vue 的响应式系统中。当这些 property 的值发生改变时，视图将会产生“响应”，即匹配更新为新的值。

```js
{
  el: '#app',
  data: {
    message: 'Hello Vue!'
  },
  created: function () {
    // `this` 指向 vm 实例
    console.log('a is: ' + this.a)
  },
}
```

>当使用Object.freeze()时，会阻止修改现有的 property，也意味着响应系统无法再追踪变化。

除了数据 property，Vue 实例还暴露了一些有用的实例 property 与方法。它们都有前缀 $，以便与用户定义的 property 区分开来。例如：

```js
var data = { a: 1 }
var vm = new Vue({
  el: '#example',
  data: data
})

vm.$data === data // => true
vm.$el === document.getElementById('example') // => true

// $watch 是一个实例方法
vm.$watch('a', function (newValue, oldValue) {
  // 这个回调将在 `vm.a` 改变后调用
})
```

Vue 实例和 Vue 应用是什么关系呢？

一个 Vue 应用由一个通过new Vue()创建的根 Vue 实例，以及可选的嵌套的、可复用的组件树组成。所以 Vue 实例是属于 Vue 应用的一部分，与组件树组成了 Vue 应用.所有的 Vue 组件都是 Vue 实例，并且接受相同的选项对象 (一些根实例特有的选项除外)。

```js
// 一个Vue应用，由根实例+组件树组成
根实例
└─ 根组件 // 此行开始，为组件树
   ├─ 组件1
   │  ├─ 组件1-1
   │  └─ 组件1-2
   └─ 组件2
      ├─ 组件2-1
      └─ 组件2-2
```
## 3.3 vue实例的生命周期和钩子函数

下图展示了vue的生命周期

![](./assets/vue-2.png)

每个 Vue 实例在被创建时都要经过一系列的初始化过程——例如，需要设置数据监听、编译模板、将实例挂载到 DOM 并在数据变化时更新 DOM 等。同时在这个过程中也会运行一些叫做生命周期钩子的函数，这给了用户在不同阶段添加自己的代码的机会。

比如 created 钩子可以用来在一个实例被创建之后执行代码：
```js
new Vue({
  data: {
    a: 1
  },
  created: function () {
    // `this` 指向 vm 实例
    console.log('a is: ' + this.a)
  }
})
// => "a is: 1"
```
也有一些其它的钩子，在实例生命周期的不同阶段被调用，如 mounted、updated 和 destroyed。生命周期钩子的 this 上下文指向调用它的 Vue 实例。

# 四、vue指令

## 4.1 指令定义

指令 (Directives) 是带有 v- 前缀的特殊 attribute。指令 attribute 的值预期是单个 JavaScript 表达式 (v-for 是例外情况，稍后我们再讨论)。指令的职责是，当表达式的值改变时，将其产生的连带影响，响应式地作用于 DOM。回顾我们在介绍中看到的例子：

```html
<!--v-if 指令将根据表达式 seen 的值的真假来插入/移除 <p> 元素。-->
<p v-if="seen">现在你看到我了</p>
```

## 4.2 指令参数（：）

一些指令能够接收一个“参数”，在指令名称之后以冒号表示。例如，v-bind 指令可以用于响应式地更新 HTML attribute：

```html
<a v-bind:href="url">...</a>
<!--在这里 href 是参数，告知 v-bind 指令将该元素的 href attribute 与表达式 url 的值绑定。-->
```

另一个例子是 v-on 指令，它用于监听 DOM 事件：

```html
<a v-on:click="doSomething">...</a>
<!--在这里参数是监听的事件名。我们也会更详细地讨论事件处理。-->
```

## 4.3 动态参数

从 2.6.0 开始，可以用方括号括起来的 JavaScript 表达式作为一个指令的参数：

```html
<!--
注意，参数表达式的写法存在一些约束，如之后的“对动态参数表达式的约束”章节所述。
-->
<a v-bind:[attributeName]="url"> ... </a>
```

这里的 attributeName 会被作为一个 JavaScript 表达式进行动态求值，求得的值将会作为最终的参数来使用。例如，如果你的 Vue 实例有一个 data property attributeName，其值为 "href"，那么这个绑定将等价于 v-bind:href。

同样地，你可以使用动态参数为一个动态的事件名绑定处理函数：

```html
<a v-on:[eventName]="doSomething"> ... </a>
```
在这个示例中，当 eventName 的值为 "focus" 时，v-on:[eventName] 将等价于 v-on:focus。

对动态参数的值的约束

动态参数预期会求出一个字符串，异常情况下值为 null。这个特殊的 null 值可以被显性地用于移除绑定。任何其它非字符串类型的值都将会触发一个警告。

对动态参数表达式的约束

动态参数表达式有一些语法约束，因为某些字符，如空格和引号，放在 HTML attribute 名里是无效的。例如：

```html
<!-- 这会触发一个编译警告 -->
<a v-bind:['foo' + bar]="value"> ... </a>
```

变通的办法是使用没有空格或引号的表达式，或用计算属性替代这种复杂表达式。

在 DOM 中使用模板时 (直接在一个 HTML 文件里撰写模板)，还需要避免使用大写字符来命名键名，因为浏览器会把 attribute 名全部强制转为小写：

```html
<!--
在 DOM 中使用模板时这段代码会被转换为 `v-bind:[someattr]`。
除非在实例中有一个名为“someattr”的 property，否则代码不会工作。
-->
<a v-bind:[someAttr]="value"> ... </a>
```

## 4.4 修饰符

修饰符 (modifier) 是以半角句号 . 指明的特殊后缀，用于指出一个指令应该以特殊方式绑定。

```html
.prevent 修饰符告诉 v-on 指令对于触发的事件调用 event.preventDefault()：
<form v-on:submit.prevent="onSubmit">...</form>
```

在接下来对 v-on 和 v-for 等功能的探索中，你会看到修饰符的其它例子。


# 五、模板语法基础

## 5.1 文本插值

数据绑定最常见的形式就是使用“Mustache”语法 (双大括号) 的文本插值,可以将vue的属性值插入到html的文本中

```html
<div id="app">
    <span>Message: {{msg}}</span>
</div>
```

```js
//定义一个全局的vue变量，将对象的element绑定识图元素的id
var app = new Vue({
  el: '#app',
  data: {
    msg: 'Hello Vue!'
  }
})
```

Mustache 标签将会被替代为对应数据对象上 msg property 的值。无论何时，绑定的数据对象上 msg property 发生了改变，插值处的内容都会更新。

## 5.2 v-once指令使文本只绑定一次

通过使用 v-once 指令，你也能执行一次性地插值，当数据改变时，插值处的内容不会更新。但请留心这会影响到该节点上的其它数据绑定：

```html
<span v-once>这个将不会改变: {{ msg }}</span>
```

## 5.3 v-html指令绑定html代码差值

双大括号会将数据解释为普通文本，而非 HTML 代码。为了输出真正的 HTML，你需要使用 v-html 指令：

```html
<body>
    <div id="app">
        <div>{{ message }}</div>
        <div>{{ msgHtml }}</div>

        <div v-html="message"></div>
        <div v-html="msgHtml"></div>
    </div>
    
    <script>
        new Vue({
            el: "#app",
            data:{
                
                message: "欢迎来到tetVue的世界",
                msgHtml: "<p style='color: red'>欢迎来到红色Vue的世界</p>",
               
            },
        });
    </script>
</body>
```
最后显示为：

```html
欢迎来到tetVue的世界
<p style='color: red'>欢迎来到红色Vue的世界</p>
欢迎来到tetVue的世界

欢迎来到红色Vue的世界
```

这个 span 的内容将会被替换成为 property 值 rawHtml，直接作为 HTML——会忽略解析 property 值中的数据绑定。注意，你不能使用 v-html 来复合局部模板，因为 Vue 不是基于字符串的模板引擎。反之，对于用户界面 (UI)，组件更适合作为可重用和可组合的基本单位。

**站点上动态渲染的任意 HTML 可能会非常危险，因为它很容易导致 XSS 攻击。请只对可信内容使用 HTML 插值，绝不要对用户提供的内容使用插值。**

## 5.4 v-bind指令绑定html的属性值

Mustache 语法不能作用在 HTML属性上，遇到这种情况应该使用 v-bind 指令：

```html
<!-- 绑定一个属性 -->
<img v-bind:src="imageSrc" />
<!-- 缩写 -->
<img :src="imageSrc" />
<!-- 最终会生成 `<img src="${imageSrc}">` 这样的模板 -->

<!-- 动态特性名 (2.6.0+) -->
<button v-bind:[key]="value"></button>
<!-- 动态特性名缩写 (2.6.0+) -->
<button :[key]="value"></button>
<!-- 最终会生成 `<button ${key}="${value}">` 这样的模板 -->

<!-- 内联字符串拼接 -->
<img :src="'/path/to/images/' + fileName" />

<!-- class 绑定 -->
<div :class="{ red: isRed }"></div>
<div :class="[classA, classB]"></div>
<div :class="[classA, { classB: isB, classC: isC }]">
  <!-- style 绑定 -->
  <div :style="{ fontSize: size + 'px' }"></div>
  <div :style="[styleObjectA, styleObjectB]"></div>

  <!-- 绑定一个有属性的对象 -->
  <div v-bind="{ id: someProp, 'other-attr': otherProp }"></div>
</div>
```


* v-bind绑定布尔值

对于布尔 attribute (它们只要存在就意味着值为 true)，v-bind 工作起来略有不同，在这个例子中：

```html
<button v-bind:disabled="isButtonDisabled">Button</button>
```
如果 isButtonDisabled 的值是 null、undefined 或 false，则 disabled attribute 甚至不会被包含在渲染出来的 <button> 元素中。

使用JavaScript 表达式

迄今为止，在我们的模板中，我们一直都只绑定简单的 property 键值。但实际上，对于所有的数据绑定，Vue.js 都提供了完全的 JavaScript 表达式支持。
```js
{{ number + 1 }}

{{ ok ? 'YES' : 'NO' }}

{{ message.split('').reverse().join('') }}

<div v-bind:id="'list-' + id"></div>
```
这些表达式会在所属 Vue 实例的数据作用域下作为 JavaScript 被解析。有个限制就是，每个绑定都只能包含单个表达式，所以下面的例子都不会生效。
```js
<!-- 这是语句，不是表达式 -->
{{ var a = 1 }}

<!-- 流控制也不会生效，请使用三元表达式 -->
{{ if (ok) { return message } }}
```
模板表达式都被放在沙盒中，只能访问全局变量的一个白名单，如 Math 和 Date 。你不应该在模板表达式中试图访问用户定义的全局变量。

* v-bind缩写

vue中规定v-bind指令可以缩写成：

```js
<!-- 完整语法 -->
<a v-bind:href="url">...</a>

<!-- 缩写 -->
<a :href="url">...</a>

<!-- 动态参数的缩写 (2.6.0+) -->
<a :[key]="url"> ... </a>
```

## 5.5 计算属性（computed）

模板内的表达式非常便利，但是设计它们的初衷是用于简单运算的。在模板中放入太多的逻辑会让模板过重且难以维护。例如：

```html
<div id="example">
  {{ message.split('').reverse().join('') }}
</div>
```
在这个地方，模板不再是简单的声明式逻辑。你必须看一段时间才能意识到，这里是想要显示变量 message 的翻转字符串。当你想要在模板中的多处包含此翻转字符串时，就会更加难以处理。

所以，对于任何复杂逻辑，你都应当使用计算属性。

计算属性的一个例子：
```html
<div id="example">
  <p>Original message: "{{ message }}"</p>
  <p>Computed reversed message: "{{ reversedMessage }}"</p>
</div>

var vm = new Vue({
  el: '#example',
  data: {
    message: 'Hello'
  },
  computed: {
    // 计算属性的 getter
    reversedMessage: function () {
        // `this` 指向 vm 实例
        return this.message.split('').reverse().join('')
    }
  }
})
```
我们可以像绑定普通 property 一样在模板中绑定计算属性。Vue 知道 vm.reversedMessage 依赖于 vm.message，因此当 vm.message 发生改变时，所有依赖 vm.reversedMessage 的绑定也会更新。而且最妙的是我们已经以声明的方式创建了这种依赖关系：计算属性的 getter 函数是没有副作用 (side effect) 的，这使它更易于测试和理解。

## 5.6 侦听器（watch）

侦听器用来监听某些数据变化，观察 Vue 实例变化的一个表达式或计算属性函数。Vue 实例将会在实例化时调用$watch()，遍历 watch 对象的每一个属性。watch支持的方式有好几种，回调函数得到的参数为新值和旧值：
```js
var vm = new Vue({
  data: {
    a: 1,
    b: 2,
    c: 3,
    d: 4,
    e: {
      f: {
        g: 5
      }
    }
  },
  watch: {
    a: function(val, oldVal) {
      console.log("new: %s, old: %s", val, oldVal);
    },
    // 方法名
    b: "someMethod",
    // 该回调会在任何被侦听的对象的 property 改变时被调用，不论其被嵌套多深
    c: {
      handler: function(val, oldVal) {
        /* ... */
      },
      deep: true
    },
    // 该回调将会在侦听开始之后被立即调用
    d: {
      handler: "someMethod",
      immediate: true
    },
    e: [
      "handle1",
      function handle2(val, oldVal) {
        /* ... */
      },
      {
        handler: function handle3(val, oldVal) {
          /* ... */
        }
        /* ... */
      }
    ],
    // watch vm.e.f's value: {g: 5}
    "e.f": function(val, oldVal) {
      /* ... */
    }
  },
  methods: {
    someMethod() {}
  }
});
vm.a = 2; // => new: 2, old: 1
```

# 六、绑定class与style

操作元素的 class 列表和内联样式是数据绑定的一个常见需求。因为它们都是 attribute，所以我们可以用 v-bind 处理它们：只需要通过表达式计算出字符串结果即可。不过，字符串拼接麻烦且易错。因此，在将 v-bind 用于 class 和 style 时，Vue.js 做了专门的增强。表达式结果的类型除了字符串之外，还可以是对象或数组。

## 6.1 绑定css class（v-bind:class）

根据属性值决定class是否显示

```html
 <div id="app">
        <!--当isActive的值为true时，渲染成为<div class="active"></div>-->
        <div v-bind:class="{active:isActive }">{{ message }}</div>

        <!--当isActive和hasError的值为true时，渲染成为<div class="static active text-danger"></div>-->
        <div class="static" v-bind:class="{ active: isActive, 'text-danger': hasError }"></div>
        
        <!--渲染成为<div class="active"></div>-->
        <div v-bind:class="classObject"></div>
</div>

<script>
        new Vue({
            el: "#app",
            data: {
                isActive: true,
                hasError: true,
                classObject: {
                    active: true,
                    'text-danger': false
                }

            }
        });
    </script>
```

* 数组语法
* 
我们可以把一个数组传给 v-bind:class，以应用一个 class 列表：
```html
<div v-bind:class="[activeClass, errorClass]"></div>
data: {
  activeClass: 'active',
  errorClass: 'text-danger'
}
```
渲染为：

```
<div class="active text-danger"></div>
```

如果你也想根据条件切换列表中的 class，可以用三元表达式：
```
<div v-bind:class="[isActive ? activeClass : '', errorClass]"></div>
```
这样写将始终添加 errorClass，但是只有在 isActive 是 truthy[1] 时才添加 activeClass。

不过，当有多个条件 class 时这样写有些繁琐。所以在数组语法中也可以使用对象语法：
```
<div v-bind:class="[{ active: isActive }, errorClass]"></div>
```

## 6.2 绑定样式（v-bind:style）

v-bind:style 的对象语法十分直观——看着非常像 CSS，但其实是一个 JavaScript 对象。CSS property 名可以用驼峰式 (camelCase) 或短横线分隔 (kebab-case，记得用引号括起来) 来命名：
```js
<div v-bind:style="{ color: activeColor, fontSize: fontSize + 'px' }"></div>
data: {
  activeColor: 'red',
  fontSize: 30
}
```
直接绑定到一个样式对象通常更好，这会让模板更清晰：

```vue
<div v-bind:style="styleObject"></div>
data: {
  styleObject: {
    color: 'red',
    fontSize: '13px'
  }
}
```
同样的，对象语法常常结合返回对象的计算属性使用。

## 七、条件渲染（v-if)

v-if 指令用于条件性地渲染一块内容。这块内容只会在指令的表达式返回 truthy 值的时候被渲染。
```html
<h1 v-if="awesome">Vue is awesome!</h1>

<!--也可以用 v-else 添加一个“else 块”：-->
<h1 v-if="awesome">Vue is awesome!</h1>
<h1 v-else>Oh no 😢</h1>
```

* 在 <template> 元素上使用 v-if 条件渲染分组

因为 v-if 是一个指令，所以必须将它添加到一个元素上。但是如果想切换多个元素呢？此时可以把一个 <template> 元素当做不可见的包裹元素，并在上面使用 v-if。最终的渲染结果将不包含 <template> 元素。

```html
<template v-if="ok">
  <h1>Title</h1>
  <p>Paragraph 1</p>
  <p>Paragraph 2</p>
</template>
```

* v-else-if

v-else-if，顾名思义，充当 v-if 的“else-if 块”，可以连续使用：
```
<div v-if="type === 'A'">
  A
</div>
<div v-else-if="type === 'B'">
  B
</div>
<div v-else-if="type === 'C'">
  C
</div>
<div v-else>
  Not A/B/C
</div>
```
类似于 v-else，v-else-if 也必须紧跟在带 v-if 或者 v-else-if 的元素之后。

* 用 key 管理可复用的元素

Vue 会尽可能高效地渲染元素，通常会复用已有元素而不是从头开始渲染。这么做除了使 Vue 变得非常快之外，还有其它一些好处。例如，如果你允许用户在不同的登录方式之间切换：

```html
<template v-if="loginType === 'username'">
  <label>Username</label>
  <input placeholder="Enter your username">
</template>
<template v-else>
  <label>Email</label>
  <input placeholder="Enter your email address">
</template>
```
那么在上面的代码中切换 loginType 将不会清除用户已经输入的内容。因为两个模板使用了相同的元素，<input> 不会被替换掉——仅仅是替换了它的 placeholder。


这样也不总是符合实际需求，所以 Vue 为你提供了一种方式来表达“这两个元素是完全独立的，不要复用它们”。只需添加一个具有唯一值的 key attribute 即可：
```html
<template v-if="loginType === 'username'">
  <label>Username</label>
  <input placeholder="Enter your username" key="username-input">
</template>
<template v-else>
  <label>Email</label>
  <input placeholder="Enter your email address" key="email-input">
</template>
```
现在，每次切换时，输入框都将被重新渲染。注意，<label> 元素仍然会被高效地复用，因为它们没有添加 key attribute。

* v-show

另一个用于根据条件展示元素的选项是 v-show 指令。用法大致一样：
```
<h1 v-show="ok">Hello!</h1>
```
不同的是带有 v-show 的元素始终会被渲染并保留在 DOM 中。v-show 只是简单地切换元素的 CSS property display。

注意，v-show 不支持 <template> 元素，也不支持 v-else。

# 八、列表渲染（v-for)

用 v-for 把一个数组对应为一组元素

我们可以用 v-for 指令基于一个数组来渲染一个列表。v-for 指令需要使用 item in items 形式的特殊语法，其中 items 是源数据数组，而 item 则是被迭代的数组元素的别名。

```js
<body>
    <div id="app">
       <ul v-for="item in numbers">
        <li>
            {{item}}
        </li>
       </ul>

       <ul v-for="item in items">
        <li>
            {{item.value}}
        </li>
       </ul>
    </div>

    <script>
        new Vue({
            el: "#app",
            data: {
                items:[
                    {value:"test"},
                    {value:"hello"},
                    {value:"is"},
                ],
                numbers:[1,2,3],
                } 
        });
    </script>
</body>
```

# 九、事件处理v-on
```js
<div id="example-2">
  <!-- `greet` 是在下面定义的方法名 -->
  <button v-on:click="greet">Greet</button>
</div>

var example2 = new Vue({
  el: '#example-2',
  data: {
    name: 'Vue.js'
  },
  // 在 `methods` 对象中定义方法
  methods: {
    greet: function (event) {
      // `this` 在方法里指向当前 Vue 实例
      alert('Hello ' + this.name + '!')
      // `event` 是原生 DOM 事件
      if (event) {
        alert(event.target.tagName)
      }
    }
  }
})

// 也可以用 JavaScript 直接调用方法
example2.greet() // => 'Hello Vue.js!'
```

# 十、双向绑定数据V-mode

```js
<input type="checkbox" id="jack" value="Jack" v-model="checkedNames">
<label for="jack">Jack</label>
<input type="checkbox" id="john" value="John" v-model="checkedNames">
<label for="john">John</label>
<input type="checkbox" id="mike" value="Mike" v-model="checkedNames">
<label for="mike">Mike</label>
<br>
<span>Checked names: {{ checkedNames }}</span>
new Vue({
  el: '...',
  data: {
    checkedNames: []
  }
})
```

# 十一、组件

