# 使用ActivityIndicator制作一个loading效果

在本课程里，我们将使用[`ActivityIndicator`](https://facebook.github.io/react-native/docs/activityindicator.html)来制作一个loading效果，就是当app启动的时候，需要从AsyncStorage或者是云端API获取数据，在获取数据这段时间内，我们在app界面上显示一个loading效果，当数据获取成功（或者是失败）之后，这个loading效果自动消失。

### 导入ActivityIndicator

```
import {View, Text, StyleSheet, Platform, ListView, Keyboard, AsyncStorage, ActivityIndicator} from "react-native";
```

### 制作一个loading效果

我们将使用一个`View`来包含一个`ActivityIndicator`配合`StyleSheet`来实现这个loading效果。

```
<View style={styles.loading}>
  <ActivityIndicator
    size="large"
    animating
  />
</View>
```

这个`styles.loading`实现如下：

```
loading: {
  position: "absolute",
  top: 0,
  left: 0,
  right: 0,
  bottom: 0,
  alignItems: "center",
  justifyContent: "center",
  backgroundColor: "rgba(0,0,0,0.2)"
}
```

效果如下：

![](/assets/ActivityIndicator.png)

### 自动显示和消失

我们希望在app启动的时候，这个loading效果自动出现，load完成之后自动消失。我们可以使用一个state值来实现这一点。

增加一个`state`变量`loading`，初始值为`true`，表示一开始要出现loading

```
this.state = {
  loading: true,
  filter: "ALL",
  value: "",
  items: [],
  dataSource: ds.cloneWithRows([])
};
```
然后我们在load数据完成后将这个state值设置为`false`

```
componentWillMount() {
  AsyncStorage.getItem("items").then(json => {
    try {
      const items = JSON.parse(json);
      this.setSource(items, items, {loading: false});
    } catch (e) {
      this.setState({loading: false});
    }
  });
}
```
最后，我们使用这个`state.loading`变量来判断是否要显示这个loading View。

```
{this.state.loading && <View style={styles.loading}>
  <ActivityIndicator
    size="large"
    animating
  />
</View>}
```

本节代码：https://github.com/zhiwehu/todo/tree/ActivityIndicator
