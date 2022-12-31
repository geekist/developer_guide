
vue中pops知识点：

# 什么是 props ?

props 是我们在不同组件之间传递变量和其他信息的方式。这类似于在 JS 中，我们可以将变量作为参数传递给函数:
```js
const myMessage = "I'm a string";

function addExclamation(message) {
  return message + '!';
}

console.log(addExclamation(myMessage)); // I'm a string!
```

这里，我们将变量myMessage作为参数message传递给函数。在函数内部，我们可以将该值作为message访问。

props的工作原理与此非常相似。我们将props传递给另一个组件，然后该组件可以使用该值。但是首先需要了解一些规则。

# props 的两个主要特点

在处理props时，有两件事需要特别注意：

 >props 通过组件树传递给后代(而不是向上传递)
 >props 是只读的，不能修改
 
Vue 使用单向数据流，这意味着数据只能从父组件流向子组件，不能将数据从子对象传递到父对象。因为父组件“拥有”它传递的值，所以子组件不能修改它。如果只允许一个组件更改它，那么跟踪bug就更容易了，因为我们确切地知道应该从哪里查找。

在开发确保没有违反这两条规则，开发就会变得更容易些，出问题也比较好找原因。接着来看看如何将 props 从一个组件传递到另一个组件。

# 将 props 传递给其他组件

如果希望将值从组件传递到子组件，这与添加HTML属性完全相同。
```js
<template>
  <Camera
    name="Sony A7RIV"
    img="../sony-a7riv.jpg"
  />
</template>
```
Camera组件将使用name和img props 来渲染自身页面。内容大概如下：
```js
<template>
  <div class="camera">
    <h2 class="camera__name">{{ name }}</h2>
    <img class="camera__image" :src="img" />
  </div>
</template>
```
在这里，我们将name渲染到h2标记中，并使用img设置img标记上的src属性。

但是，如果我们将此信息存储在某个位置的变量中怎么办？

为此，我们需要使用稍微不同的语法，因为我们希望使用 JS 表达式而不是传递字符串。
```js
<template>
  <Camera
    v-bind:name="cameraName"
    v-bind:img="cameraImage"
  />
</template>
```
v-bind:name="cameraName"行告诉Vue将 JS 表达式cameraName绑定到 prop name。JS 表达式是 JS 的任何代码段。 可能是像我们在此处这样的变量名，或更复杂的名称。

还可以使用逻辑或 img 设置图像路径：
```js
<template>
  <Camera
    v-bind:name="cameraName"
    v-bind:img="cameraImage || '../no-camera-found.jpg'"
  />
</template>
```
v-bind 可以用简写形式 :
```js
<template>
  <Camera
    :name="cameraName"
    :img="cameraImage || '../no-camera-found.jpg'"
  />
</template>
```

# 添加 props

在此代码实际起作用之前，我们需要获取Camera组件才能实际收听props。 默认情况下，组件会忽略它们。为此，我们必须在组件定义中添加一个props部分：
```js
export default {
  name: 'Camera',
  props: ['name', 'img'],
}
```
通常不建议这么写，应该为props对象指定类型：
```js
export default {
  name: 'Camera',
  props: {
      name: {
      type: String,
      },
      img: {
      type: String,
      }
  }
}
```

通过从数组到对象，我们可以指定更多的 props 细节，比如类型。我们为什么要向 props 添加类型？

在Vue中，props 可以有很多不同的类型：

String
Number
Boolean (true 或者 false)
Array
Object

通过添加类型，我们可以设置我们期望收到的数据类型。如果我们将camera的props中的name设置为true，它将无法正常工作，因此Vue会警告我们使用错误。

接着添加一个rating到我们的Camera组件中，该 rating 类型为 Number:

```js
export default {
  name: 'Camera',
  props: {
      name: {
      type: String,
      },
      img: {
      type: String,
      },
      rating: {
      type: Number,
      },
  }
}
```
然后在 template 中显示 rating:

```js
<template>
  <div class="camera">
    <h2 class="camera__name">{{ name }}</h2>
    <span class="camera__rating">{{ rating }}</span>
    <img class="camera__image" src="img" />
  </div>
</template>
```

在外层调用：
```js
<template>
  <Camera
    name="Sony A7RIV"
    img="../sony-a7riv.jpg"
    :rating="9"
  />
</template>
```
# 必填的 props

不是所有的 props 都是一样的，为了使组件正常工作，其中一些要求必填的。

对于我们的Camera组件，我们肯定需要一个name，但 img 和 rating 不是必需的。
```js
export default {
  name: 'Camera',
  props: {
      name: {
      type: String,
      required: true,
      },
      img: {
      type: String,
      },
      rating: {
      type: Number,
      },
  }
}
```
通过设置 required: true 要求我们的 name 是必需要传入的，相反，required 为 false 对应的props可传可不传。

# 默认值

对于不是每次都传入的 props，我们可以为其，添加默认值。

export default {
  name: 'Camera',
  props: {
      name: {
      type: String,
      required: true,
      },
      img: {
      type: String,
      default: '../no-camerage-found.jpg',
      },
      rating: {
      type: Number,
      },
  }
}

前面我们通过逻辑或为img添加默认值，这次我们使用 default 属性为img设置默认值。

同样也需要为我们的rating设置默认值。如果没有设置也没有从外部传入，我们访问的时候就会得到undefined，这可能会给我们带来一些问题

# 在模板外使用 props

虽然能够在template中使用props很棒，但是真正强大的功能来自于在方法、计算属性和组件中在使用其他 JS 中使用它们。

在我们的template中，我们看到我们只需要props名称，例如：{{rating}}。 但是，在Vue组件的其他任何地方，我们都需要使用this.rating访问我们的props。

让我们重构应用程序，以便为图像使用标准的URL结构。 这样，我们不必每次都将其传递给Camera组件，而只需从名称中找出即可。

我们将使用以下结构：./images/cameras/${cameraName}.jpg

因此，如果 camera 是Sony A6400，则URL将变为./images/cameras/Sony%20A6400.jpg。 ％20来自对空格字符的编码，因此我们可以在URL中使用它。

首先，我们将移除不再需要的img props

```js
export default {
  name: 'Camera',
  props: {
      name: {
      type: String,
      required: true,
      },
      rating: {
      type: Number,
      default: 0,
      },
  }
}
```
然后，我们将添加一个计算属性，该属性将为我们生成图像URL：

```js
export default {
  name: 'Camera',
  props: {
      name: {
      type: String,
      required: true,
      },
      rating: {
      type: Number,
      default: 0,
      },
  },
  computed: {
    img() {
      return `./images/cameras/${encodeURIComponent(this.name)}.jpg`;
    }
  }
}
```

并非所有字符都可以在URL中使用，因此encodeURIComponent会为我们转换这些字符。

因为我们可以使用与常规props相同的方式来访问此计算 props，所以我们根本不需要更改模板，并且模板可以像以前一样保持不变：

```js
<template>
  <div class="camera">
    <h2 class="camera__name">{{ name }}</h2>
    <span class="camera__rating">{{ rating }}</span>
    <img class="camera__image" src="img" />
  </div>
</template>
```
样，您可以在以下位置使用组件的props：

watch 中
生命周期 hook
method
computed 中
以及组件定义中的其他任何地方！

总结
以上，这些是关于 props 的知识点，但是，总会有更多东西要学习。 Vue 也是一个永无止境的学习过程。keep going !