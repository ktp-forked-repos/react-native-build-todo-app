#属性和状态

在接着开发我们的App之前，我们需要理解一下什么是属性（Props）和状态（State）

###属性(Props)
Props很好理解，一个组件在创建的时候，都会有一些参数，比如长宽高等，这些参数就称为属性，比如html里的`<Img>`，我们知道有一个`src`的属性。属性一般是在父组件中创建子组件时传值，而传值之后在子组件的生命周期中值不会发生变化。对于需要改变的数据，我们可以使用状态（State）。属性可以是一个数值，也可以是一个回调函数，这点比较重要，我们后面的例子中将大量使用将回调函数传给子组件的属性。

我们来看一个属性的例子：我们将导入一个自定义的Button，然后传入了2个属性，其中一个是字符串，而另一个是回调函数。

```
import React, { Component } from 'react';
import { AppRegistry, View } from 'react-native';

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

button.js
我们使用了TouchableOpacity来实现这个自定义的Button，里面的Text读取了`this.props.text`的值，而`onPress`方法则使用了属性里传过来的`this.props.onClick`方法。这样我们就把你组件的方法传给了子组件。
```
import React, { Component } from 'react';
import { View, TouchableOpacity, Text } from 'react-native';

class Button extends Component {
  render() {
    return (
      <View>
        <TouchableOpacity onPress={this.props.onClick}>
          <Text>{this.props.text}</Text>
        </TouchableOpacity>
      </View>
    );
  }
}

export default Button;

```

在线示例：https://rnplay.org/apps/wKOMyQ

###状态(State)
State是组件里需要变化的数据，比如一个组件在初始导入数据的时候，我们需要显示一个loading的效果，而当数据导入完成，这个loading效果会消失掉，我们就可以使用一个boolean型的state来实现这一点。State一般是在组件的构造函数里进行初始化，使用`this.setState()`方法来更新状态值。

我们来看一个实现loading效果的例子
* 在constructor()方法里，设置`this.state.loading`为`true`
* 在componentWillMount()方法里，执行具体的loading操作，完成后将loading设置为false：`this.setState({loading:false})`
* 在render()方法里，根据`this.state.loading`来判断是否显示loading

```
import React, { Component } from 'react';
import { View, Text } from 'react-native';

class Loading extends Component {
  constructor(props){
    super(props);
    this.state = {
      loading: true
    }
  }
  
  componentWillMount() {
    //loading,完成后，将this.state.loading设置为false
    this.setState({
      loading: false
    })
  }
  
  render() {
    return (
      <View>
        {this.state.loading && <View style={styles.loading}>
          <Text>Loading...</Text>
        </View>}
      </View>
    );
  }
}

export default Loading;

```

