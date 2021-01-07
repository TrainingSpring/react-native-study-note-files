# 搭建开发环境

- [下载jdk](https://www.oracle.com/java/technologies/javase/javase-jdk8-downloads.html)  Ctrl+鼠标左键打开链接

- 配置环境变量
- ![image-20201230103341776](.\image-20201230103341776.png) 
- ![image-20201230104101726](.\image-20201230104101726.png) 

- [安装Android studio](https://developer.android.google.cn/studio/)

- 安装Android studio的时候 选择custom 安装sdk等工具

- 配置sdk环境变量

  - ![image-20201230104152242](.\image-20201230104152242.png) 
  - ![image-20201230104227143](.\image-20201230104227143.png) 

- 安装 react native模块

  - ```shell 
    npm install react -g -save
    npm install react-native -g -save
    ```

- 创建RN项目

  - ```shell
    npx react-native init project-name  # 创建普通项目
    npx react-native init project-name --template react-native-template-typescript  # 创建带有typescript配置的项目
    npx react-native init project-name --version X.XX.X  # 指定版本
    ```

# 第一次启动

- 准备Android设备
  
  - 可以是真机 也可以是模拟器
  - 模拟器可以是Android studio提供的  也可以是其他第三方模拟器,如[Genymotion](https://www.genymotion.com/download)、BlueStack 
  - 官方模拟器免费,且功能完善 但性能较差
  
- 启动项目

  - 进入项目目录

  - ```shell
    yarn android
    # 或者
    yarn react-native run-android
    ```

## 报错处理

- 如果报"没有安装环境变量"的错误 <font color="red">react native Failed to install the app. Make sure you have the Android development environment set...</font>
  - 检查Android SDK是否配置成功
  - 检查jre是否配置成功
  - 模拟器是否开启
  - 是否连接了模拟器  
- 如果还报错  那就用Android Studio打开项目 ,Android Studio会自动检查错误
- 如果安卓studio报Could not open cp_init remapped class cache for 679impwcp2nsmss9dbzs5238f...错误  大概率是jre版本过高
  - https://www.oracle.com/java/technologies/oracle-java-archive-downloads.html 去下载历史版本的jre试试
- AS启动完成了, 用命令再启动试试  或者直接使用AS调试

# 目录结构

- "\__test\__" 测试文件
- "android"/"ios" 原生项目生成目录  基本不会操作
- "node_modules" node包
- "index.js" 入口文件
- "app.json"  配置文件
- "metro.config.js"  Facebook的工程构建工具  不用管

# 开发

## 引入模块

1. 必要的时react组件

2. react-native组件

   ```javascript
   import React, {Component} from 'react';   // 引入react组件
   import {
     SafeAreaView,
     StyleSheet, // 样式对象
     ScrollView,
     View, // 相当于div
     Text, // 相当于span, 文本必须卸载Text组件中
     StatusBar,
   } from 'react-native'; // 从react-native中引入原生插件
   ```

## 页面布局示例

```react
// 1. 第一种方式
const App = ()=>{
    return(
    <>
        <View style="styles.view">
        	<Text>这里放文字</Text>
        </View>
    </>
    )
}
// 第二种方式,class的方式
class App extends React.Components{
    render(){
        return (
            {/*设置样式对象*/}
            <View style={style.container}>
            	<View style={[style.txt,{color:"red"}]}>
                	<Text>没有样式继承的,每一个组件都要单独设置样式</Text>
                	<Text>传入一个数组, 会自动合并样式</Text>
                </View>
                {/*这里的双括号理解为:在单括号中 包含了一个JSON对象{}*/}
                <View style={{width:100,backgroundColor:"#eee"}}>创建样式对象</View>
            </View>
        )
    }
}
export default App; // 最终要将App导出去
```

## 编写样式

```react
// 1. Stylesheet对象
const styles = Stylesheet.crate({
    view:{
        height:200,
        width:200,
        backgroundColor:"#ccc" // 驼峰命名法
    }
})
// 2. 直接在标签中使用
const App = ()=>{
    return(
    <>
        <View style={{height:100,width:100,marginTop:20}}>
        	<Text style={{color:"red"}}>这里放文字</Text>
        </View>
    </>
    )
}
// 弹性布局  与css中的flex类似
const styles = Stylesheet.crate({
    view:{
        flex:1,
        flexDerection:"row",
        justifyContent:"space-around"
    }
})
```

## 文本Text组件

[文档地址](https://reactnative.cn/docs/text)

## 组件状态 state

- 组件的state 类似于Vue的data
- setState 更改状态并更改视图

```React
import React,{Component} from "React"
class AApp extends Component{
    state={
        a:1,
        b:2
    }
	updateState=()=>{
        const a = this.state.a===1?2:1;
        this.setState({a})
    }
    render(){
        return (
        	<View onPress={this.updateState}>
                <Text>{a}</Text>
            </View>
        )
    }
}
```



## 组件属性 props

- 给子组件传递数据

```react
// ... 省略导入React
class ChildComponent extends Component{
    constructor(props){
        super(props);
        this.state={
            a:props.a
        }
    }
    updateState(){
        const a = this.state.a === 1?0:1;
        this.setState({a:a})
    }
    render(){
        const a = this.state.a
        return(
        <View>
        	<Text onPress={updateState}>{a}</Text>    
        </View>)
    }
}
class Parent extends Component{
    render(){
        return(
        	<View>
        		<ChildComponent a={1}></ChildComponent>
            </View>
        )
    }
}
default export Parent;
```

## 输入组件 textInput

[文档地址](https://reactnative.cn/docs/textinput)

# redux 

- 一个状态管理的组件
- 不仅仅支持react  普通的js, angular等等都可以使用redux
- 平常的js的状态管理使一层一层的递进管理  
- redux 则是直接通过store 更改对应层的状态
  - ![image-20210106162540251](.\image-20210106162540251.png) 
- 例子:
  - 学生(ReactComponent) 去图书馆借书, 首先找到管理员(action creaturs) , 告诉管理员要借什么书, 然后图书管理员去到图书馆(store) , 用管理软件(Reducers) 找到书籍的具体位置  再将书给学生 

## store

- 首先是store仓库

- 创建一个基本的store

- ``` React
  import {createStore} from 'redux'
  import reducer from "./reducer.js" // 假装下面的reducer在reducer.js里面
  const store = createStore(reducer) // 将reducer放到仓库中  相当于买了个管理工具
  export default store;
  ```

- 理论上说 这样就完成了

## reduce

- reducer 管理工具

- reducer只能接收state 不能改变state

- 一个简单的reducer

- ``` react
  const defaultData = { // 默认数据
      value:"默认数据", 
      list:[1,2,3,4,5,8]
  }
  export default (state = defaultData, action)=>{
      if(action.type === "TEST_TYPE"){
          // 处理数据 由于reducer只能接收state, 不能改变state
          // 所以可以深度拷贝一个
  
          let newSate = JSON.parse(JSON.stringfy(state));
          newState.value = action.value;
          return newState;
          // 当然 上面这样写比较low 可以使用ES6的方式
          return {
              ...state,
              [action.storeData]:{
                  ...state[action.storeName],
                  value:action.value,
                  title:action.title
              }
          }
      }
      return state;
  }
  ```

## action 

- action是一个对象 

- action里面的type是必须的, 用type来指定更改哪个状态

- 然后可以有其他参数, 也就是数据了

- 有了action还不行  若要更改action  必须使用dispatch函数进行改变

- 如:

- ``` react 
  let action = {
      type:"TEST_TYPE",
      storeData:{
          title:"数据",
           value:"123"
      }
  }
  store.dispatch(action);
  ```

- dispatch 过后  store直接将action给了reducer, reducer根据action返回对应的状态, 然后store来处理

- action中的type因为特殊的原因,  可以写在一个单独的文件中防止写错和复用

# 中间件

## thunk	

- ``` shell 
  npm install --save redux-thunk // 安装thunk
  ```

- store中引入中间件

- ```react
  import {creatStore,applyMiddleware, compose} from "redux"; // 引入中间件
  import thunk from "redux-thunk";  // 引入thunk
  // thunk 的使用
  const store = creatStore(reducer,applyMiddleware(thunk))
  ```

- [thunk的使用](https://www.bilibili.com/video/BV1w441137ss?p=16)

- 调用了中间件  就可以在action中加入业务逻辑了

- ```react
  import axios from "axios"
  const otherAction = (data)=>{type:"test",data}
  const demo1 = ()=>{
      return (dispatch)=>{
          axios.get("地址").then((res)=>{
              const data = res.data;
              const action = otherAction(data);
              dispatch(action);
          })
      }
  }
  ```

- 

## saga

``` shell 
npm install --save redux-saga // 安装saga
```

引入

``` react
import {creatStore,applyMiddleware, compose} from "redux"; // 引入中间件
import saga from "redux-saga";  // 引入thunk
```

[saga教程地址](https://www.bilibili.com/video/BV1w441137ss?p=18)

[saga教程地址2](https://www.bilibili.com/video/BV1w441137ss?p=19)

# react-redux

``` shell 
npm install --save react-redux
```

## Provider 	

- provider 包裹的内容都可以正确的使用store的内容

- 如:

- ``` react
  import {Provider} from "react-redux"; // 提供器
  import store from "./store"
  import TestDom from "./testdom.js";
  const App = (
  	<Provide store={store}>
      	<TestDom></TestDom>
      </Provide>
  )
  ReactDom.render(App,document.getElementById('root'))
  ```

- 

## connect 

- 连接器
- 参数1 映射关系  将state映射到props

```react 
import React, {Component} from "react"
import {View,Text,Image,ImageBackground,ActivityIndicator,StyleSheet} from "react-native";
import {SCREEN_WIDTH,SCREEN_HEIGHT} from "../../static/js/static"
import {connect} from "react-redux"; // 连接器
type Props = {};
const bgi = require("../../static/images/welcome/bg.png");
class Welcome extends React.Component<Props>{
    constructor(props: Props | Readonly<Props>){
        super(props);

    }

    render(): React.ReactElement<any, string | React.JSXElementConstructor<any>> | string | number | {} | React.ReactNodeArray | React.ReactPortal | boolean | null | undefined {

        return (
            <View style={styles.container}>
               <Image source={bgi} style={styles.bg} onclick="clickImage"></Image>
                <ActivityIndicator color={"#ccc"} style={styles.loading} />
            </View>
        )
    }
}
const stateToProps=(state)=>{
    return{
        value:state.value
    }
}
export default connect(stateToProps,null)(WelCome);
```

- 参数2 映射方法

- ```react
  const dispatchToProps=(dispatch)=>{
      return {
          clickImage(e){
              let action = {type:"test"}
              dispatch(action)
          }
      }
  }
  ```

- 