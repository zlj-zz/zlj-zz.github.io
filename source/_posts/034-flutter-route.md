---
title: Flutter 路由小谈
date: 2020-11-17 11:38
category: Flutter
---

## 直接跳转

```java
import 'newpage.dart';

Navigator.of(context).push(
    MaterialPageRoute(builder: (context)=>NewPage())
);
```

> 如果跳转的页面需要参数，可以直接传入。

## 初始化执行路线

定义路由集合

```java
final routes = {
  '/': (context) => Loading(),
  '/home': (context) => Home(),
  '/settings': (context) => Settings(),
  '/update': (context) => UpdatePage(),
};
```

使用路由

```java
void main() {
 runApp(MarsApp());
}

class MarsApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
	return MaterialApp(
	  initialRoute: '/',
	  routes: routes,
	);
  }
}
```

页面跳转

```java
Navigator.pushNamed(context, '/stats');
```

## `pushAndRemoveUntil`

这个方式是跳转到某个页面，然后移除路由直到`...`为止

```java
// 普通使用
Navigator.of(context).pushAndRemoveUntil(
    MaterialPageRoute(builder: (context) => MyPage()), (route) => route == null);

// 路由方式
Navigator.pushNamedAndRemoveUntil(context, '/', (route) => route == null);
```

上面的两种使用方式都可以达到禁止返回上一级的作用，应为这里给出的是 `route == null`, 所以会将路由栈里的元素全部移除。

```java
Navigator.of(context).pushNamedAndRemoveUntil('/setting',ModalRoute.withName('/'));
```

上面的方式可以在跳转到`setting`页面后，若返回则直接到`/`页面。

## `pushReplancementNamed`

使用该方法跳转页面，会在路由栈中用新页面替换老页面。这时完成跳转后返回则好似返回上上个页面。

```java
Navigator.of(context).pushReplacementNamed('/page');
```

## `popUntil`

```java
Navigator.popUntil(context,ModalRoute.withName('/'));
```

与 `pushAndRemoveUntil` 配套，退回到某个指定页面。

## `popAndPushNamed`

与`pushReplacementNamed`方法类似，其执行结果栈是的路由栈与`pushReplacementNamed`方法的效果是一致的。
