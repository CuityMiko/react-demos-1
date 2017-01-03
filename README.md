# React 基本教程
### 简介
React 是一个用于构建用户界面的 JAVASCRIPT 库。
React主要用于构建UI，很多人认为 React 是 MVC 中的 V（视图）。
React 起源于 Facebook 的内部项目，用来架设 Instagram 的网站，并于 2013 年 5 月开源。
React 拥有较高的性能，代码逻辑非常简单，越来越多的人已开始关注和使用它。
### React 特点
* 声明式设计 −React采用声明范式，可以轻松描述应用。
* 高效 −React通过对DOM的模拟，最大限度地减少与DOM的交互。
* 灵活 −React可以与已知的库或框架很好地配合。
* JSX − JSX 是 JavaScript 语法的扩展。React 开发不一定使用 JSX ，但我们建议使用它。
* 组件 − 通过 React 构建组件，使得代码更加容易得到复用，能够很好的应用在大项目的开发中。
* 单向响应的数据流 − React 实现了单向响应的数据流，从而减少了重复代码，这也是它为什么比传统数据绑定更简单。

### React 安装
React 可以直接下载使用，可以从[官网](https://facebook.github.io/react/)下载最新版。也可以使用React CDN 库; 
```javascript
<script src="https://cdnjs.cloudflare.com/ajax/libs/react/15.3.1/react.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/react/15.3.1/react-dom.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/babel-core/6.1.4/browser.min.js"></script>
```
基本模板输出hello,world!
```javascript
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8"/>
    <title>Hello React!</title>
    <script src="../../build/react.js"></script>
    <script src="../../build/react-dom.js"></script>
    <script src="../../build/browser.min.js"></script>
</head>
<body>
<div id="example"></div>
<script type="text/babel">
    ReactDOM.render(
        <h1>Hello, world!</h1>,
        document.getElementById('example')
    );
</script>
</body>
</html>
```
需注意两点：
* 首先，最后一个 script 标签的 type 属性为 text/babel 。这是因为 React 独有的 JSX 语法，跟 JavaScript 不兼容。凡是使用 JSX 的地方，都要加上 type="text/babel" 。
* 其次，上面代码一共用了三个库： react.js 、react-dom.js 和 Browser.js ，它们必须首先加载。其中，react.js 是 React 的核心库，react-dom.js 是提供与 DOM 相关的功能，Browser.js 的作用是将 JSX 语法转为 JavaScript 语法，这一步很消耗时间，实际上线的时候，应该将它放到服务器完成。

#### 一、React JSX
```javascript
ReactDOM.render(
    <h1>Hello, world!</h1>,
    document.getElementById('example')
);
```
##### a）、JavaScript 表达式
```javascript
ReactDOM.render(
	<div>
	  <h1>{i == 1 ? 'True!' : 'False'}</h1>
	</div>,
	document.getElementById('example')
);
```
注意：在 JSX 中不能使用 if else 语句，但可以使用 conditional (三元运算) 表达式来替代。以下实例中如果变量 i 等于 1 浏览器将输出 true, 如果修改 i 的值，则会输出 false。
##### b）、样式
React 推荐使用内联样式。我们可以使用 camelCase 语法来设置内联样式. React 会在指定元素数字后自动添加 px 。以下实例演示了为 h1 元素添加 myStyle 内联样式：
```javascript
var myStyle = {
	fontSize: 100,
	color: '#FF0000'
};
ReactDOM.render(
	<h1 style = {myStyle}>Example</h1>,
	document.getElementById('example')
);
```
##### c）、数组
JSX 允许在模板中插入数组，数组会自动展开所有成员：
```javascript
var arr = [
  	<h1>Example</h1>,
  	<h2>This is React demo</h2>,
];
ReactDOM.render(
  	<div>{arr}</div>,
  	document.getElementById('example')
);
```
#### 二、React 组件
##### a）、基本组件
```javascript
var HelloMessage = React.createClass({
render: function () {
    console.log(this.props)
    return <h1 title={this.props.title}>hello {this.props.name}</h1>;
}
});
ReactDOM.render(
    <HelloMessage name="Jack" title="My name is Jack"/>,
    document.getElementById('example')
)
```
React.createClass 方法用于生成一个组件类 HelloMessage。
注意：原生 HTML 元素名以小写字母开头，而自定义的 React 类名以大写字母开头，比如 HelloMessage 不能写成 helloMessage。除此之外还需要注意组件类只能包含一个顶层标签，否则也会报错。
如果需要向组件传递参数，可以使用 this.props 对象；
##### b）、复合（组合）组件
```javascript
var data = {
    name:'Jone',
    url:'http://www.cj365.cc/'
};
var Webstie = React.createClass({
    render:function () {
        return (
            <div>
                <Title title={this.props.title} name={this.props.name}/>
                <Nodelist url={this.props.url}/>
            </div>
        )
    }
})
var Title = React.createClass({
    render: function () {
        return <h1>Hello {this.props.title} '{this.props.name}'</h1>;
    }
});
var Nodelist = React.createClass({
    render: function () {
        return <p>My website is <a href={this.props.url}>{this.props.url}</a></p>;
    }
});
ReactDOM.render(
    <Webstie name={data.name} title="My name is" url={data.url}/>,
    document.getElementById('example')
)
```
#### 三、React State(状态)
React 把组件看成是一个状态机（State Machines）。通过与用户的交互，实现不同状态，然后渲染 UI，让用户界面和数据保持一致。
React 里，只需更新组件的 state，然后根据新的 state 重新渲染用户界面（不要操作 DOM）。
```javascript
var LikeButton = React.createClass({
    // 默认属性
    getDefaultProps: function () {
        return {
            bgColor: 'green',
            color: '#fff'
        };
    },
    getInitialState: function () {
        return {liked: false};
    },
    handleClick: function () {
        this.setState({
            liked: !this.state.liked
        });
        this.refs.p.style.backgroundColor = this.state.liked ? 'green' : 'red';
    },
    render: function () {
        var text = this.state.liked ? 'like' : 'haven\'t liked';
        return (
            <p onClick={this.handleClick} ref="p"
               style={{backgroundColor: this.props.bgColor, color: this.props.color}}>
                You {text} this. Click to toggle.
            </p>
        )
    }
});
ReactDOM.render(
    <LikeButton />,
    document.getElementById('example')
)
```
#### 四、React Props(属性)
React 组件可以通过 Props 来传递数据。state 和 props 主要的区别在于 props 是不可变的，而 state 可以根据与用户交互来改变。这就是为什么有些容器组件需要定义 state 来更新和修改数据。
```javascript
var HelloMessage = React.createClass({
render: function () {
    return <h1 title={this.props.title}>hello {this.props.name}</h1>;
}
});
ReactDOM.render(
    <HelloMessage name="Jack" title="My name is Jack"/>,
    document.getElementById('example')
)
```
##### a）、默认 Props
可以通过 getDefaultProps() 方法为 props 设置默认值，见代码：
```javascript
var HelloMessage = React.createClass({
    getDefaultProps: function() {
        return {
            name: 'Jack'
        };
    },
    render: function() {
        return <h1>Hello {this.props.name}</h1>;
    }
});

ReactDOM.render(
    <HelloMessage />,
    document.getElementById('example')
);
```
##### b）、Props 验证
Props 验证使用 propTypes，它可以保证我们的应用组件被正确使用，React.PropTypes 提供很多验证器 (validator) 来验证传入数据是否有效。当向 props 传入无效数据时，JavaScript 控制台会抛出警告。
```javascript
//PropTypes验证组件实例的类属性是否符合要求
var price = 5, name = "苹果";
var Component = React.createClass({
    // 默认属性
    getDefaultProps: function () {
        return {
            text: '你要多少斤？'
        };
    },
    propTypes: {
        title: React.PropTypes.number.isRequired,
        name: React.PropTypes.string.isRequired
    },
    render: function () {
        return <h1>{this.props.name}的价格是{this.props.title}元。{this.props.text}</h1>
    }
})
ReactDOM.render(
    <Component title={price} name={name}/>,
    document.getElementById("example")
)
```
#### 五、React 组件 API
* 组件有基本7种方法：
* 设置状态：setState
* 替换状态：replaceState
* 设置属性setProps
* 替换属性replaceProps
* 强制更新：forceUpdate
* 获取DOM节点：findDOMNode
* 判断组件挂载状态：isMounted

实例：
```javascript
var ComponentApi = React.createClass({
    getInitialState:function() {
        return {
            clickCount : 0
        }
    },
    handleClick:function(){
        this.setState({
            clickCount:this.state.clickCount+1
        })
    },
//        handleClick:function () {
//            this.setState(function (state) {
//                return {
//                    clickCount:state.clickCount+1
//                }
//            })
//        },
    render:function () {
        return (
            <h1 onClick={this.handleClick}>{this.props.content}{this.state.clickCount}</h1>
        )
    }
})
ReactDOM.render(
    <ComponentApi content="点击我,点击次数为"/>,
    document.getElementById('example')
)
```
#### 六、React 组件生命周期
组件的生命周期可分成三个状态：
* Mounting：已插入真实 DOM
* Updating：正在被重新渲染
* Unmounting：已移出真实 DOM

React 为每个状态都提供了两种处理函数，will 函数在进入状态之前调用，did 函数在进入状态之后调用，三种状态共计五种处理函数。

* componentWillMount()
* componentDidMount()
* componentWillUpdate(object nextProps, object nextState)
* componentDidUpdate(object prevProps, object prevState)
* componentWillUnmount()

此外，React 还提供两种特殊状态的处理函数。

* componentWillReceiveProps(object nextProps)：已加载组件收到新的参数时调用
* shouldComponentUpdate(object nextProps, object nextState)：组件判断是否重新渲染时调用
