# 使用AsyncStorage存储数据

在之前的示例中，当我们每次重新运行程序时，原先的todo list就丢失了。在今天的课程里，我们将使用[AsyncStorage](https://facebook.github.io/react-native/docs/asyncstorage.html)来存储数据，这样只要不卸载程序，即便重新运行程序，数据也不会丢失。

### 导入AsyncStorage

在app.js里，我们导入AsyncStorage

```
import {View, Text, StyleSheet, Platform, ListView, Keyboard, AsyncStorage} from "react-native";
```

### 使用AsyncStorage

我们将在App类的`componentWillMount`方法里使用`AsyncStorage`将数据取出来，`componentWillMount`在组件的生命周期中，当组件即将`Mount`的时候会自动调用。下面一个图可以简单了解一下React Native的组件生命周期。

![](http://busypeoples.github.io/img/lifecycle_init.png)

也就是在`render()`调用之前，react native会调用其`componentWillMount`方法。

```
componentWillMount() {
  AsyncStorage.getItem("items").then(json => {
    try {
      const items = JSON.parse(json);
      this.setSource(items, items);
    } catch (e) {

    }
  });
}
```

### 更新AsyncStorage

当我们增加、修改、删除了todo之后，我们需要将数据更新回AsyncStorage，我们只需要在我们的`setSource`里做这个步骤就可以了。

```
/*
 一个通用的setSource方法,方便调用
 */
setSource(items, itemsDatasource, otherState = {}) {
  this.setState({
    items,
    dataSource: this.state.dataSource.cloneWithRows(itemsDatasource),
    ...otherState
  });
  AsyncStorage.setItem("items", JSON.stringify(items));
}
```

我们在今天的代码里使用了`JSON.parse()`方法和`JSON.stringify()`方法，parse方法是将一个json格式的字符串转成一个对象，stringify则相反，将一个对象转成json格式的字符串。

今天的代码：https://github.com/zhiwehu/todo/tree/AsyncStorage
