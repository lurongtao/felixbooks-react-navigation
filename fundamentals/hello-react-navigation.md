# 初探 React Navigation

在web浏览器中，可以使用`<a>`标签链接到不同的页面。当用户单击一个链接时，URL被推送到浏览器历史堆栈中。当用户按下后退按钮时，浏览器会从历史堆栈的顶部弹出项目，因此活动页面现在是以前访问过的页面。React Native并不像web浏览器那样有一个内置的全局历史堆栈的概念。

React Navigation 的 stack navigator 为你的应用程序提供了在屏幕之间转换和管理导航历史的方法。如果您的应用程序只使用一个堆栈导航器，那么它在概念上与web浏览器处理导航状态的方式类似-当用户与它交互时，应用程序从导航堆栈中推送和弹出项目，这会导致用户看到不同的屏幕。这在web浏览器和React导航中的一个关键区别，React导航的stack navigator提供了在Android和iOS上在堆栈中的路由之间导航时所期望的手势和动画。

让我们从演示最常用的导航器 createStackNavigator 开始。

## 安装 stack navigator library
到目前为止，我们安装的库是导航器的构建块和共享基础，每个导航器都在自己的React Navigation 库中导航。要使用堆栈导航器，我们需要安装 @react navigation/stack：

```
npm install @react-navigation/stack
```

> @react navigation/stack 依赖于 @react native community/masked view和我们在《起步》中安装的其他库。如果您还没有安装，请转到《起步》并按照安装说明进行操作。

## 创建导航栈
createStackNavigator 是返回包含两个属性的对象的函数：Screen和Navigator。它们都是用于配置导航器的React组件。导航器应包含屏幕元素作为其子元素，以定义路由的配置。

NavigationContainer 是一个组件，它管理我们的导航树并包含导航状态。此组件必须包装所有导航器结构。通常，我们会在应用程序的根目录中定义这个组件，它通常是从app.js导出的组件。

```js
// 如 App.js
import * as React from 'react'
import { View, Text } from 'react-native'
import { NavigationContainer } from '@react-navigation/native'
import { createStackNavigator } from '@react-navigation/stack'

function HomeScreen() {
  return (
    <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
      <Text>Home Screen</Text>
    </View>
  );
}

const Stack = createStackNavigator()

function App() {
  return (
    <NavigationContainer>
      <Stack.Navigator>
        <Stack.Screen name="Home" component={HomeScreen} />
      </Stack.Navigator>
    </NavigationContainer>
  )
}

export default App
```

<div align=center>![demo](../_images/basic_stack_nav.png)

如果运行此代码，您将看到一个带有空导航栏和包含主屏幕组件的灰色内容区域的屏幕（如上图所示）。您在导航栏和内容区域中看到的样式是堆栈导航器的默认配置，我们稍后将学习如何配置这些样式。

> 路由名称的大小写无关紧要。你可以使用小写的 home 或大写的 Home。更推荐大家用大写的路由名称。

> screen 唯一需要的配置是name 和 props。

## 导航配置

所有路由配置都被指定为导航器的 props。我们没有向导航器传递任何 props，所以它只是使用默认配置。

让我们将第二个 screen 添加到堆栈导航器，并将主屏幕配置为首屏显示：

```js
function DetailsScreen() {
  return (
    <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
      <Text>Details Screen</Text>
    </View>
  )
}

const Stack = createStackNavigator()

function App() {
  return (
    <NavigationContainer>
      <Stack.Navigator initialRouteName="Home">
        <Stack.Screen name="Home" component={HomeScreen} />
        <Stack.Screen name="Details" component={DetailsScreen} />
      </Stack.Navigator>
    </NavigationContainer>
  )
}
```

现在我们的路由栈有两条路由，一条主页路由和一条详情页路由。可以使用 Screen 组件指定路由。Screen 组件接受一个 name 属性，该属性对应于我们将用于导航的路由的名称，以及一个组件属性，该属性对应于它将显示的组件。

这里，Home route对应于HomeScreen组件，Details route对应于DetailsScreen组件。路由栈的初始路由是主路由。尝试将其更改为详细信息并重新加载应用程序（React Native的快速刷新不会更新来自initialRouteName的更改，如您所料），请注意，您现在将看到详细信息屏幕。然后把它改回原位，重新装填。

> 注意:  component 属性值只能是组件, 不是渲染函数 (如不能赋值：component={() => <HomeScreen />})。

## 制定 options 选项
导航器中的每个 Screen 都可以为导航器指定一些选项，例如要在标题中显示的标题。这些选项可以在每个 Screen 组件的选项 options 中传递：

```js
<Stack.Screen
  name="Home"
  component={HomeScreen}
  options={{ title: 'Overview' }}
/>
```

有时我们想为导航器所有的 Screen 指定相同的 options，为此我们可以将`screenOptions` 属性传递给导航器。

## 传递附加的属性

有时我们可能想把附加的属性传给Screen。我们可以通过两种方法做到这一点：
1. 使用React context，用上下文提供组件包装导航器来将数据传递到Screen（推荐）。
2. 给 Screen 传递渲染函数(render functions), 而不是指定 component 属性：

```js
<Stack.Screen name="Home">
  {props => <HomeScreen {...props} extraData={someData} />}
</Stack.Screen>
```

> 注意：默认情况下，React Navigation 对 Screen 组件应用优化以防止不必要的渲染。使用渲染函数将删除这些优化。因此，如果使用渲染函数，则需要确保对屏幕组件使用React.memo 或 React.PureComponent，以避免性能问题。

## 总结

* React Native没有像web浏览器那样的内置导航API。React Navigation为您提供了这一功能，同时还提供了iOS和Android手势和动画，以便在屏幕之间切换。

* Stack.Navigator 是一个组件，它将路由配置作为它的子级，并使用附加的配置 props 来渲染我们的内容。

* 每个Stack.Screen组件都有一个name属性，它引用路由的名称，而component属性指定要为路由渲染的组件。这是两个必需的属性。

* 要指定路由栈中的初始路由，请使用initialRouteName属性配置。

* 要指定特定于屏幕的选项，我们可以将选项属性传递给Stack.screen，对于常用选项，我们可以将 screenOptions 传递给Stack.Navigator。