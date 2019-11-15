### 混入
### Mixins是一种分发Vue组件中可复用功能的非常灵活的一种方式。

实现功能：
不同的组件，它们的作用是切换一个状态布尔值，一个模态框和一个提示框。这些提示框和模态框除了在功能上，
没有其他共同点：它们看起来不一样，用法不一样，但是逻辑一样。

``` Javascript
// mixin.js
export const toggle = {
    data () {
        isshowing: false
    },
    methods: {
        toggleShow() {
            this.isshowing = !this.isshowing
        }
    }
}
```
// modal.vue
// 将mixin引入该组件，就可以直接使用 toggleShow() 了

```Javascript
import {mixin} from '../mixin.js'
export default {
    mixins: [mixin],
    mounted () {
        
    }
}
// tooltip组件同上
```

### 合并
1、数据对象内
mixin的数据对象和组件的数据发生冲突时以组件数据优先。
```Javascript
var mixin = {
    data: function () {
        return {
            message: 'hello',
            foo: 'abc'
        }
    }
}

new Vue({
    mixins: [mixin],
    data: function () {
        return {
        message: 'goodbye',
        bar: 'def'
        }
    },
    created: function () {
        console.log(this.$data)
        // => { message: "goodbye", foo: "abc", bar: "def" }
    }
})
```

2、钩子函数(钩子即生命周期)
同名钩子函数将会混合为一个数组，都将被调用到，但是混入对象的钩子将在组件自身钩子之前调用。
```Javascript
var mixin = {
    created: function () {
        console.log('混入对象的钩子被调用')
    }
}

new Vue({
    mixins: [mixin],
    created: function () {
        console.log('组件钩子被调用')
    }
})

// => "混入对象的钩子被调用"
// => "组件钩子被调用"
```
3、值为对象的选项
值为对象的选项，例如 methods, components 和 directives，将被混合为同一个对象。两个对象键名冲突时，取组件对象的键值对。
```Javascript
var mixin = {
    methods: {
        foo: function () {
        console.log('foo')
        },
        conflicting: function () {
        console.log('from mixin')
        }
    }
}

var vm = new Vue({
    mixins: [mixin],
    methods: {
        bar: function () {
            console.log('bar')
        },
        conflicting: function () {
            console.log('from self')
        }
    }
})

vm.foo() // => "foo"
vm.bar() // => "bar"
vm.conflicting() // => "from self"
```
全局混入

全局混合被注册到了每个单一组件上。因此，它们的使用场景极其有限并且要非常的小心。一个我能想到的用途就是它像一个插件，你需要赋予它访问所有东西的权限。但即使在这种情况下，我也对你正在做的保持警惕，尤其是你在应用中扩展的函数，可能对你来说是不可知的。

```Javascript
Vue.mixin({
    mounted() {
        console.log("我是mixin");
    }
})

new Vue({
    ...
})
```