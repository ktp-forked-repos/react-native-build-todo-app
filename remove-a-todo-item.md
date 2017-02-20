# 删除todo item

本小节我们来实现如何删除一条todo item。首先我们需要使用[TouchableOpacity](https://facebook.github.io/react-native/docs/touchableopacity.html)控件来做一个删除按钮。

我们在row.js里，在todo item的文字后面增加一个新的删除按钮，注意`TouchableOpacity`的`onPress`事件，绑定的方法是从外层传递进来的。

```
<TouchableOpacity onPress={this.props.onRemove}>
    <Text style={styles.remove}>X</Text>
</TouchableOpacity>
```

接下来在app.js里，我们定义这个方法，使用Array.filter方法来将需要删除的item从列表中剔除出来。

```
handleRemoveItem(key) {
  const newItems = this.state.items.filter((item) => {
    return (item.key !== key);
  });
  this.setSource(newItems, newItems);
}
```

接下来，将这个方法传给Row控件

```
<Row
  key={key}
  onRemove = {() => this.handleRemoveItem(key)}
  onComplete = {(complete) => this.handleToggleComplete(key, complete)}
  {...value}
  />
                
```

运行结果如下：![](/assets/delete todo item.png)

### method.bind(this)

也许你注意到了，我在app.js的App类的构造方法里，将我们定义的方法，通过bind(this)来重新设置了一下，为什么我们需要这样做呢？

```
this.setSource = this.setSource.bind(this);
this.handleAddItem = this.handleAddItem.bind(this);
this.handleToggleComplete = this.handleToggleComplete.bind(this);
this.handleRemoveItem = this.handleRemoveItem.bind(this);
```

比如我们今天的方法`handleRemoveItem`是App这个类的方法，在JavaScript中，类方法是默认不绑定的，所以在`handleRemoveItem`这个类方法里，如果我们不将this绑定的话，我们便无法在这个方法里使用`this`，否则会报`this`会`undefined`。

建议参考这两篇文章：
1. 官网介绍：https://facebook.github.io/react/docs/handling-events.html
2. [React Binding Patterns: 5 Approaches for Handling `this`](https://medium.com/@housecor/react-binding-patterns-5-approaches-for-handling-this-92c651b5af56#.tpe59c2ym)
