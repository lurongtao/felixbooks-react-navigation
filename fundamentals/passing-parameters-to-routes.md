# 路由传参

现在我们知道了如何创建带有某些路由的堆栈导航器，以及如何在这些路由之间导航，下面让我们看看如何在导航到路由时将数据传递给它们。

这里有两个部分：

通过将参数作为 navigation.navigate 函数的第二个参数放入到一个对象中，将参数传递给路由：navigation.navigate('RouteName', {/*参数*/})

读取Screen组件route.params中的参数。

```js
function HomeScreen({ navigation }) {
  return (
    <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
      <Text>Home Screen</Text>
      <Button
        title="Go to Details"
        onPress={() => {
          /* 1. 携带参数后导航到Details页面 */
          navigation.navigate('Details', {
            itemId: 86,
            otherParam: 'anything you want here',
          })
        }}
      />
    </View>
  );
}

function DetailsScreen({ route, navigation }) {
  /* 2. 在DetailsScreen组件里获取参数 */
  const { itemId } = route.params
  const { otherParam } = route.params
  return (
    <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
      <Text>Details Screen</Text>
      <Text>itemId: {JSON.stringify(itemId)}</Text>
      <Text>otherParam: {JSON.stringify(otherParam)}</Text>
      <Button
        title="Go to Details... again"
        onPress={() =>
          navigation.push('Details', {
            itemId: Math.floor(Math.random() * 100),
          })
        }
      />
      <Button title="Go to Home" onPress={() => navigation.navigate('Home')} />
      <Button title="Go back" onPress={() => navigation.goBack()} />
    </View>
  )
}
```

<div align=center>![demo](/_images/passing_params.png)

组件内部还可以更新其参数，就像它们可以更新其状态一样。navigation.setParams 方法允许您更新页面的参数。有关更多详细信息，请参阅[setParams](https://reactnavigation.org/docs/en/navigation-prop.html#setparams---make-changes-to-route-params)的API参考。

还可以向页面传递一些初始参数。如果导航到此页面时未指定任何参数，则将使用初始参数。它们还与你传递的任何参数浅合并。可以使用Initial params属性指定初始参数：

```js
<Stack.Screen
  name="Details"
  component={DetailsScreen}
  initialParams={{ itemId: 42 }}
/>
```

参数不仅对于将某些数据传递到新页面有用，而且对于将数据传递到上一个页面也很有用。例如，假设你有一个带有create post按钮的页面，create post按钮打开一个新的页面来创建post。创建post后，需要将post的数据传递回上一个页面。

为了实现这一点，可以使用导航方法，如果屏幕已经存在，它会像 goBack 一样。可以使用 navigate 传递参数以将数据传递回：

```js
function HomeScreen({ navigation, route }) {
  React.useEffect(() => {
    if (route.params?.post) {
      // 提交信息更新, 利用 `route.params.post` 参数做些事情
      // 比如, 传递 post 数据给服务器
    }
  }, [route.params?.post]);

  return (
    <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
      <Button
        title="Create post"
        onPress={() => navigation.navigate('CreatePost')}
      />
      <Text style={{ margin: 10 }}>Post: {route.params?.post}</Text>
    </View>
  );
}

function CreatePostScreen({ navigation, route }) {
  const [postText, setPostText] = React.useState('');

  return (
    <>
      <TextInput
        multiline
        placeholder="What's on your mind?"
        style={{ height: 200, padding: 10, backgroundColor: 'white' }}
        value={postText}
        onChangeText={setPostText}
      />
      <Button
        title="Done"
        onPress={() => {
          // 返回首页同时携带post参数
          navigation.navigate('Home', { post: postText });
        }}
      />
    </>
  );
}
```

在这里，点击“完成”按钮后，主页的 route.params 将更新，以反映在 navigate 中传递的 post 文本。

## 总结

* navigate 和 push 接受可选的第二个参数，以便将参数传递到要导航到的路由。例如：navigation.navigate('RouteName'，{paramName:'value'})。

* 可以通过页面内的 route.params 读取参数

* 可以使用navigation.setParams更新页面的参数

* 初始参数可以通过 Screen 上的 Initial params 属性传递