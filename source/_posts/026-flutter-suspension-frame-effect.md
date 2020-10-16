---
title: Flutter 悬浮弹出效果
date: 2020-08-06
category: Flutter
---

最近在 Flutter 的使用中需要做一个悬浮弹窗的效果。通过查阅资料了解 `Overlay` 可以做出悬浮框的效果，但没有弹出效果，不符合我的预期。后来发现其实并不一定要真的悬浮，通过 `showModalBouttonSheet`方法和 `Stack`组件，可以模拟出这样的效果。

```java
  Future _createSelectViewWithContext() async {
    //屏幕宽高
    RenderBox renderBox = context.findRenderObject();
    var screenSize = renderBox.size;
    final option = await showModalBottomSheet(
        context: context,
        builder: (BuildContext context) {
          return StatefulBuilder(builder: (context, state) {
            return Stack(
                alignment: AlignmentDirectional.topCenter,
                children: <Widget>[
                  Container(
                    color: Colors.black54,
                    height: screenSize.height * 0.37,
                    width: double.infinity,
                  ),
                  Container(
                    height: screenSize.height * 0.35,
                    width: screenSize.width * 0.95,
                    decoration: BoxDecoration(
                        color: Colors.white,
                        borderRadius: BorderRadius.all(Radius.circular(20.0))),
                    child: Column(
                      children: <Widget>[
                        Text(
                          'This is a title',
                          style: TextStyle(color: Colors.grey, fontSize: 25),
                        ),
                        Divider(),
                        Expanded(
                            child: ListView(
                          children: <Widget>[
                            ListTile(
                              leading: Icon(Icons.home),
                              title: Text('home'),
                              trailing: RaisedButton(
                                  onPressed: () {},
                                  color: Colors.blue,
                                  shape: RoundedRectangleBorder(
                                      borderRadius: BorderRadius.circular(20)),
                                  child: Text('botton')),
                            ),
                            ListTile(
                              leading: Icon(Icons.hotel),
                              title: Text('hotel'),
                              trailing: RaisedButton(
                                  onPressed: () {},
                                  color: Colors.blue,
                                  shape: RoundedRectangleBorder(
                                      borderRadius: BorderRadius.circular(20)),
                                  child: Text('botton')),
                            ),
                          ],
                        ))
                      ],
                    ),
                  )
                ]);
          });
        });
  }
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200806162244613.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4NDEwNDk0,size_16,color_FFFFFF,t_70)
