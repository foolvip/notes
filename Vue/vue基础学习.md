
# Vue基础知识总结
## 官网地址
[vue官网](vhttps://cn.vuejs.org/)、[vue-cli官网](https://cli.vuejs.org/zh/)、[vue router官网](https://router.vuejs.org/zh/)、[Vuex官网](https://vuex.vuejs.org/zh/)
## 基础知识
知识总结blog:  
基础用法：https://github.com/liangklfangl/react-article-bucket/blob/master/vue/basic.md  
总结：https://github.com/liangklfangl/react-article-bucket/blob/master/vue/experience-usage.md
### 1、v-for用法
```
<p v-for="msg in messages">{{ msg }}</p>
```
### 2、生命周期总结
生命周期理解blog：https://segmentfault.com/a/1190000011381906   
1、 在beforeCreate和created钩子函数之间的生命周期  
在这个生命周期之间，进行初始化事件，进行数据的观测，可以看到在created的时候数据已经和data属性进行绑定（放在data中的属性当值发生改变的同时，视图也会改变）。
注意看下：此时还是没有el选项  
2、created钩子函数和beforeMount间的生命周期  
首先会判断对象是否有el选项。如果有的话就继续向下编译，如果没有el选项，则停止编译，也就意味着停止了生命周期，直到在该vue实例上调用vm.$mount(el)。  
如果我们在后面继续调用vm.$mount(el),可以发现代码继续向下执行了。  
el的判断要在template之前是因为vue需要通过el找到对应的outer template。    
render函数选项 > template选项 > outer HTML.  
3、 beforeMount和mounted 钩子函数间的生命周期  
可以看到此时是给vue实例对象添加$el成员，并且替换掉挂在的DOM元素  
4、mounted  
在mounted之前h1中还是通过{{message}}进行占位的，因为此时还有挂在到页面上，还是JavaScript中的虚拟DOM形式存在的。在mounted之后可以看到h1中的内容发生了变化。  
5、beforeUpdate钩子函数和updated钩子函数间的生命周期  
当vue发现data中的数据发生了改变，会触发对应组件的重新渲染，先后调用beforeUpdate和updated钩子函数  
6、beforeDestroy和destroyed钩子函数间的生命周期  
beforeDestroy钩子函数在实例销毁之前调用。在这一步，实例仍然完全可用。
destroyed钩子函数在Vue 实例销毁后调用。调用后，Vue 实例指示的所有东西都会解绑定，所有的事件监听器会被移除，所有的子实例也会被销毁。
### 3、监视数据变化 - watch  
```
new Vue({
  data: { a: 1, b: { age: 10 } },
  watch: {
    a: function(val, oldVal) {
      // val 表示当前值
      // oldVal 表示旧值
      console.log('当前值为：' + val, '旧值为：' + oldVal)
    },

    // 监听对象属性的变化
    b: {
      handler: function (val, oldVal) { /* ... */ },
      // deep : true表示是否监听对象内部属性值的变化 
      deep: true
    },

    // 只监视user对象中age属性的变化
    'user.age': function (val, oldVal) {
    },
  }
})
```
### 4、计算属性  
注意：computed中的属性不能与data中的属性同名，否则会报错 
```
var vm = new Vue({
  el: '#app',
  data: {
    firstname: 'jack',
    lastname: 'rose'
  },
  computed: {
    fullname() {
      return this.firstname + '.' + this.lastname
    }
  }
})
```

## 二、高级总结
### 1、监听路由
```
export default{
    data(){
        return{}
    },
    watch:{
        "$route":"getPath"  // 监听事件
    },
    methods:{
        getPath(){
            let path = this.$rount.path;    //获得当前路径进行逻辑判断
        }
    }
}
```
### 2、组件间传参
```
<!-- 父组件 parent.vue  -->
<template>
    <div class="parent">
        <h3>问卷调查</h3>
        <child v-model="form.name"></child>
        <div class="">
            <p>姓名：{{form.name}}</p>
        </div>
    </div>
</template>

<script>
    import child from './child.vue'
    export default {
        components: {
            child
        }，
        data () {
            return {
                form: {
                    name: '请输入姓名'
                }
            }
        }
    }
</script>

<!-- 子组件 child.vue  -->
<template>
    <div class="child">
        <label>
            姓名：<input type="text" :value="currentValue" @input="changeName">
        </label>
    </div>
</template>

<script>
    export default {
        props: {
            // 这个 prop 属性必须是 valule，因为 v-model 展开所 v-bind 的就是 value
            value: ''
        },
        methods: {
            changeName (e) {
                // 给input元素的 input 事件绑定一个方法 changeName 
                // 每次执行这个方法的时候都会触发自定义事件 input，并且把输入框的值传递进去。
                this.$emit('input', e.target.value)
            }
        }
    }
</script>
```
### 3、通过.native为Vue组件添加原生事件
例如：为ButtonMessage组件添加原生的click事件:
```
// 只有为Vue的组件添加.native才行，如果是下面的div或者p元素添加并不会起作用
<template>
    <div id="message-event-example" class="demo">
        <p v-for="msg in messages">{{ msg }}</p>
        <button-message v-on:click.native="focus" v-on:message="handleMessage" ></button-message>
    </div>
</template>
<script>
import ButtonMessage from "./ButtonMessage";
export default {
    data: function() {
        return {
            messages: []
        }
    },
    methods: {
        handleMessage: function(payload) {
            this.messages.push(payload.message)
        },
        focus:function(){
            alert('fuck');
        }
    },
    components: { ButtonMessage }
}
</script>
<style>
#message-event-example{
    border:1px solid red;
}
</style>
```
### 4、父组件调用子组件的方法修改子组件的值 
父组件通过ref属性直接调用子组件的方法
```
<!-- 父组件-->
<work-space :tab-value="index" ref="workspaceRef"></work-space> // html  
this.$refs.workspaceRef[this.editableTabValue].changeFormData(way, type)  // js
<!--子组件--> 
 changeFormData (way, type) {
    this.form.type = type
 },
```
### 5、子组件调用父组件的方法  
子组件可以通过this.$emit('方法名', 参数)触发父组件的方法，eg:    
this.$emit('setKgName', {selected: this.selected, index: this.tabValue})  
