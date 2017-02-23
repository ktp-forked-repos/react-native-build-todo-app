# 筛选todo list

在本小节里，我们将在footer里增加几个按钮，用于筛选todo list，分别是显示所有的todo，显示没有完成的todo以及显示所有完成的todo。

在footer.js里，我们将导入TouchableOpacity来生成几个按钮。

```
// 定义Footer类，这个类是Component的子类
// 定义Footer类，这个类是Component的子类
class Footer extends Component {
  /*
   实现Header类的render方法，这个方法返回一个View，显示Footer
   */
  render() {
    return (
      <View style={styles.container}>
        <View style={styles.filters}>
          <TouchableOpacity style={styles.filter} onPress={() => this.props.onFilter('ALL')}>
            <Text>全部</Text>
          </TouchableOpacity>
          <TouchableOpacity style={styles.filter} onPress={() => this.props.onFilter('ACTIVE')}>
            <Text>未完成</Text>
          </TouchableOpacity>
          <TouchableOpacity style={styles.filter} onPress={() => this.props.onFilter('COMPLETE')}>
            <Text>已完成</Text>
          </TouchableOpacity>
        </View>
      </View>
    );
  }
}

```

对于每个按钮，我们会绑定onPress事件一个方法，和之前一样，我们将不在footer.js里去实现这个方法，而是通过props从上层组件中传递进来。

在app.js里，我们将来实现onPress事件上绑定的方法。

首先我们需要创建一个工具方法，这个工具方法将在很多地方通用，这个工具方法传递进去2个参数 ：`filter`和`todos`，`filter`表示筛选条件，可以是`ALL`,`ACTIVE`或者是`COMPLETE`中的一个值，分别代表了全部，未完成以及全部完成的todo。`todos`表示需要进行筛选的todo列表。

这个方法如下：

```
const filterItems = (filter, todos) => {
  return todos.filter(todo => {
    if (filter === 'ALL') return true;
    if (filter === 'ACTIVE') return !todo.complete;
    if (filter === 'COMPLETE') return todo.complete;
  });
}
```

然后在App component里定义一个方法：

```
handleFilter(filter) {
    this.setSource(this.state.items, filterItems(filter, this.state.items), {filter: filter})
  }
```

这个方法非常简单，当用户点击一个筛选条件的时候，将filter传递过来，使用我们上面定义的工具方法，得到筛选过后的列表，调用我们之前定义的 `setSource`方法，重新设置`app.state`的值，注意setSource第三参数，我们需要将用户这时的`filter`传递给`app.state.filter`，所以我们要在state里增加一个新的变量`filter`，默认值是`ALL`。

```
// 初始化状态
    this.state = {
      filter: "ALL",
      value: "",
      items: [],
      dataSource: ds.cloneWithRows([])
    };
```

接下来我们只需要在app.js里调用Footer组件里，将这个方法传递给Footer组件。

```
<Footer
  onFilter = {this.handleFilter}
/>
```

> 别忘记在App的constructor方法里，使用`bind`将`handleFilter`方法`bind(this）`

```
this.handleFilter = this.handleFilter.bind(this);
```

OK现在我们的filter已经起作用了，但是经过测试，我们发现当你增加一个新todo，或者是删除一个todo，或者是改变一个todo的状态时，我们的列表又重新显示所有的数据了，为了解决这个问题，我们只需要在对应的`handleAddItem`，`handleRemoveItem`以及`handleToggleComplete`这几个方法里，对`setSource`方法里，调用我们的工具方法`filterItems`来筛选数据再传给`app.state`。

```
this.setSource(newItems, filterItems(this.state.filter, newItems));
```

最后，我们将`state.filter`也传给Footer组件，用于显示当前的filter是什么。

```
<Footer
  filter = {this.state.filter}
  onFilter = {this.handleFilter}
/>
```

在Footer组件里，可以使用`this.props.filter`来获取当前的`filter`，使用style来显示一个外边框，来突出显示当前选中的filter。

```
render() {
  const {filter} = this.props;
  return (
    <View style={styles.container}>
      <View style={styles.filters}>
        <TouchableOpacity style={[styles.filter, filter === 'ALL' && styles.selected]} onPress={() => this.props.onFilter('ALL')}>
          <Text>全部</Text>
        </TouchableOpacity>
        <TouchableOpacity style={[styles.filter, filter === 'ACTIVE' && styles.selected]} onPress={() => this.props.onFilter('ACTIVE')}>
          <Text>未完成</Text>
        </TouchableOpacity>
        <TouchableOpacity style={[styles.filter, filter === 'COMPLETE' && styles.selected]} onPress={() => this.props.onFilter('COMPLETE')}>
          <Text>已完成</Text>
        </TouchableOpacity>
      </View>
    </View>
  );
}
```
运行结果如下。
![](/assets/filter todo list.png)

本节代码：https://github.com/zhiwehu/todo/tree/filter

