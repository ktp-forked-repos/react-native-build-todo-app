# 搭建开发环境

```
开发React Native，建议使用MacOS，这样可以同时开发iOS版本和Android版本。以下环境是在Mac OS上搭建的，如果是其他操作系统请参考[React Native官方文档](https://facebook.github.io/react-native/docs/getting-started.html#content)。
```

### 安装Node.js

在MacOS上安装Node.js最方便的方法是使用[Homebrew](http://brew.sh/)。Homebrew是MacOS上一款软件包管理器，如果你的电脑上尚未安装Homebrew，可以在终端上运行以下命令进行安装：

```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

```
使用Homebrew安装Node.js，在终端上运行以下命令：
```

```
brew install node
```

```
React Native开发建议安装[Watchman](https://facebook.github.io/watchman/)，这是一个Facebook开发维护的工具，它可以监测到代码文件的变化，从而可以方便的进行自动重新编译运行新代码。
```

```
brew install watchman
```

### 安装react-native-cli

```
Node.js安装后会自带[npm](https://www.npmjs.com/)，npm是JavaScript的软件包管理器。我们使用npm来安装React Native命令行接口：
```

```
npm install -g react-native-cli
```

#### 小结

* Homebrew是MacOS软件包管理器，用它来安装Node.js和Watchman（可选）。
* npm是JavaScript软件包管理器，用它来安装react-native-cli（React Native命令行接口）

---



