---
title: iOS15 适配总结
date: 2021-11-29 15:29:38
categories: 技术
tags: iOS
---

手机升级iOS 15系统以后，通过Xcode 13编译以后，界面显示出现视图问题


## UINavigationBar和UITabBar

问题: 设置颜色方法在iOS 15中失效了

在iOS 13更新的API中新增了针对navigationBar,tabbar分别新增了新的属性专门管理这些滑动时候产生的颜色透明等等信息，由于我们app根据业务要求还需要向下兼容iOS 9和iOS 10等等，对于导航栏的设置还没有使用UINavigationBarAppearance和UITabBarAppearance来设置，导致更新iOS 15以后原有设置方式就失效了。

使用

```
/// UITabbar
if #available(iOS 15, *) {
    let bar = UITabBarAppearance.init()
    bar.backgroundColor = UIColor.white
    bar.shadowImage = UIColor.init(0xEEEEEE).image
    let selTitleAttr = [
        NSAttributedString.Key.font: itemFont,
        NSAttributedString.Key.foregroundColor: UIColor.theme
    ]
    bar.stackedLayoutAppearance.selected.titleTextAttributes = selTitleAttr // 设置选中attributes
    self.tabBar.scrollEdgeAppearance = bar
    self.tabBar.standardAppearance = bar
}
```

```
/// UINavigationBar
if #available(iOS 13.0, *) {
  let appearance = UINavigationBarAppearance()
  appearance.configureWithOpaqueBackground()
  appearance.backgroundImage = UIImage()
  appearance.shadowImage = UIImage()
  appearance.shadowColor = UIColor.clear
  nav.navigationBar.standardAppearance = appearance
  nav.navigationBar.scrollEdgeAppearance = nav.navigationBar.standardAppearance
}
```


## UITableView

在iOS 15里，UITableView新增了sectionHeaderTopPadding的属性，即在section header的上方的填充，默认值是UITableViewAutomaticDimension，高度为22个像素。我们在iOS 15上需要将这个属性的值设置为0，否则我们所有的tableView列表上将会多出22个像素的高度。

使用

```
/// 可以在创建UITableView的实例时，做如下设置:

if #available(iOS 15, *) {
    tableView.sectionHeaderTopPadding = 0
}
```

或者

```
/// 全局设置
if #available(iOS 15, *) {
   UITableView.appearance().sectionHeaderTopPadding = 0
}
```

## 总结
总的来说，iOS 15的工作量较小，适配相对简单易行~~

