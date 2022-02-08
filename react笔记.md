# react基础
## 引入

            <!-- 引入react核心库 -->
            <script type="text/javascript" src="../js/react.development.js"></script>
            <!-- 引入react-dom，用于支持react操作DOM -->
            <script type="text/javascript" src="../js/react-dom.development.js"></script>
            <!-- 引入babel，用于将jsx转为js -->
            <script type="text/javascript" src="../js/babel.min.js"></script>

## 虚拟DOM的两种创建方式
* 关于虚拟DOM
	* 1.本质是Object类型的对象（一般对象）
	* 2.虚拟DOM比较“轻”，真实DOM比较“重”，因为虚拟DOM是React内部在用，无需真实DOM上那么多的属性。
	* 3.虚拟DOM最终会被React转化为真实DOM，呈现在页面上。
            
### 1.使用jsx创建虚拟DOM  √

            <!-- 准备好一个“容器” -->
	        <div id="test"></div>
            <script type="text/babel" > /* 此处一定要写babel */
                //1.创建虚拟DOM
                const VDOM = (  /* 此处一定不要写引号，因为不是字符串 */
                    <h1 id="title">
                        <span>Hello,React</span>
                    </h1>
                )
                //2.渲染虚拟DOM到页面
                //ReactDOM.render(虚拟DOM,容器)
                ReactDOM.render(VDOM,document.getElementById('test'))
            </script>
### 2.使用js创建虚拟DOM   ×

            <!-- 不用引入babel，只引入前两个 --> 
            <!-- 准备好一个“容器” -->
            <div id="test"></div>
            <script type="text/javascript" > 
                //1.创建虚拟DOM
                const VDOM = React.createElement('h1',{id:'title'},React.createElement('span',{},'Hello,React'))//嵌套复杂
                //2.渲染虚拟DOM到页面
                ReactDOM.render(VDOM,document.getElementById('test'))
            </script>

## jsx语法规则
* 1.定义虚拟DOM时，不要写引号。
* 2.标签中混入JS表达式时要用{}。

            一定注意区分：【js语句(代码)】与【js表达式】
					1.表达式：一个表达式会产生一个值，可以放在任何一个需要值的地方
								下面这些都是表达式：
										(1). a
										(2). a+b
										(3). demo(1)
										(4). arr.map() 
										(5). function test () {}
					2.语句(代码)：
								下面这些都是语句(代码)：
										(1).if(){}
										(2).for(){}
										(3).switch(){case:xxxx}

* 3.样式的类名指定不要用class，要用className。
* 4.内联样式，要用style={{key:value}}的形式去写。
    * style={{color:'white',fontSize:'29px'}}
* 5.只有一个根标签
* 6.标签必须闭合
    * <input type="text"/>
* 7.标签首字母
    * (1).若小写字母开头，则将该标签转为html中同名元素，若html中无该标签对应的同名元素，则报错。
    * (2).若大写字母开头，react就去渲染对应的组件，若组件没有定义，则报错。
## react中定义的组件
### 函数式组件   适用简单组件

            <script type="text/babel">
                //1.创建函数式组件
                function MyComponent(){
                    console.log(this); //此处的this是undefined，因为babel编译后开启了严格模式
                    return <h2>我是用函数定义的组件(适用于【简单组件】的定义)</h2>
                }
                //2.渲染组件到页面
                ReactDOM.render(<MyComponent/>,document.getElementById('test'))
                /* 
                    执行了ReactDOM.render(<MyComponent/>.......之后，发生了什么？
                            1.React解析组件标签，找到了MyComponent组件。
                            2.发现组件是使用函数定义的，随后调用该函数，将返回的虚拟DOM转为真实DOM，随后呈现在页面中。
                */
            </script>
### 类式组件    适用复杂组件

            <script type="text/babel">
                //1.创建类式组件        
                class MyComponent extends React.Component {//必须继承 Component是内置的类
                    //render必须有，也必须有return，展示什么返回什么
                    render(){
                        //render是放在哪里的？—— MyComponent的原型对象上，供实例使用。
                        //render中的this是谁？—— MyComponent的实例对象 <=> MyComponent组件实例对象。
                        console.log('render中的this:',this);
                        return <h2>我是用类定义的组件(适用于【复杂组件】的定义)</h2>
                    }
                }
                //2.渲染组件到页面
                ReactDOM.render(<MyComponent/>,document.getElementById('test'))
                /* 
                    执行了ReactDOM.render(<MyComponent/>.......之后，发生了什么？
                            1.React解析组件标签，找到了MyComponent组件。
                            2.发现组件是使用类定义的，随后new出来该类的实例，并通过该实例调用到原型上的render方法。
                            3.将render返回的虚拟DOM转为真实DOM，随后呈现在页面中。
                */
            </script>