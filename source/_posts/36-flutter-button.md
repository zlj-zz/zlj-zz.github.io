---
title: flutter 按钮用法小记
date: 2020-11-17 15:12
category: Flutter
---

# Flutter 按钮

知识是人类进步的阶梯，总结笔记就是...... 额，管他的呢，挺好。那今天就聊聊 Flutter 中的按钮组件。

Flutter 中已经定义好了一些默认的按钮组件供我们直接使用，所以先请它们登场

1. `RaisedButton`: 凸起的按钮
2. `FlatButton`: 扁平的按钮
3. `OutlineButton`: 边框按钮
4. `IconButton`: 图标按钮

> 前面三个都继承自 `MaterialButton`，最后一个继承自 `StatelessWidget`。

为了去使用它们，那得再了解了解其内在（属性），下面就给出一张表：

| 属性              | 值类型                        | 说明                                                                                        |
| ----------------- | ----------------------------- | ------------------------------------------------------------------------------------------- |
| onPressed         | VoidCallback                  | 一般接收一个方法 必填参数，按下按钮时触发的回调，传 `null` 表示按钮禁用，会显示禁用相关样式 |
| child             | Widget                        | 文本控件                                                                                    |
| textColor         | Color                         | 文本颜色                                                                                    |
| color             | Color                         | 按钮的颜色                                                                                  |
| disabledColor     | Color                         | 按钮禁用时的颜色                                                                            |
| disabledTextColor | Color                         | 按钮禁用时的文本颜色                                                                        |
| splashColor       | Color                         | 点击按钮时水波纹的颜色                                                                      |
| highlightColor    | Color                         | 点击（长按）按钮后按钮的颜色                                                                |
| elevation         | double                        | 阴影的范围，值越大阴影范围越大                                                              |
| padding           | EdgeInsetsGeometry （抽象类） | 内边距                                                                                      |
| shape             | ShapeBorder（抽象类）         | 设置按钮的形状                                                                              |
| minWidth          | double                        | 最小宽度                                                                                    |
| height            | double                        | 高度                                                                                        |

上面这些属性呢，就是大多按钮都有的。但是，有那么几个我们的单独说一下（怕你不会用啊）。

**padding**

内边距，其接收值的类型是`EdgeInsetsGeometry`类型的，是一个抽象类。所以一般都用`EdgeInsets`类中的方法来设置，先看看这个类中的常用方法。

```dart
//单独设置左上右下的间距，四个参数都要填写
const EdgeInsets.fromLTRB(this.left, this.top, this.right, this.bottom);

//单独设置左上右下的间距，四个均为可选参数
  const EdgeInsets.only({
    this.left = 0.0,
    this.top = 0.0,
    this.right = 0.0,
    this.bottom = 0.0
  });

//一次性设置上下左右的间距
const EdgeInsets.all(double value)
  : left = value, top = value, right = value, bottom = value;
```

看到这些方法，想必大家应该知道怎么用了吧，这里我就不举例了哈。（啥？有人说不会。唉！那我就写一个吧）

```dart
RaisedButton(
      onPressed: null,
      child: Text("按钮"),
      color: Colors.blue,
      textColor: Colors.white,
      padding: EdgeInsets.only(top: 30),
    );
```

**shape**

shape 用来设置按钮的形状，其接收值是 ShapeBorder 类型，也是一个抽象类。它的实现类还是蛮多的，我们主要来看看常用的就好了。

- `BeveledRectangleBorder` 六边形
- `CircleBorder` 圆形
- `RoundedRectangleBorder` 圆角矩形
- `StadiumBorder` 两端是半圆

它们主要用的属性就两个，`side` 和 `borderRadius`

`side` 接收一个 `BorderSide` 类型的值。

```dart
const BorderSide({
    this.color = const Color(0xFF000000), // 颜色
    this.width = 1.0,                     // 宽度
    this.style = BorderStyle.solid,       // 样式
})
```

`borderRadius` 接收一个 `BorderRadius` 类型的值，常用方法也不多。

```dart
BorderRadius.all()        // 配置所有方向
BorderRadius.cricular()   // 环形配置，跟all效果差不多，直接接收double类型的值
BorderRadius.horizontal() // 只配置左右方向
BorderRadius.only()       // 可选左上，右上，左下，右下配置
BorderRadius.vertical()   // 只配置上下方向
```

**OutlineButton**

`OutlineButton` 是一个有默认边线且背景透明的按钮，也就是说我们设置其边线和颜色是无效的。

**IconButton**

`IconButton` 是直接继承自 **StatelessWidget **的，默认没有背景,所以有个 `icon` 参数是必填的。下面给出参考代码

```dart
IconButton(
      icon: Icon(Icons.home),
      onPressed: null,
      color: Colors.blueAccent,
      highlightColor: Colors.red,
    );
```

Icon 除了用 flutter 提供的之外还可以使用图片去自定义图形图标，下面也会给出参考代码

> **叮** 图片一定要 png，加载出来是纯色的背景图片哦

```dart
Widget getImageIcon(){
    return ImageIcon(
          AssetImage('images/test.png'),
          color: Colors.red,
          size: 50,
        );
}

IconButton(
      icon: getImageIcon(),
      onPressed: null,
    );
```

当然也可以使用图片

```dart
Widget getImage() {
    return new Image.asset(
      'images/test.png',
      width: 300.0,
      height: 300.0,
      matchTextDirection: false,
      fit: BoxFit.fitWidth,
    );
}
```
