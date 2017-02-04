# 使用TextInput

接下来，我们将使用[TextInput](https://facebook.github.io/react-native/docs/textinput.html)作为todo的输入框，将todo item加到todo list里。

首先，思考一下我们要怎么对数据进行管理。对于整个app来说，我们需要有2个状态(state)值，一个用于存储当前正在输入的todo，一个用于存储所有的todo item，这2个状态在app初始状态的时候，都是空的（对于todo list值来说，后面我们需要导入之前持久存储的数据，这个我们后面再讲），当我们在TextInput中输入字符的时候，会更新当前正在输入的todo value；当我们按下键盘上的"done"按钮的时候，会将这个todo value增加到todo list里，同时将这个value清空。

### 2个state
1. value: 存储当前正在输入的todo，TextInput onChangeText事件会更新这个状态
2. items: 存储所有todo list，TextInput onSubmitEditing事件会更新这个状态，同时将value状态设置为空。

### header.js
TextInput是需要放在Header里面的，以下是header.js的新代码：

```
// 引入React和Component
import React, {Component} from "react";
// 引入Text，显示文字
import {View, Text, StyleSheet, TextInput} from "react-native";

// 定义Header类，这个类是Component的子类
class Header extends Component {
  /*
   实现Header类的render方法，这个方法返回一个View，显示Footer
   */
  render() {
    return (
      <View style={styles.header}>
        <TextInput
          placeholder="什么需要做?"
          value={this.props.value}
          onChangeText={this.props.onChange}
          onSubmitEditing={this.props.onAddItem}
          blurOnSubmit={false}
          returnKeyType="done"
          style={styles.input}
        />
      </View>
    );
  }
}

// 创建StyleSheet
const styles = StyleSheet.create({
  header: {
    paddingHorizontal: 16,
    flexDirection: "row",
    justifyContent: "space-around",
    alignItems: "center"
  },
  input: {
    flex: 1,
    height: 50
  }
});

// 导出这个模块，供外部调用
export default Header;

```
我们可以看到，我们在Header是创建了一个TextInput，并且把值和处理方法都放在Header的props中传给了这个TextInput，也就是意味着我们不需要在Header中写代码来处理app的逻辑，这部分代码我们统一放在app.js里来做。

我们对Header传了三个props:
> 1. value，是一个值，就是app用于存储当前正在输入的todo value
> 2. onChange，一个回调函数，用于TextInput onChangeText的时候，更新app.state.value
> 3. onAddItem，一个回调函数，当TextInput onSubmitEditing（提交）的时候，更新app.state.items，并将app.state.value设置为空。

接下来我们在app.js里实现这部分代码。

### 初始化state

在App类的构造方法里，初始化state

```
  // 构造方法,初始化state
  constructor(props) {
    super(props);
    // 初始化2个状态
    this.state = {
      value: "",
      items: []
    };
  }
```

### TextInput onSubmitEditing回调函数
```
  /*
   传给Header.TextInput.onSubmitEditing的回调函数
   更新this.state.items
   设置this.state.value为空
   */
  handleAddItem() {
    if (!this.state.value) return;
    // 创建一个新的items,从this.state.items里copy现有的数据,再增加一个新的
    const newItems = [
      ...this.state.items,
      {
        key: Date.now(),
        text: this.state.value,
        complete: false
      }
    ];
    // 更新state
    this.setState({
      items: newItems,
      value: ""
    });
  }
```

> 我并没有直接往this.state.items里增加一条新数据，而是重新创建了一个新的items，从老的items里copy了原有数据，并且增加了一条新数据。这样做的好处是让react native的shouldComponentUpdate性能更好，从而更加快速的知道一个组件是否有状态变化，从而重新render数据。详情参考：https://facebook.github.io/react/docs/optimizing-performance.html

### 传值给Header props
接下来就是给Header props传值了
```
  <Header
    value = {this.state.value}
    onAddItem = {this.handleAddItem.bind(this)}
    onChange = {(value) => this.setState({value})}
  />
```
运行结果如下：
![](/assets/add TextInput.png)

当然，现在按下Done增加一个新的todo，我们的app没有任何变化，我们将在下一篇中讲解如果显示todo list。

本节代码：https://github.com/zhiwehu/todo/tree/second


