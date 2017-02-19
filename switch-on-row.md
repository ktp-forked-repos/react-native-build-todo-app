# 使用Switch来修改todo item的状态

今天我们将使用React Native的[Switch](https://facebook.github.io/react-native/docs/switch.html)控件来修改一个todo item的状态。

首先修改row.js文件，导入Switch

```
import {View, Text, StyleSheet, Switch} from "react-native";
```

然后使用这个Switch控件，非常简单，我们只要将todo.complete属性传给Switch的`value`属性即可。注意我们在上节中，将整个todo item的属性都传给了Row的props，所以这里只需要使用`this.props.complete`即可。

```
<Switch
  value = {this.props.complete}
/>
```

我们需要给Switch的onValueChange绑定一个事件的处理方法，我们并不在row.js里定义这个事件的处理方法，而是和之前的方式一样，将这个方法通过props传递进去。

```
<Switch
  value = {this.props.complete}
  onValueChange = {this.props.onComplete}
/>
```

这个`onComplete`是在app.js里传递给row的。
```
<Row
  key={key}
  onComplete = {(complete) => this.handleToggleComplete(key, complete)}
  {...value}
/>
```

我们来定义这个`handleToggleComplete`方法：
```
handleToggleComplete(key, complete) {
  const newItems = this.state.items.map((item) => {
    if (item.key !== key) return item;
    return {
      ...item,
      complete
    }
  });

  this.setSource(newItems, newItems);
}
```

这里使用了Array.map方法，遍历todo list里每一条todo，如果发现其key和传递进来的key相同，使用ES6的`...`操作符来更新`complete`状态。

以下是运行结果。
![](/assets/switch todo complete.png)

代码：https://github.com/zhiwehu/todo/tree/switch
