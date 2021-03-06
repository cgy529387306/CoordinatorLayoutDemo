# CoordinatorLayout的演示Demo

项目中需要相关效果,然后记得`Design`库提供了相关组件.

在网上找到了很多样例和讲解.但是网上找的都只是一部分.各有偏重点.但没有一个从基础原理上完完整整的讲解

只能自己对照源码去一步步的分析.以期完全掌握其动作原理,流程等.

于是有了这个Demo.从浅到深用一个个的案例去演示不同的用法. 做了几天,发现坑被自己越挖越大,只能慢慢填了.

一开始一头雾水,看到别人写的案例,分析.看过后总有很多**为什么会这样**,**具体是怎么做到的**之类的纠结.在自己一步步写完Demo后问题都解决了.但回头写文档时发现的新问题就是已经忘记了当初是纠结了哪些问题....


??  需要目标组件??需要配对??


### 新手上路
#### 一 `CoordinatorLayout`干什么用的
用来通过`Behavior`谐调两个子组件之间的互动改变.比如滑动,缩放.等等.目前官方推出的配套的其它组件主要集中在滑动互动上.

其核心功能就是拦截事件,寻找对应的`Behavior`,让`Behavior`处理事件
#### 二 怎么做到的
一个重要的新角色`Behavior`出场,由Behavior代理子组件的相关事件,比如`onMeasure,onLayout,onTouchEvent`等

在触摸事件中`CoordinatorLayout`拦截触摸事件,根据被触摸的子组件寻找处理该事件的`Behavior`,调用其相关方法处理此事件.

#### 三 怎么配对的
`Behavior`的运行是基与代理模式.其代理的组件称为`ChildView`

`Behavior`是承继自`CoordinatorLayout.Behaviro<T extends View>`.其中的泛型用来确定`ChildView`的类型.

在触摸事件中,`CoordinatorLayout`会遍历直接子组件.寻找带有能处理触摸事件的`Behavior`,带有`Behavior`的子组件就是其`ChildView`

另外`Behavior`还是一种基与观察者模式的控制器.其中被观察的`View`叫`DependencyView`,配对的`View`叫`ChildView`.

`Behavior`中`layoutDependsOn`方法用来判断一个`View`是否为`DependencyView`

#### 四 Behavior是怎么设置的
`Behavior`是存放在`CoordinatorLayout.LayoutParams`里的.`CoordinatorLayout`的直接子组件的`LayoutParams`都是.

`Behavior`有三种设置方式
* 在`xml`中使用`app:scrolling_flags`来设置
* 在组件类声明上使用注解`@Coordinator.DefaultBehavior(T.class)`来设置
* 在代码中创建实例,设置到`Coodinator.LayoutParams`中

#### 五 Behavior是每个相关联的View都附带吗?

#### 六 有哪些配套的`View`和`Behavior`
* `AppBarLayout`及其`AppBarLayout.Behavior`,其`ChildView`为`AppBarLayout`,`Dependency`为`View`
* `CollapsingToolbarLayout`,配合`AppBarLayout`使用,生成折叠及视差模式
* `FloatingActionButton`及其`FloatingActionButton.Behavior`, 其`ChildView`为`FloatingActionButton`,`Dependency`为`View`
* `BottomSheetBehavior`,`Dependency`为`View`,因为泛型为`View`所以需要在代码中使用.定义好`ChildView`的类型.
默认提供`BottomSheetDialog`,`BottomSheetFragment`的实现.
* `SwipeDismissBehavior`, `Dependency`为`View`,因为泛型为`View`,所以需要在代码中使用.定义好`ChildView`的类型.
在`SnakeBar`中有个其子类`SnakeBar.Behavior<SnakeBar>`

通常使用得最多的是`AppbarLayout`和`CollapsingToolbarLayout`

#### 七 具体的使用方法
[看Demo及对应的说明](doc/Demo案例说明.md)


#### 相关知识点分析
* [`NestedScrolling嵌套滑动机件分析`](doc/NestedScrolling嵌套滑动机件分析.md)
* [`CoordinatorLayout相关分析`](doc/CoordinatorLayout分析.md)
* [`Behavior分析`](doc/Behavior分析.md)
* [`官方配套的View分析`](doc/官方配套的View分析.md)







