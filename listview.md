# 使用ListView

在上一节中，我们已经通过TextInput将todo item加到了`app.state.items`中，本节将讨论如何通过ListView来显示这些todo items。

[ListView](https://facebook.github.io/react-native/docs/listview.html)是React Naitve的一个核心组件，用于高效率的显示纵向滚动的可变化数据列表。使用ListView一般需要：

1. 创建一个[ListView.DataSource](https://facebook.github.io/react-native/docs/listview.html)，并将列表数据填入。我们一般将这个dataSource放在app的state中，这样当数据变化时，app会重新渲染ListView。
2. 创建一个ListView，设置其dataSource属性为刚刚创建的dataSource，并实现其renderRow方法。

接下来我们通过代码来实现上面的2点。

### 引入ListView

```
// 引入ListView
import {..., ListView, ...} from "react-native";
```

### 创建ListView.DataSource并初始化数据

```
// 创建ListView.DataSource
// 实现rowHasChanged方法，这里认为只要新老两条数据不相同即为变化
const ds = new ListView.DataSource({rowHasChanged: (row1, row2) => row1 !== row2});

// 将ListView放在state中，这里使用cloneWithRows方法来从一个空列表克隆数据
this.state = {
  ...
  dataSource: ds.cloneWithRows([])
};
```

### 初始化ListView

```
<ListView
  enableEmptySections
  dataSource={this.state.dataSource}
  renderRow={(item) => {
    return (
      <View key={item.key}>
        <Text>{item.text}</Text>
      </View>
    )
  }}
/>
```

### 更新dataSource
我们需要在add item到items的时候，同时更新dataSource，我们只需要调用`this.setState()`方法即可。在app.js的`handleAddItem`方法里更新dataSource

```
handleAddItem() {
  ...
  const newItems = ...
  // 更新state
  this.setState({
    ...
    dataSource: this.state.dataSource.cloneWithRows(newItems)
  });
}
```

运行结果如下：

![](/assets/ListView_first_run.png)

### 使用新的Row组件

为了以后扩展方便，我们将创建一个新的Row组件，用于显示ListView中的每一行数据。

row.js

```
import React, {Component} from "react";

import {View, Text, StyleSheet} from "react-native";

class Row extends Component {
  render() {
    return (
      <View style={styles.container}>
        <View style={styles.textWrap}>
          <Text style={styles.text}>{this.props.text}</Text>
        </View>
      </View>
    )
  }
}

const styles = StyleSheet.create({
  container: {
    padding: 10,
    flexDirection: "row",
    flex: 1,
    justifyContent: "space-between",
    alignItems: "flex-start"
  },
  textWrap: {
    flex: 1,
    marginHorizontal: 10
  },
  text: {
    fontSize: 24,
    color: "#4d4d4d"
  }
});

export default Row;

```

### 更新app.js代码

在app.js里，更新ListView的代码如下：
```
// 引入Keyboard
import {View, Text, StyleSheet, Platform, ListView, Keyboard} from "react-native";

// 引入Row
import Row from "./row";
...

<ListView
  style={styles.list}
  enableEmptySections
  dataSource={this.state.dataSource}
  onScroll={() => Keyboard.dismiss()}
  renderRow={({key, ...value}) => {
    return (
      <Row
        key={key}
        {...value}
      />
    )
  }}
  renderSeparator={(sectionId, rowId) => {
    return <View key={rowId} style={styles.separator}/>
  }}
/>

// ListView的style如下：
...
list: {
  backgroundColor: '#FFF'
},
separator: {
  borderWidth: 1,
  borderColor: "#F5F5F5"
}
...
```

> [enableEmptySections](https://facebook.github.io/react-native/docs/listview.html#enableemptysections)，在未来的react native版本中，ListView将默认渲染空section headers，所以这里设置这个参数。
> `onScroll={() => Keyboard.dismiss()}`会在ListView手指滚动的时候，将输入框隐藏起来，给用户更好的体验
> `({key, ...value})`使用ES6的...来将item的属性动态的传给Row组件
> [renderSeparator](https://facebook.github.io/react-native/docs/listview.html#renderseparator)用于渲染数据行的间隔横线

再次运行，是不是感觉样式更美观了呢？
![](/assets/ListView_second_run.png)

### 待改进的地方
更新dataSource我们可以写一个通用的方法，这样以后在增加、修改、删除以及过滤查询时都可以很方便的调用

```
/*
 一个通用的setSource方法,方便调用
 */
setSource(items, itemsDatasource, otherState = {}) {
  this.setState({
    items,
    dataSource: this.state.dataSource.cloneWithRows(itemsDatasource),
    ...otherState
  })
}
```
这样我们只需要在调用这个方法来更新state了。
```
handleAddItem() {
    ...
    // 更新state
    this.setSource(newItems, newItems, {value: ""})
  }
```
本节代码：https://github.com/zhiwehu/todo/tree/listview
