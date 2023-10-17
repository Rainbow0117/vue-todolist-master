# Todolist

> TodoList 顾名思义就是一个任务列表

## 项目预览
![todolist](http://images.leegeing.cn/hexoImg/todolist.gif)

## 如何运行

和大多数项目一样，直接下载到本地运行如下命令即可，前提是你已经安装了 `node.js`

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev
```

## 运用到的Vue知识

+ 使用了官方推荐的脚手架 `vue-cli` ,用 `webpack` 来快速构建打包项目

+ 使用 `stylus` 预加载样式，图标是用阿里的 `iconfont`

+ 引入了 `VUX` 库，用了时间控件，还有弹窗控件

+ 引入了 `Animate.css` 动画效果

+ 引入了 `ESlint` 语法来规范代码

+ 页面之间用了 `vue-router` 来进行页面的切换。

+ 使用了 `props` 父子组件传值

+ 本来还有路由传值的，但是后面用了 `Vuex`，就没有使用路由传值了

+ 使用了本地存储数据 `localStorage` ，即便刷新后也不会丢失数据

+ 列表用了 `better-scroll` 做一个区域滚动


## 遇到的问题
+ 以前我对什么时候使用组件，什么时候使用路由没有概念，就是胡乱使用。现在要我来总结的话，**一个页面的组成尽可能的使用组件来构成**，方便维护和开发。

+ 页面之间的传值可以使用路由传值，简单的组件之间可以使用父子组件传值，但是不同页面之间的组件进来传值，推荐你还是使用 `Vuex` 来进行数据的交互，因为不这样做的话，你的代码会变得非常臃肿，**页面之间耦合程度非常高**，就是说一环错，你可能会步步错，不利于维护。

+ **Vue中props值无法赋值给data域的问题**

+ 在使用 `Vuex` 之后，我不清楚是用父组件统一去操作数据再传给各个子组件，还是由子组件直接操作数据。因为这这两种方式我都使用了，好像没有什么很大的区别。


## 如何解决问题
解决问题的第一步并不是谷歌或者百度，而是自己先去想想问题，去查查官方文档。有些问题官方文档已经提及到了，或许是你没有看仔细而遗漏了，而这个时候正是你复习的绝佳机会。

+  prop 用来传递一个初始值；这个子组件接下来希望将其作为一个本地的 prop 数据来使用。这意味着你不能直接对接收到的值进行修改。


```JS
// 在这种情况下，最好定义一个本地的 data 属性并将这个 prop 用作其初始值

props: ['initialCounter'],
data: function () {
  return {
    counter: this.initialCounter
  }
}
```
根据上面这个问题而衍生出来的第二个问题，你会发现并不能赋值成功。

+ vue中props值无法赋值给data域的问题


```JS
export default {
  name: "]'Total',
  props:['total'],
  data () {
    return {
        amount:this.total
    }
  },
//watch很重要。data（）函数只是在初始化的时候会运行一次。所以总是空。而我们异步过来的数据，需要watch他 才能得到。
  watch:{
    total:function(newVal,oldVal){
        this.amount = newVal;
    }
  },
  mounted(){
      console.log(this.amount)
  }
}
```

+ 复杂数据类型在栈中存贮的是指针,所以赋值给新的变量也会改变原始的变量值.

这个问题困扰我蛮久了，因为以前都是简单的赋值，并没有出现过这个情况。我的使用场景是，在把时间戳转为时间格式的时候，我没有直接对原有的数据进行修改，而是复制了一份出来，去操作那份复制的数据，可每次都会影响到原来的值，这让我很困扰，后来还是找到了资料解决。**复杂数据类型在栈中存贮的是指针,所以赋值给新的变量也会改变原始的变量值.**


```JS
// 数组深度克隆
var x = [1,2,3];
var y = [];
for (var i = 0; i < x.length; i++) {
    y[i]=x[i];
}
console.log(y);  //[1,2,3]
y.push(4);
console.log(y);  //[1,2,3,4]
console.log(x);  //[1,2,3]
```

```JS
// 对象深度克隆:
var x = {a:1,b:2};
var y = {};
for(var i in x){
    y[i] = x[i];
}
console.log(y);  //Object {a: 1, b: 2}
y.c = 3;
console.log(y);  //Object {a: 1, b: 2, c: 3}
console.log(x);  //Object {a: 1, b: 2}
```


```JS
// 函数深度克隆
// 由于函数对象克隆之后的对象会单独复制一次并存储实际数据，因此并不会影响克隆之前的对象。所以采用简单的复制“=”即可完成克隆。
var x = function(){console.log(1);};
var y = x;
y = function(){console.log(2);};
x();  //1
y();  //2
```


```JS
// 强大的JSON.stringify和JSON.parse
const obj1 = JSON.parse(JSON.stringify(obj));
```


## 总结
项目其实不难，最主要的是要对自己有信心，别怕不会做，只要你勇敢的跨出那一步，一切问题都会迎刃而解。自己独立做过一个小项目之后，会对这个框架有一个全新的认识。

什么时候该用什么方法，什么时候不该用什么方法，你心里都会有个底了。

有时候会遇到一些难题，因为不能及时解决而导致丧失信心，或者心情郁闷，但其实你可以去问问前辈们，或许他们的简单两句话就能帮上你的大忙。

完结撒花🎊🎊🎉🎉🎊
