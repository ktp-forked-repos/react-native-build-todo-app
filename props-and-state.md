#属性和状态

在接着开发我们的App之前，我们需要理解一下什么是属性（Props）和状态（State）

###属性(Props)
Props很好理解，一个组件在创建的时候，都会有一些参数，比如长宽高等，这些参数就称为属性，比如html里的`<Img>`，我们知道有一个`src`的属性。属性一般是在父组件中创建子组件时传值，而传值之后在子组件的生命周期中值不会发生变化。对于需要改变的数据，我们可以使用状态（State）。属性可以一个数值，也可以是一个回调函数，这点比较重要，我们后面的例子中大量使用将回调函数传给子组件的属性。

我们来看一个属性的例子：

```
import React, { Component } from 'react';
import { AppRegistry } from 'react-native';

import Button from './button';

class Demo extends Component {
  onClick() {
    // 点击处理事件
  }
  
  render() {
    return (
      <View>
        <Button
          text='This is a button'
          onClick={this.onClick.bind(this)}
        />
      </View>
    );
  }
}

AppRegistry.registerComponent('Demo', () => Demo);
```

###状态(State)
State是组件里需要变化的数据，比如一个组件在初始导入数据的时候，我们需要显示一个loading的效果，而当数据导入完成，这个loading效果会消失掉，我们就可以使用一个boolean型的state来实现这一点。State一般是在组件的构造函数里进行初始化，使用`this.setState()`方法来更新状态值。
