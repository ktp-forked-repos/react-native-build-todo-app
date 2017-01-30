# app.js

```
// 引入React和Component
import React, { Component } from "react";
// 引入View，类似于html的Div
import { View, Text, } from "react-native";
// 引入我们的Header模块
import Header from "./header";
// 引入我们的Footer模块
import Footer from "./footer";

// 定义App类，这个类是Component的子类
class App extends Component {
  /*
  实现App类的render方法，这个方法返回一个View，
  其组成分别是Header, Content和Footer
  */
  render() {
    return (
      <View>
        <Header />
        <View>
          <Text>我是Content</Text>
        </View>
        <Footer />
      </View>
    );
  }
}

// 导出这个模块，供外部调用
export default App;
```

App类的render()方法是App模块的渲染方法，该方法返回一个View模块，其组成是Header和Footer，中间是一个子View