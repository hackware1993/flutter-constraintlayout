# Flutter ConstraintLayout

[English](https://github.com/hackware1993/flutter-constraintlayout/blob/master/README.md)

一个超级强大的 Stack，使用约束构建极为灵活的布局，和 Android 下的 ConstraintLayout 和 iOS 下的 AutoLayout 类似。

# 大幅提高 Flutter 的开发体验和效率，并提升应用性能

不管布局有多复杂，约束有多深，它始终有媲美单一 Flex 或 Stack
的性能，在面对复杂的布局时，它能提供更好的性能，更大的灵活性，更少的代码，以及非常扁平的代码层次结构，大大提升代码的可维护性。对”嵌套地狱“说不。

总之一句话，用了就回不去了。

改善”嵌套地狱“是我开发 Flutter ConstraintLayout 的初衷之一，但我不推崇极致地追求一层嵌套，这是不必要的。因此像链这种特性，Flex 本身已经很好的支持了，因此
ConstraintLayout 不会积极支持它。

查看 [Flutter Web 在线示例](https://constraintlayout.flutterfirst.cn)

**Flutter ConstraintLayout 有极高的布局性能。它不基于 Cassowary 算法，无需线性方程求解。推荐在顶层使用
ConstraintLayout。对于极端复杂的布局（1000 个子元素，2000 个约束），非首帧布局和绘制的总耗时在 5 毫秒内（在 Windows 10
调试模式下，发布模式耗时更少），理论上首帧优势会更明显。对于常规复杂布局（50 个子元素，100 个约束），帧率可轻松达到 200 fps。**

**如非必要，尽量相对于 parent 布局，这样可以定义更少的 id，或者使用相对 id。**

**警告**:
为了布局性能的考虑，约束总是单向的，不允许存在两个子元素相互约束对方（比如 A 的右边约束在 B 的左边，而 B 的左边又反过来约束在 A
的右边）。每一个约束都应该确切的描述子元素是如何定位的。尽管约束只能单向，但你仍然能更好的处理以前双向约束才能做到的事情，比如链（暂时还未支持，请结合 Flex 使用）。

你可以使用以下的”嵌套地狱“代码恶心任何给你发送骚扰信息的人:

```dart
class NestedHell extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return S(
      child: U(
        child: N(
          child: O(
            child: F(
              child: B(
                child: E(
                  child: A(
                    child: C(
                      child: H(
                        child: F(
                          child: U(
                            child: C(
                              child: K(
                                child: Y(
                                  child: O(
                                    child: U(
                                      child: Text('ooooooooo'),
                                    ),
                                  ),
                                ),
                              ),
                            ),
                          ),
                        ),
                      ),
                    ),
                  ),
                ),
              ),
            ),
          ),
        ),
      ),
    );
  }
}
```

# Feature

1. 基本约束
    1. leftToLeft
    2. leftToRight
    3. rightToLeft
    4. rightToRight
    5. topToTop
    6. topToBottom
    7. bottomToTop
    8. bottomToBottom
    9. baselineToTop
    10. baselineToBottom
    11. baselineToBaseline
2. margin and goneMargin（当依赖的元素的可见性为 gone 或者其某一边的实际大小为 0 时，goneMargin 就会生效，否则 margin 会生效，即便其自身的可见性为
   gone）
3. clickPadding（
   快速扩大子元素的点击区域而无需改变子元素的实际大小。这意味着你可以完全遵照视觉稿来布局，而不用为了考虑点击区域而做额外的事情，这会提升一定的开发效率。这也意味着子元素之间可以在不增加嵌套的情况下共享点击区域，有时可能需要结合
   e-index 使用）
4. 可见性控制
    1. visible
    2. invisible
    3. gone（有时更好的做法是使用条件表达式来避免创建子元素，使用 gone 的好处是可以保留状态）
5. 完善的约束缺失、非法、冗余提示
6. 偏移（当同时设置了上下或左右约束时，可以使用 horizontalBias 和 verticalBias 来调整偏移。默认值是 0.5，代表居中）
7. z-index（绘制顺序，默认是子元素的顺序）
8. 平移
9. 百分比布局（当大小被设置为 matchConstraint 时，就会启用百分比布局，默认的百分比是 1（100%）。相关的属性是
   widthPercent，heightPercent，widthPercentageAnchor，heightPercentageAnchor）
10. 引导线
11. 约束和 Widget 分离
12. 栅栏
13. 比例布局
    1. widthHeightRatio: 1 / 3,
    2. ratioBaseOnWidth: true, （默认值是 null，代表自动推断，未确定边的大小会根据确定边的大小和 widthHeightRatio
       计算出来。未确定边的大小必须设置为 matchConstraint，确定边的大小可以为 matchParent，固定大小（>=0），matchConstraint）
14. 相对 id（这是为懒癌患者设计的，因为命名是个麻烦事。如果已经为子元素定义了 id，则不能再使用相对 id 来引用他们）
    1. rId(3) 代表第三个子元素，以此类推
    2. sId(-1) 代表上一个兄弟元素，以此类推
    3. sId(1) 代表下一个子元素，以此类推
15. 包装约束，是对基本约束的封装，便于使用，最终会转化成基本约束
    1. topLeftTo
    2. topCenterTo
    3. topRightTo
    4. centerLeftTo
    5. centerTo
    6. centerRightTo
    7. bottomLeftTo
    8. bottomCenterTo
    9. bottomRightTo
    10. centerHorizontalTo
    11. centerVerticalTo
    12. outTopLeftTo
    13. outTopCenterTo
    14. outTopRightTo
    15. outCenterLeftTo
    16. outCenterRightTo
    17. outBottomLeftTo
    18. outBottomCenterTo
    19. outBottomRightTo
    20. centerTopLeftTo
    21. centerTopCenterTo
    22. centerTopRightTo
    23. centerCenterLeftTo
    24. centerCenterRightTo
    25. centerBottomLeftTo
    26. centerBottomCenterTo
    27. centerBottomRightTo
16. 瀑布流、网格、列表（列表是一个特殊的瀑布流，网格也是一个特殊的瀑布流）
17. 圆形定位
18. e-index（事件分发顺序，默认是 z-index，一般用来处理点击区域）
19. 子元素的大小可以被设置为：
    1. 固定大小（>=0）
    2. matchParent
    3. wrapContent（默认值，支持最大、最小设置）
    4. matchConstraint
20. 自身的大小可以被设置为：
    1. 固定大小（>=0）
    2. matchParent（default）
    3. wrapContent（暂不支持最大、最小设置）

即将支持:

1. 链
2. 约束可视化
3. 更多...

支持的平台:

1. Android
2. iOS
3. Mac
4. Windows
5. Linux
6. Web

# 导入

支持空安全

```yaml
dependencies:
  flutter_constraintlayout:
    git:
      url: 'https://github.com/hackware1993/Flutter-ConstraintLayout.git'
      ref: 'v0.9.33-stable'
```

```yaml
dependencies:
  flutter_constraintlayout: ^0.9.33-stable
```

```dart
import 'package:flutter_constraintlayout/flutter_constraintlayout.dart';
```

# 示例 [Flutter Web Online Example](https://constraintlayout.flutterfirst.cn)

![effect.gif](https://github.com/hackware1993/flutter-constraintlayout/blob/master/effect.gif?raw=true)

```dart
class ExampleState extends State<Example> {
  double x = 0;
  double y = 0;

  ConstraintId box0 = ConstraintId('box0');
  ConstraintId box1 = ConstraintId('box1');
  ConstraintId box2 = ConstraintId('box2');
  ConstraintId box3 = ConstraintId('box3');
  ConstraintId box4 = ConstraintId('box4');
  ConstraintId box5 = ConstraintId('box5');
  ConstraintId box6 = ConstraintId('box6');
  ConstraintId box7 = ConstraintId('box7');
  ConstraintId box8 = ConstraintId('box8');
  ConstraintId box9 = ConstraintId('box9');
  ConstraintId box10 = ConstraintId('box10');
  ConstraintId box11 = ConstraintId('box11');

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        backgroundColor: Colors.black,
        body: ConstraintLayout(
          // Constraints can be separated from widgets
          childConstraints: [
            Constraint(
              id: box0,
              width: 200,
              height: 200,
              bottomLeftTo: parent,
              zIndex: 20,
            )
          ],
          children: [
            Container(
              color: Colors.redAccent,
              alignment: Alignment.center,
              child: const Text('box0'),
            ).applyConstraintId(
              id: box0, // Constraints can be separated from widgets
            ),
            Container(
              color: Colors.redAccent,
              alignment: Alignment.center,
              child: const Text('box1'),
            ).apply(
              constraint: Constraint(
                // Constraints set with widgets
                id: box1,
                width: 200,
                height: 100,
                topRightTo: parent,
              ),
            ),
            Container(
              color: Colors.blue,
              alignment: Alignment.center,
              child: const Text('box2'),
            ).applyConstraint(
              // Constraints set with widgets easy way
              id: box2,
              width: matchConstraint,
              height: matchConstraint,
              centerHorizontalTo: box3,
              top: box3.bottom,
              bottom: parent.bottom,
            ),
            Container(
              color: Colors.orange,
              width: 200,
              height: 150,
              alignment: Alignment.center,
              child: const Text('box3'),
            ).applyConstraint(
              id: box3,
              width: wrapContent,
              height: wrapContent,
              right: box1.left,
              top: box1.bottom,
            ),
            Container(
              color: Colors.redAccent,
              alignment: Alignment.center,
              child: const Text('box4'),
            ).applyConstraint(
              id: box4,
              width: 50,
              height: 50,
              bottomRightTo: parent,
            ),
            GestureDetector(
              child: Container(
                color: Colors.pink,
                alignment: Alignment.center,
                child: const Text('box5 draggable'),
              ),
              onPanUpdate: (details) {
                setState(() {
                  x += details.delta.dx;
                  y += details.delta.dy;
                });
              },
            ).applyConstraint(
              id: box5,
              width: 120,
              height: 100,
              centerTo: parent,
              zIndex: 100,
              translate: Offset(x, y),
              translateConstraint: true,
            ),
            Container(
              color: Colors.lightGreen,
              alignment: Alignment.center,
              child: const Text('box6'),
            ).applyConstraint(
              id: box6,
              width: 120,
              height: 120,
              centerVerticalTo: box2,
              verticalBias: 0.8,
              left: box3.right,
              right: parent.right,
            ),
            Container(
              color: Colors.lightGreen,
              alignment: Alignment.center,
              child: const Text('box7'),
            ).applyConstraint(
              id: box7,
              width: matchConstraint,
              height: matchConstraint,
              left: parent.left,
              right: box3.left,
              centerVerticalTo: parent,
              margin: const EdgeInsets.all(50),
            ),
            Container(
              color: Colors.cyan,
              alignment: Alignment.center,
              child: const Text('child[7] pinned to the top right'),
            ).applyConstraint(
              width: 200,
              height: 100,
              left: box5.right,
              bottom: box5.top,
            ),
            const Text(
              'box9 baseline to box7',
              style: TextStyle(
                color: Colors.white,
              ),
            ).applyConstraint(
              id: box9,
              width: wrapContent,
              height: wrapContent,
              baseline: box7.baseline,
              left: box7.left,
            ),
            ...horizontalChain(
              centerHorizontalTo: parent,
              hChainList: [
                Container(
                  color: Colors.redAccent,
                  alignment: Alignment.center,
                  child: const Text('chain item 1'),
                ).applyConstraint(
                  id: box10,
                  width: matchConstraint,
                  height: 200,
                  top: parent.top,
                ),
                Container(
                  color: Colors.redAccent,
                  alignment: Alignment.center,
                  child: const Text('chain item 2'),
                ).applyConstraint(
                  id: box11,
                  width: matchConstraint,
                  height: 200,
                  top: parent.top,
                ),
              ],
            ),
            Container(
              color: Colors.yellow,
              alignment: Alignment.bottomCenter,
              child: const Text(
                  'percentage layout\nwidth: 50% of parent\nheight: 30% of parent'),
            ).applyConstraint(
              width: matchConstraint,
              height: matchConstraint,
              widthPercent: 0.5,
              heightPercent: 0.3,
              horizontalBias: 0,
              verticalBias: 0,
              centerTo: parent,
            ),
          ],
        ),
      ),
    );
  }
}
```

# 高级用法

1. 引导线 [Flutter Web 在线示例](https://constraintlayout.flutterfirst.cn)

![guideline.webp](https://github.com/hackware1993/flutter-constraintlayout/blob/master/guideline.webp?raw=true)

```dart
class GuidelineExample extends StatelessWidget {
  const GuidelineExample({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    ConstraintId guideline = ConstraintId('guideline');
    return MaterialApp(
      home: Scaffold(
        body: ConstraintLayout(
          children: [
            Container(
              color: const Color(0xFF005BBB),
            ).applyConstraint(
              width: matchParent,
              height: matchConstraint,
              top: parent.top,
              bottom: guideline.top,
            ),
            Guideline(
              id: guideline,
              horizontal: true,
              guidelinePercent: 0.5,
            ),
            Container(
              color: const Color(0xFFFFD500),
            ).applyConstraint(
              width: matchParent,
              height: matchConstraint,
              top: guideline.bottom,
              bottom: parent.bottom,
            ),
            const Text(
              'Stand with the people of Ukraine',
              style: TextStyle(
                fontSize: 40,
                color: Colors.white,
              ),
            ).applyConstraint(
              width: wrapContent,
              height: wrapContent,
              centerHorizontalTo: parent,
              bottom: guideline.bottom,
            )
          ],
        ),
      ),
    );
  }
}
```

2. 栅栏 [Flutter Web 在线示例](https://constraintlayout.flutterfirst.cn)

![barrier.gif](https://github.com/hackware1993/flutter-constraintlayout/blob/master/barrier.gif?raw=true)

```dart
class BarrierExample extends StatelessWidget {
  const BarrierExample({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    ConstraintId leftChild = ConstraintId('leftChild');
    ConstraintId rightChild = ConstraintId('rightChild');
    ConstraintId barrier = ConstraintId('barrier');
    return MaterialApp(
      home: Scaffold(
        body: ConstraintLayout(
          debugShowGuideline: true,
          children: [
            Container(
              color: const Color(0xFF005BBB),
            ).applyConstraint(
              id: leftChild,
              width: 200,
              height: 200,
              top: parent.top,
              left: parent.left,
            ),
            Container(
              color: const Color(0xFFFFD500),
            ).applyConstraint(
              id: rightChild,
              width: 200,
              height: matchConstraint,
              right: parent.right,
              top: parent.top,
              bottom: parent.bottom,
              heightPercent: 0.5,
              verticalBias: 0,
            ),
            Barrier(
              id: barrier,
              direction: BarrierDirection.bottom,
              referencedIds: [leftChild, rightChild],
            ),
            const Text(
              'Align to barrier',
              style: TextStyle(
                fontSize: 40,
                color: Colors.blue,
              ),
            ).applyConstraint(
              width: wrapContent,
              height: wrapContent,
              centerHorizontalTo: parent,
              top: barrier.bottom,
              goneMargin: const EdgeInsets.only(top: 20),
            )
          ],
        ),
      ),
    );
  }
}
```

3. 角标 [Flutter Web 在线示例](https://constraintlayout.flutterfirst.cn)

![badge.webp](https://github.com/hackware1993/flutter-constraintlayout/blob/master/badge.webp?raw=true)

```dart
class BadgeExample extends StatelessWidget {
  const BadgeExample({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    ConstraintId anchor = ConstraintId('anchor');
    return Scaffold(
      body: ConstraintLayout(
        children: [
          Container(
            color: Colors.yellow,
          ).applyConstraint(
            width: 200,
            height: 200,
            centerTo: parent,
            id: anchor,
          ),
          Container(
            color: Colors.green,
            child: const Text(
              'Indeterminate badge size',
              style: TextStyle(
                color: Colors.black,
                fontSize: 20,
              ),
            ),
          ).applyConstraint(
            left: anchor.right,
            bottom: anchor.top,
            translate: const Offset(-0.5, 0.5),
            percentageTranslate: true,
          ),
          Container(
            color: Colors.green,
          ).applyConstraint(
            width: 100,
            height: 100,
            left: anchor.right,
            right: anchor.right,
            top: anchor.bottom,
            bottom: anchor.bottom,
          )
        ],
      ),
    );
  }
}
```

4. 网格 [Flutter Web 在线示例](https://constraintlayout.flutterfirst.cn)

![grid.webp](https://github.com/hackware1993/flutter-constraintlayout/blob/master/grid.webp?raw=true)

```dart
class GridExample extends StatelessWidget {
  const GridExample({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    List<Color> colors = [
      Colors.redAccent,
      Colors.greenAccent,
      Colors.blueAccent,
      Colors.orangeAccent,
      Colors.yellow,
      Colors.pink,
      Colors.lightBlueAccent
    ];
    return Scaffold(
      body: ConstraintLayout(
        children: [
          ...constraintGrid(
              id: ConstraintId('grid'),
              left: parent.left,
              top: parent.top,
              itemCount: 50,
              columnCount: 8,
              itemWidth: 50,
              itemHeight: 50,
              itemBuilder: (index) {
                return Container(
                  color: colors[index % colors.length],
                );
              },
              itemMarginBuilder: (index) {
                return const EdgeInsets.only(
                  left: 10,
                  top: 10,
                );
              })
        ],
      ),
    );
  }
}
```

5. 瀑布流 [Flutter Web 在线示例](https://constraintlayout.flutterfirst.cn)

![staggered_grid.gif](https://github.com/hackware1993/flutter-constraintlayout/blob/master/staggered_grid.gif?raw=true)

```dart
class StaggeredGridExample extends StatelessWidget {
  const StaggeredGridExample({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    List<Color> colors = [
      Colors.redAccent,
      Colors.greenAccent,
      Colors.blueAccent,
      Colors.orangeAccent,
      Colors.yellow,
      Colors.pink,
      Colors.lightBlueAccent
    ];
    const double smallestSize = 40;
    const int columnCount = 8;
    Random random = Random();
    return Scaffold(
      body: ConstraintLayout(
        children: [
          TextButton(
            onPressed: () {
              (context as Element).markNeedsBuild();
            },
            child: const Text(
              'Upset',
              style: TextStyle(
                fontSize: 32,
                height: 1.5,
              ),
            ),
          ).applyConstraint(
            left: ConstraintId('horizontalList').right,
            top: ConstraintId('horizontalList').top,
          ),
          ...constraintGrid(
              id: ConstraintId('horizontalList'),
              left: parent.left,
              top: parent.top,
              margin: const EdgeInsets.only(
                left: 100,
              ),
              itemCount: 50,
              columnCount: columnCount,
              itemBuilder: (index) {
                return Container(
                  color: colors[index % colors.length],
                  alignment: Alignment.center,
                  child: Text('$index'),
                );
              },
              itemSizeBuilder: (index) {
                if (index == 0) {
                  return const Size(
                      smallestSize * columnCount + 35, smallestSize);
                }
                if (index == 6) {
                  return const Size(smallestSize * 2 + 5, smallestSize);
                }
                if (index == 7) {
                  return const Size(smallestSize * 6 + 25, smallestSize);
                }
                if (index == 19) {
                  return const Size(smallestSize * 2 + 5, smallestSize);
                }
                if (index == 29) {
                  return const Size(smallestSize * 3 + 10, smallestSize);
                }
                return Size(
                    smallestSize, (2 + random.nextInt(4)) * smallestSize);
              },
              itemSpanBuilder: (index) {
                if (index == 0) {
                  return columnCount;
                }
                if (index == 6) {
                  return 2;
                }
                if (index == 7) {
                  return 6;
                }
                if (index == 19) {
                  return 2;
                }
                if (index == 29) {
                  return 3;
                }
                return 1;
              },
              itemMarginBuilder: (index) {
                return const EdgeInsets.only(
                  left: 5,
                  top: 5,
                );
              })
        ],
      ),
    );
  }
}
```

6. 圆形定位 [Flutter Web 在线示例](https://constraintlayout.flutterfirst.cn)

![circle_position.gif](https://github.com/hackware1993/flutter-constraintlayout/blob/master/circle_position.gif?raw=true)

```dart
class CirclePositionExampleState extends State<CirclePositionExample> {
  late Timer timer;
  late int hour;
  late int minute;
  late int second;

  double centerTranslateX = 0;
  double centerTranslateY = 0;

  @override
  void initState() {
    super.initState();
    calculateClockAngle();
    timer = Timer.periodic(const Duration(seconds: 1), (_) {
      calculateClockAngle();
    });
  }

  void calculateClockAngle() {
    setState(() {
      DateTime now = DateTime.now();
      hour = now.hour;
      minute = now.minute;
      second = now.second;
    });
  }

  @override
  void dispose() {
    super.dispose();
    timer.cancel();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: ConstraintLayout(
        children: [
          GestureDetector(
            child: Container(
              decoration: const BoxDecoration(
                color: Colors.red,
                borderRadius: BorderRadius.all(
                  Radius.circular(1000),
                ),
              ),
            ),
            onPanUpdate: (details) {
              setState(() {
                centerTranslateX += details.delta.dx;
                centerTranslateY += details.delta.dy;
              });
            },
          ).applyConstraint(
            width: 20,
            height: 20,
            centerTo: parent,
            zIndex: 100,
            translate: Offset(centerTranslateX, centerTranslateY),
            translateConstraint: true,
          ),
          for (int i = 0; i < 12; i++)
            Text(
              '${i + 1}',
              style: const TextStyle(
                fontWeight: FontWeight.bold,
                fontSize: 25,
              ),
            ).applyConstraint(
              centerTo: rId(0),
              translate: circleTranslate(
                radius: 205,
                angle: (i + 1) * 30,
              ),
            ),
          for (int i = 0; i < 60; i++)
            if (i % 5 != 0)
              Transform.rotate(
                angle: pi + pi * (i * 6 / 180),
                child: Container(
                  color: Colors.grey,
                  margin: const EdgeInsets.only(
                    top: 405,
                  ),
                ),
              ).applyConstraint(
                width: 1,
                height: 415,
                centerTo: rId(0),
              ),
          Transform.rotate(
            angle: pi + pi * (hour * 30 / 180),
            alignment: Alignment.topCenter,
            child: Container(
              color: Colors.green,
            ),
          ).applyConstraint(
            width: 5,
            height: 80,
            centerTo: rId(0),
            translate: const Offset(0, 0.5),
            percentageTranslate: true,
          ),
          Transform.rotate(
            angle: pi + pi * (minute * 6 / 180),
            alignment: Alignment.topCenter,
            child: Container(
              color: Colors.pink,
            ),
          ).applyConstraint(
            width: 5,
            height: 120,
            centerTo: rId(0),
            translate: const Offset(0, 0.5),
            percentageTranslate: true,
          ),
          Transform.rotate(
            angle: pi + pi * (second * 6 / 180),
            alignment: Alignment.topCenter,
            child: Container(
              color: Colors.blue,
            ),
          ).applyConstraint(
            width: 5,
            height: 180,
            centerTo: rId(0),
            translate: const Offset(0, 0.5),
            percentageTranslate: true,
          ),
          Text(
            '$hour:$minute:$second',
            style: const TextStyle(
              fontSize: 40,
            ),
          ).applyConstraint(
            outTopCenterTo: rId(0),
            margin: const EdgeInsets.only(
              bottom: 250,
            ),
          )
        ],
      ),
    );
  }
}
```

7. margin [Flutter Web 在线示例](https://constraintlayout.flutterfirst.cn)

![margin.webp](https://github.com/hackware1993/flutter-constraintlayout/blob/master/margin.webp?raw=true)

```dart
class MarginExample extends StatelessWidget {
  const MarginExample({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: ConstraintLayout(
        children: [
          Container(
            color: const Color(0xFF005BBB),
          ).applyConstraint(
            size: 50,
            topLeftTo: parent,
            margin: const EdgeInsets.only(
              left: 20,
              top: 100,
            ),
          ),
          Container(
            color: const Color(0xFFFFD500),
          ).applyConstraint(
            size: 100,
            top: sId(-1).bottom,
            right: parent.right.margin(100),
          ),
          Container(
            color: Colors.pink,
          ).applyConstraint(
            size: 50,
            topRightTo: parent.rightMargin(20).topMargin(50),
          ),
        ],
      ),
    );
  }
}
```

# 性能优化

1. 当布局复杂时，如果子元素需要频繁重绘，可以考虑使用 RepaintBoundary。当然合成 Layer 也有开销，所以需要合理使用。

```dart
class OffPaintExample extends StatelessWidget {
  const OffPaintExample({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        body: ConstraintLayout(
          children: [
            Container(
              color: Colors.orangeAccent,
            ).offPaint().applyConstraint(
              width: 200,
              height: 200,
              topRightTo: parent,
            )
          ],
        ),
      ),
    );
  }
}
```

2. 尽量使用 const Widget。如果你没法将子元素声明为 const 而它自身又不会改变。可以使用内置的 OffBuildWidget 来避免子元素重复 build。

```dart
class OffBuildExample extends StatelessWidget {
  const OffBuildExample({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        body: ConstraintLayout(
          children: [

            /// subtrees that do not change
            Container(
              color: Colors.orangeAccent,
            ).offBuild(id: 'id').applyConstraint(
              width: 200,
              height: 200,
              topRightTo: parent,
            )
          ],
        ),
      ),
    );
  }
}
```

3. 子元素会自动成为 RelayoutBoundary 除非它的宽或高是 wrapContent。可以酌情的减少 wrapContent 的使用，因为当 ConstraintLayout
   自身的大小发生变化时（通常是窗口大小发生变化，移动端几乎不存在此类情况），所有宽或高为 wrapContent
   的子元素都会被重新布局。而其他元素由于传递给它们的约束未发生变化，不会触发真正的布局。

4. 如果你在 children 列表中使用 Guideline 或 Barrier， Element 和 RenderObject 将不可避免的被创建，它们会被布局但不会绘制。此时你可以使用
   GuidelineDefine 或 BarrierDefine 来优化， Element 和 RenderObject 就不会再创建了。

```dart
class BarrierExample extends StatelessWidget {
  const BarrierExample({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    ConstraintId leftChild = ConstraintId('leftChild');
    ConstraintId rightChild = ConstraintId('rightChild');
    ConstraintId barrier = ConstraintId('barrier');
    return Scaffold(
      body: ConstraintLayout(
        childConstraints: [
          BarrierDefine(
            id: barrier,
            direction: BarrierDirection.bottom,
            referencedIds: [leftChild, rightChild],
          ),
        ],
        children: [
          Container(
            color: const Color(0xFF005BBB),
          ).applyConstraint(
            id: leftChild,
            width: 200,
            height: 200,
            topLeftTo: parent,
          ),
          Container(
            color: const Color(0xFFFFD500),
          ).applyConstraint(
            id: rightChild,
            width: 200,
            height: matchConstraint,
            centerRightTo: parent,
            heightPercent: 0.5,
            verticalBias: 0,
          ),
          const Text(
            'Align to barrier',
            style: TextStyle(
              fontSize: 40,
              color: Colors.blue,
            ),
          ).applyConstraint(
            centerHorizontalTo: parent,
            top: barrier.bottom,
          )
        ],
      ),
    );
  }
}   
```

# 支持我

如果它对你帮助很大，可以考虑赞助我一杯奶茶，毕竟开源不易。或者反手给个 star。
[Paypal](https://www.paypal.com/paypalme/hackware1993)
![support.webp](https://github.com/hackware1993/flutter-constraintlayout/blob/master/support.webp?raw=true)

# 联系方式

hackware1993@gmail.com

# 协议

```
MIT License

Copyright (c) 2022 hackware1993

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and
associated documentation files (the "Software"), to deal in the Software without restriction,
including without limitation the rights to use, copy, modify, merge, publish, distribute,
sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial
portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT
NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND
NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES
OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN
CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
```