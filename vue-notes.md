### vue组件

#### 组件定义

通过component  函数来定义一个新的组件

```javascript
// 定义一个名为 button-counter 的新组件
Vue.component('button-counter', {
  data: function () {
    return {
      count: 0
    }
  },
  template: '<button v-on:click="count++">You clicked me {{ count }} times.</button>'
})
```

#### 组件使用

定义了组件后我们就可以像使用普通`html`标签一样在页面中使用了,以上面定义的组件为例：

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport"
        content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>vue组件基础</title>
</head>
<body>
    <div id="myApp">
        <button-counter></button-counter>
    </div>
	
    <script type="javascript">
    	var app = new Vue({'el':'#myApp'})
    </script>
</body>
</html>
```

因为组件是可复用的 Vue 实例，所以它们与 `new Vue` 接收相同的选项，如`data`、`computed`、`watch`、`methods` 以及生命周期钩子等。*除了像 `el` 这样根实例特有的选项*

#### 组件复用

你可以将组件进行任意次数的复用:

```html
<div id="components-demo">
  <button-counter></button-counter>
  <button-counter></button-counter>
  <button-counter></button-counter>
</div>
```

上述代码会生成3个不同的vue实例，当你点击按钮时每个组件都会独立的维护内部的count变量. 这是因为你每用一次组件就会创建一个它的新的实例。

注意：定义组件时，组件的`data` 选项必须是一个 **函数** ，这使得每个实例可以维护一份被返回对象的拷贝。如果不遵守Vue定义的规则，那么每次修改组件都会影响到所有存活的实例，这显然不是我们想要的。

#### 组件注册

##### 全局注册

全局注册的组件可以用在其被注册之后的任何 (通过 `new Vue`) 新创建的 Vue 根实例，也包括其组件树中的所有子组件的模板中。

```javascript
Vue.component('button-counter', {
  data: function () {
    return {
      count: 0
    }
  },
  template: '<button v-on:click="count++">You clicked me {{ count }} times.</button>'
})
```

##### 局部注册

全局注册往往是不太理想的，当你使用模块化工具如webpack构建系统时，即便你已经不使用某一个组件了，但它依然会被打包进构建包中，这会使得用户下载的js代码增加。

####MVVM框架中单向绑定与双向绑定
MVVM就是在前端页面上，应用了扩展的MVC模式，我们关心Model的变化，MVVM框架自动把Model的变化映射到View视图的DOM树结构上，这样，用户看到的页面内容就会随着Model的变化而更新。
#####1.单向绑定
单向绑定是指model与视图view之间的一种单方面的绑定关系,当model数据变化时会将值传递给
视图视图负责显示,视图view的变化不会影响到model的数据
```html
<html>
<head>
    <!-- 引用jQuery -->
    <script src="..js/jquery.min.js"></script>
    <!-- 引用Vue -->
    <script src="..js/vue.js"></script>

</head>

<body>

<div id="vm">
    <p>Hello, {{ name }}!</p>
    <p>You are {{ age }} years old!</p>
</div>
<script>
    // 初始化代码:
    $(function () {
        var vm = new Vue({
            el: '#vm',
            data: {
                name: 'kaerro',
                age: 15
            }
        });
        window.vm = vm;
    });
</script>
</body>
<html>
```
在上面的例子中使用`{{` 与 `}}`包裹需要绑定的属性名来实现与model属性的绑定关系。
如果属性关联的是对象，那么可以使用多个.分隔引用,例如：`{{ user.hobby }}`
此外还有一种方法通过vue的指令 v-text来与元素进行绑定
```html
<p v-text="nextLine"> 对酒当歌，人生几何！ 譬如朝露，去日苦多</p>
```
#####2.双向绑定
有单向绑定，就有双向绑定。如果用户更新了View，Model的数据也自动被更新了，这种情况就是双向绑定。

什么情况下用户可以更新View呢？填写表单就是一个最直接的例子。当用户填写表单时，View的状态就被更新了，如果此时MVVM框架可以自动更新Model的状态，那就相当于我们把Model和View做了双向绑定.典型的使用场景就是我们填写完表单数据后
可能会希望自动更新表单绑定的内容，以往我们在提交表单数据的操作流程一般是使用jquery获取表单的数据序列化然后通过ajax传递到后端进行处理
。有了双向绑定后，不在需要手动获取数据，因为用户的修改直接体现在绑定的模型上了，我们可以通过model直接获得用户的输入并进行对应的处理。

在Vue中，我们使用`v-model` 来进行view与model的双向绑定
```html
<!doctype html>
<html lang="en">
<head>
    <script type="text/javascript" src="../js/vue.js" charset="UTF-8"></script>
    <script type="text/javascript" src="../js/elmentUI.js" charset="UTF-8"></script>
    <link rel="stylesheet" href="../css/elementUI.css">
    <title>表单输入绑定</title>
</head>
<body>
<div id="el1">
<el-form :inline="true" :model="formInline" class="demo-form-inline">
    <el-form-item label="审批人">
        <el-input v-model="formInline.user" placeholder="审批人"></el-input>
    </el-form-item>
    <el-form-item label="活动区域">
        <el-select v-model="formInline.region" placeholder="活动区域">
            <el-option label="区域一" value="shanghai"></el-option>
            <el-option label="区域二" value="beijing"></el-option>
        </el-select>
    </el-form-item>
    <el-form-item>
        <el-button type="primary" @click="onSubmit">查询</el-button>
    </el-form-item>
</el-form>
</div>

<script type="text/javascript">
    var el = new Vue(
            {
                el:'#el1',
                data:{
                    formInline: {
                        user: '',
                        region: ''
                    }
                },
                methods: {
                    onSubmit() {
                        console.log('submit!');
                        this.$message({ showClose: true,
                            message: '表单提交的数据为：'+this.formInline.user + this.formInline.region,
                            type: 'success'})
                        this.formInline.user = '罗年鑫'
                    }
                }
            });
</script>
</body>
</html>
```
