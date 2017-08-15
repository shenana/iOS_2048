* 新建：

    打开Xcode，创建新项目。选用 iOS->Source->Single View Application 模板,点击确定。
    在面板上输入项目名称，将Language选为Swift，将Devices选为iPhone，点击确定。
* 设置横竖屏均支持：

    新建的App是一个只支持竖屏显示的App，所以在继续之前，单击项目名称后可在Xcode主面板看到“General”“capabilities"等多个选项。选择"General"下方的Deployment Info - Device Orientation下面把 Landscape Left和Landscape Right 选项勾掉。
* 禁掉Auto Layout与Size Classes

    Xcode 6默认为Storyboard和nib文件开启自动布局（Auto Layout）和尺寸归类（Size Classes）。自动布局和尺寸归类这两项新技术可以构建易于调整大小的用户界面，这对支持不同尺寸的iPhone和iPad非常有用。在继续探索之前，单点main.storyboard后在右侧查找一个像文件一样的图标（即当前Storyboard的 File inspector（文件检查器）），在下方的interface builder documents中禁用Auto Layout和Size Classes。
    
* App是如何加载初始视图控制器：

  在项目的AppDelegate.swift里有如下代码：  
  
  ```
    import UIKit  
    @UIApplicationMain
    class AppDelegate: UIResponder, UIApplicationDelegate {
      var window: UIWindow?
      func application(application: UIApplication!, didFinishLaunchingWithOptions launchOptions: NSDictionary!) -> Bool {
    // Override point for customization after application launch.
    return true
  }
  ```
  上面的 @UIApplicationMain 标记指定这个AppDelegate类为该模块的入口。使用Storyboard时，应用代理必须继承 UIResponder ，必须含有 UIWindow 属性。
  
* Tab Bar Controller（分页栏控制器）
Attributes Inspector（属性检查器）
Table View Controller（表视图控制器）
导航控制器（Navigation Controller）
 Simulated Metrics（模拟度量)
 分页栏项（Tab Bar Item）
 Navigation Item（导航项）
 原型表项（Prototype Cell）
 身份检查器（Identity inspector）
  
    
* 设置初始视图控制器：
选中分页栏控制器或者视图控制器，然后在 Attributes Inspector（属性检查器） 中选定 Is Initial View Controller 。（属性检查器在右侧第四个图标）

* 把表视图控制器放到导航控制器（Navigation Controller）中：
选中表视图控制器，在Xcode菜单中选择 Editor->Embed In->Navigation Controller ，现在面板中又加入了另一个控制器.

* 将一个视图控制器连接到分页栏控制器后，在场景下面和文档大纲中会看到它被赋予的 分页栏项（Tab Bar Item） 对象(图标是白色正方形内的蓝色星星✨)，可以用来设置分页标签的标题和在分页栏控制器中看到的图标。
  
* 嵌入导航控制器的一个视图控制器包含用于设置导航栏的 Navigation Item（导航项），在文档大纲中表视图控制器的导航项的图标是白色正方形内有蓝色的<符号。