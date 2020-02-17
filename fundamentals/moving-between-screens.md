# 页面间跳转
在上一节 [Hello React Navigation](./hello-react-navigation.md)中，我们定义了一个具有两个路由（Home和Details）的导航器，但是我们没有学习如何让用户从Home导航到Details。

如果是浏览器，我们可以写这样的东西：

```html
<a href="details.html">Go to Details</a>
```

或者：

```html
<a
  onClick={() => {
    window.location.href = 'details.html';
  }}
>
  Go to Details
</a>
```

我们将实现类似第二种操作，但我们将使用传递到屏幕组件的导航属性，而不是使用window.location 全局选项。

## 导航到新窗口

```js
import * as React from 'react';
import { Button, View, Text } from 'react-native';
import { NavigationContainer } from '@react-navigation/native';
import { createStackNavigator } from '@react-navigation/stack';

function HomeScreen({ navigation }) {
  return (
    <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
      <Text>Home Screen</Text>
      <Button
        title="Go to Details"
        onPress={() => navigation.navigate('Details')}
      />
    </View>
  );
}

// ... 其他代码
```

解释一下：

* navigation：在路由栈的定义的组件(component属性定义的组件)，默认会传给定义的每个组件一个 navigation 属性。

* navigate('Details')：我们调用 navigate 函数，函数参数是在导航属性定义的名字（注意这里用字符串直接引用路由名字）

> 如果调用 navigation.navigate 时使用的路由名称不是在堆栈导航器上定义的，则不会发生任何事情。换句话说，我们只能导航到堆栈导航器上定义的路由，我们不能导航到任意组件。

现在我们有了一个包含两条路由的堆栈：1）主路由 2）详细页路由。如果我们从详情页再次导航到详情页，会发生什么情况？

## 多次导航同一路由

```js
function DetailsScreen({ navigation }) {
  return (
    <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
      <Text>Details Screen</Text>
      <Button
        title="Go to Details... again"
        onPress={() => navigation.navigate('Details')}
      />
    </View>
  );
}
```

如果您运行此代码，您将注意到当您点击 “转到详细信息…”，你会发现它什么也做不了！这是因为我们已经在详情页路由上。如果你已经在那个屏幕上，那么它将什么也不会做。

假设我们真的想导航到另一个详细页。这在向每个路由传递一些唯一数据的情况下非常常见（稍后我们讨论params时会详细介绍）。为此，我们可以将“导航”更改为“推送”。这使我们能够表达添加另一条路线的意图，而不管现有的导航历史。

```js
<Button
  title="Go to Details... again"
  onPress={() => navigation.push('Details')}
/>
```

<div align=center>![导航demo](/_images/push_stack_nav-demo.gif)

每次调用push时，我们都会向导航堆栈中添加一条新路由。当您调用导航时，它首先试图找到具有该名称的现有路由，如果栈上还没有一个路由，则只推一条新路由。

## 返回

当从活动页面返回时，stack navigator 提供的标题将自动包含一个后退按钮（如果导航堆栈中只有一个屏幕，则没有可返回的内容，因此没有后退按钮）。

有时您希望能够以编程方式触发此行为，为此您可以使用 `navigation.goBack();`

```js
function DetailsScreen({ navigation }) {
  return (
    <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
      <Text>Details Screen</Text>
      <Button
        title="Go to Details... again"
        onPress={() => navigation.push('Details')}
      />
      <Button title="Go to Home" onPress={() => navigation.navigate('Home')} />
      <Button title="Go back" onPress={() => navigation.goBack()} />
    </View>
  );
}
```

> 在 Android 设备上，React Navigation 会勾住硬件“后退”按钮，当用户按下“后退”按钮，它会为您触发goBack()函数，因此它的行为与用户期望的一样。

另一个常见的需求是能够跨页面返回：例如，如果您在一个堆栈中有多个页面，并且希望关闭所有这些页面以返回到第一个页面。在这种情况下，我们就可以使用navigate('Home') （而不是push！试试看区别）。另一种选择是 `navigation.popToTop()`，它返回到堆栈中的第一个页面。

```js
function DetailsScreen({ navigation }) {
  return (
    <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
      <Text>Details Screen</Text>
      <Button
        title="Go to Details... again"
        onPress={() => navigation.push('Details')}
      />
      <Button title="Go to Home" onPress={() => navigation.navigate('Home')} />
      <Button title="Go back" onPress={() => navigation.goBack()} />
      <Button
        title="Go back to first screen in stack"
        onPress={() => navigation.popToTop()}
      />
    </View>
  );
}
```

## 总结

* navigation.navigate('RouteName') 如果新路由不在堆栈中，则将其推送到堆栈导航器，否则将跳到该页面。

* 我们可以随意多次调用 `navigation.push('RouteName')`，它将继续推送路由。

* 标题栏将自动显示后退按钮，但您可以通过调用 `navigation.goBack()` 以编程方式返回。在 Android 上，硬件后退按钮正如预期的那样工作。

* 你可以在导航中返回到堆栈中的现有页面。navigation('RoutNeMy')，你可以返回到堆栈中的第一个页面。

* 导航 props 可用于所有 Screen 定义的组件。