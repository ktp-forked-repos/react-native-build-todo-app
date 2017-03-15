# 更新todo内容

在之前的章节里，我们已经开发了一个小的todo app，我们可以增加、删除、完成/未完成 todo items，我们还开发了一个过滤器用于筛选不同状态的todo列表。今天我们将增加一个新功能，即编辑修改todo的内容。

### 模块化编辑或者显示

> 思路：我们给每个todo增加一个新的属性：`editing`，如果这个属性为`true`则表示正在编辑状态，反之则为非编辑状态。在编辑状态时我们显示一个编辑的模块，在非编辑状态时我们显示现有的模块，我们称之为"显示"模块。

首先，我们将在row.js里，导入`TextInput`这个控件。

```
import {View, Text, StyleSheet, Switch, TouchableOpacity, TextInput} from "react-native";
```

将现有的todo内容和删除按钮模块化：

```
const textComponent = (
  <View style={styles.textWrap}>
    <Text style={[styles.text, complete && styles.complete]}>{this.props.text}</Text>
  </TouchableOpacity>
);

const removeButton = (
  <TouchableOpacity onPress={this.props.onRemove}>
    <Text style={styles.remove}>X</Text>
  </TouchableOpacity>
);
```

然后，增加一个新的编辑状态的模块：

```
const editingComponent = (
  <View style={styles.textWrap}>
    <TextInput
      autoFocus
      multiline
      value = {this.props.text}
      onChangeText = {this.props.onUpdate}
      style={styles.input}
    />

  </View>
);
```

### 增加处理方法

我们需要有2个方法，一个用于处理update todo text，另一个用于处理切换编号和非编辑状态。在app.js里，我们增加这2个方法：

```
handleUpdateText(key, text) {
  const newItems = this.state.items.map((item) =>{
    if (item.key !== key) return item;
    return {
      ...item,
      text
    }
  });
  this.setSource(newItems, filterItems(this.state.filter, newItems));
}

handleToggleEditing(key, editing) {
  const newItems = this.state.items.map((item) =>{
    if (item.key !== key) return item;
    return {
      ...item,
      editing
    }
  });
  this.setSource(newItems, filterItems(this.state.filter, newItems));
}
```

### 将2个处理方法传给Row组件

和之前我们将`handleComplete`方法传给Row组件一样，我们将这2个新的处理方法传递给Row组件，同时不要忘记先将这2个新的方法`bind(this)`。

```
<Row
  key={key}
  onRemove = {() => this.handleRemoveItem(key)}
  onComplete = {(complete) => this.handleToggleComplete(key, complete)}
  onToggleEdit = {(editing) => this.handleToggleEditing(key, editing)}
  onUpdate = {(text) => this.handleUpdateText(key, text)}
  {...value}
/>
```

### 切换编辑和非编辑状态

回到row.js，我们需要给"显示"控件增加一个长按方法，当我长按这个todo的时候，表示我想编辑这个todo，我们需要将`<View>`换成`<TouchableOpacity>`

```
const textComponent = (
  <TouchableOpacity style={styles.textWrap} onLongPress={() => this.props.onToggleEdit(true)} >
    <Text style={[styles.text, complete && styles.complete]}>{this.props.text}</Text>
  </TouchableOpacity>
);
```

现在我们运行一下app

![](/assets/editingstatus.png)

最后，我们需要在编辑状态时增加一个按钮，切换回非编辑状态，增加一个`doneButton`控件：

```
const doneButton = (
  <TouchableOpacity style={styles.done} onPress={() => this.props.onToggleEdit(false)}>
    <Text style={styles.doneText}>Save</Text>
  </TouchableOpacity>
);
```

在Row的`render`方法里，我们使用`this.props.editing`来判断是显示编辑状态还是非编辑状态。

```
{this.props.editing ? editingComponent : textComponent}
{this.props.editing ? doneButton : removeButton}
```

以下为最终的运行结果

![](/assets/editingstatus2.png)

今日代码：
https://github.com/zhiwehu/todo/tree/editing
