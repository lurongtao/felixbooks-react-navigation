# 起步

如果您已经熟悉React Native，那么你将能够快速使用React导航！如果没有，您可能需要先阅读[React Native Express](http://www.reactnativeexpress.com/)的第1到4节（包括第1到4节），完成后再回到这里。

本文档的基础知识部分将介绍React导航的最重要方面。它应该足以让您了解如何构建典型的小型移动应用程序，并为你提供深入了解React导航更高级部分所需的基础知识。

## 安装

在React Native项目中安装所需的软件包：

```
npm install @react-navigation/native
```

React Navigation由一些核心实用程序组成，应用在RN中，可以实现路由导航功能。

我们要安装的模块有 react-native-gesture-handler, react-native-reanimated, react-native-screens,react-native-safe-area-context

### 在 expo 项目里安装

```
npm install react-native-gesture-handler react-native-reanimated react-native-screens react-native-safe-area-context @react-native-community/masked-view
```

### 在纯 React Native 项目中安装

在项目根目录下，运行：

```
npm install react-native-reanimated react-native-gesture-handler react-native-screens react-native-safe-area-context @react-native-community/masked-view
```

> 在安装依赖包时可能抛出一些警告，这通常是模块的版本匹配问题。你可以暂时忽略。

From React Native 0.60 and higher, linking is automatic. So you don't need to run react-native link.

自从 React Native 0.60 或更高版本后，会自动做link。因此你不需要运行 react-native link。

If you're on a Mac and developing for iOS, you need to install pods to complete the linking. Make sure you have Cocoapods installed. Then run:

如果你开发环境是Mac的iOS系统，你需要安装pods来完成link。确保安装了`Cocoapods`。然后运行：

```
cd ios; pod install; cd ..
```

要完成react native手势处理程序的安装，请入口文件（例如index.js或App.js）的顶部添加以下内容（确保它位于文件内容的顶部，并且前面没有其他内容）：

```
import 'react-native-gesture-handler';
```

> 注意：如果你跳过这一步，你的应用程序可能会在生产中崩溃，即使它在开发中运行良好。

Now, we need to wrap the whole app in NavigationContainer. Usually you'd do this in your entry file, such as index.js or App.js:

现在，我们需要将整个组件包裹在 NavigationContainer 组件中。通常入口文件（如index.js或App.js）中执行此操作：

```js
import * as React from 'react';
import { NavigationContainer } from '@react-navigation/native';

export default function App() {
  return (
    <NavigationContainer>
      {/* 其他应用代码 */}
    </NavigationContainer>
  );
}
```

> 注意: 当你使用 navigator (比如 navigator 路由栈), 你需要安装路由系统其他的依赖。 如果系统出现 "Unable to resolve module" 错误, 那么就需要在系统中安装这些模块。

现在，你可以运行你的程序在手机或模拟器上了。