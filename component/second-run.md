# 第二次运行

![](/assets/Second Run.png)

### StyleSheet
我们需要修改一下样式，
1. Header把系统状态栏档住了
2. Content需要弹性高度

修改app.js代码，导入StyleSheet

```
import { View, Text, StyleSheet } from "react-native";
```

使用StyleSheet.create()创建一个styles对象
```
const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: "#F5F5F5",
    paddingTop: 30,
  },
  content: {
    flex: 1
  }
});
```

然后，分别给最外层的View以及content的View设置style属性。
```
  render() {
    return (
      <View style={styles.container}>
        <Header />

        <View style={styles.content}>
          <Text>我是Content</Text>
        </View>

        <Footer />
      </View>
    );
  }
```
再次运行，结果如下：
![](/assets/third run.png)

看一下在Android上运行的样子：
![](/assets/android third run.png)

注意，在Android上运行的时候，Header离状态栏距离太大了，因为Android不需要设置paddingTop而不会档住状态栏，所以这个paddingTop只需要给iOS设置就可以了。接下来我们来使用Platform来实现这个功能。

### Platform

```
import { View, Text, StyleSheet, Platform } from "react-native";
```
```
const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: "#F5F5F5",
    ...Platform.select({
      ios: {
        paddingTop: 30
      }
    })
  },
  content: {
    flex: 1
  }
});
```
再来看看Android的效果
![](/assets/android forth run.png)

代码：https://github.com/zhiwehu/todo/tree/first
