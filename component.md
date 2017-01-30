# 模块划分

接下来我们来进行模块划分，首先我们可以看到在项目根目录有2个index文件，分别是：

* index.ios.js：iOS App主文件
* index.android.js Android App主文件

我们删除掉原来的代码，改成下面的代码：

```
// 导入AppRegistry模块
import {
  AppRegistry
} from 'react-native';

// 导入我们的App模块
import App from "./app";

// 注册App
AppRegistry.registerComponent('todo', () => App);
```

我们在项目根目录创建三个js文件，分别是：

* app.js：iOS和Android通用主文件
* header.js：Header模块
* footer.js：Footer模块

接下来我们分别创建这三个文件。
