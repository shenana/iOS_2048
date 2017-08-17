###一、添加”Start“开始游戏按钮：
在ViewController.swift的ViewController类中，添加如下button：


```
var button=UIButton(frame:CGRect(x:100,y:100,width:100,heignt:100))
```


在该类的viewDidLoad方法中，设置按钮：


```
self.button.setTitle("Start",forState:UIControlState.normal)  //forState设置为普通状态下
self.button.setTitleColor(UIColor.blueColor(),forState:UIControlState.normal)
view.addSubview(self.button)
```


注：此时还未为按钮添加点击事件。
###二、创建主面板
新建文件NumbertailGameController.swift，这个文件主要处理游戏的初始化等逻辑。在该文件中的NumbertailGameController类继承UIViewController。
在该类中添加如下属性：
 

```
    var demension : Int  //2048游戏中每行每列含有多少个块
    var threshold : Int  //最高分数，判断输赢时使用
    let boardWidth: CGFloat = 260.0  //游戏区域的长度和高度
    let thinPadding: CGFloat = 3.0  //游戏区里面小块间的间距
    let viewPadding: CGFloat = 10.0  //计分板和游戏区块的间距
    let verticalViewOffset: CGFloat = 0.0  //一个初始化属性，后面会有地方用到
```
为该类添加init方法：


```
init(demension d:Int,threshold t:Int){
    demension=d<2 ? 2:d
    threshold=t<8 ? 8:t
    super.init(nibName:nil,bundle:nil)
    view.backgroundColor=UIColor(red:0xE6/255,green:0xE4/255,blue:0xD4/255,alpha:1)
}
```


注：此时Xcode会给出错误提示：required initializer 巴拉巴拉，单击错误提示的红色圈后双击”Fix-it:insert "巴拉巴拉" “即可自动生成代码：


```
required init?(coder aDecoder: NSCoder) {
        fatalError("init(coder:) has not been implemented")
    }
```
###三、为”Start"按钮添加响应事件
在ViewController类中添加setupGame方法，作为该按钮的响应事件：


```
func setupGame(sender:UIButton){
    let game=NumbertailGameController(demension:4,threshold:2048)//game作为NumbertailGameController类的实体
    self.presentViewController(game,animated:true,complition:nil)//presentViewControllerz作为页面跳转指令
}
```


在viewDidLoad()方法中为按钮设置响应事件：


```
self.button.addTarget(self,action:"setupGame:",forControlEvents:.touchUpInside)//touchUpInside：在控件之内触摸并抬起事件
```


注：该语句应该在view.addSubview(self.button)之前。

###四、创建游戏面板的视图文件：
新建GamebordView.swift文件，GamebordView类继承UIView，添加如下属性：
    
    
    ```
    var demension : Int //每行(列)区块个数
    var tileWidth : CGFloat  //每个小块的宽度
    var tilePadding : CGFloat  //每个小块间的间距
    ```
为该类添加init方法：
init(demension d:Int,tileWidth tw:CGFloat,tilePadding tp:CGFloat,backgroundColor:UIColor,foregroundColor:UIColor){
    demension=d
    tileWidth=tw
    tilePadding=tp
    let totalWidth=CGFolat(demension)*(tileWidth+tilePadding)+tileWidth//游戏区块的边长
    super.init(frame:CGRect(x:0,y:0,width:totalWidth,height:totalWidth))
    self.backgroundColor=backgroundColor
}
同上t添加required init?(coder)巴拉巴拉


###五、初始化游戏面板对象并添加到主面板中    
在游戏的主控制器NumbertailGameController中添加 setupGame()方法，并在viewDidLoad()方法中调用该setupGame（），其中setupGame（）的代码如下：
 

```
   func setupGame(){
        let mainViewWidth=view.bounds.size.width //主面板宽度
        let mainViewHeight=view.bounds.size.height//主面板长度
        //获取游戏面板左上角点的x坐标，即主面板的宽减去游戏面板的宽再除以2
        func xPositionOfGameViewStartPoint(view v:UIView)->CGFloat{
            let gameViewWidth=v.bounds.size.width
            return 0.5*(mainViewWidth-gameViewWidth)
        }
        //获取游戏面板左上角点的y坐标，即主面板的高减去所有视图面板的总高度除以2然后在加上在游戏面板之前的视图的总高度。
        func yposition2Center(order : Int , views : [UIView]) -> CGFloat {
            assert(views.count > 0)
            let totalViewHeigth = CGFloat(views.count - 1)*viewPadding +
                views.map({$0.bounds.size.height}).reduce(verticalViewOffset, combine: {$0 + $1})
            let firstY = 0.5*(mainViewHeight - totalViewHeigth)
            
            var acc : CGFloat = 0
            for i in 0..<order{
                acc += viewPadding + views[i].bounds.size.height
            }
            return acc + firstY
        }
        //游戏面板每个区块的宽度
        let block=(boardWidth-thinPadding*CGFloat(demension+1))/CGFloat(demension)
        //初始化游戏面板对象，即gameBoard
        let gamebord=GamebordView(demension: demension, tileWidth: block, tilePadding: thinPadding, bkgColor: UIColor(red: 0x90/255, green: 0x8D/255, blue: 0x80/255, alpha: 1), forgColor: UIColor(red: 0xF9/255, green: 0xF9/255, blue: 0xF9/255, alpha: 0.5))
        //现在面板中所有的视图对象，目前只有游戏面板，后续加入计分板
        let views=[gamebord]
        //设置游戏面板在整个面板中的的绝对位置，即左上角第一个点
        var f=gamebord.frame
        f.origin.x=xPositionOfGameViewStartPoint(view: gamebord)
        f.origin.y=yposition2Center(0, views: views)
        gamebord.frame=f
        //将游戏对象加入当前面板中
        view.addSubview(gamebord)
    }


```   
在viewDidLoad()方法中调用该setupGame（），即在 viewDidLoad()方法调用super.viewDidLoad()后再去调用setupGame（）方法。
###六、初始化的半透明小方块
在GamebordView.swift中添加如下方法:
 

```
 func setColor(backgroundColor bc:UIColor,foregroundColor fc:UIColor){
        self.backgroundColor=bc
        var xx=tilePadding
        var yy:CGFloat
        //最内层循环体生成一个区块，所以两层嵌套的for循环生成demension*demension个区块
        for _ in 0..<demension{
            yy=tilePadding
            for _ in 0..<demension{
                let tileFrame=UIView(frame: CGRect(x: xx, y: yy, width: tileWidth, height: tileWidth))
                tileFrame.backgroundColor=fc
                tileFrame.layer.cornerRadius=8//设置区块为圆角正方形
                addSubview(tileFrame)
                yy+=tilePadding+tileWidth
            
            }
            xx+=tilePadding+tileWidth
        }
    }
```


在NumbertailGameController.swift中对gamebord调用该方法，即在setupGame()方法调用view.addSubview(gamebord)之前使用如下语句：


```
gamebord.setColor(backgroundColor: UIColor(red : 0x90/255, green : 0x8D/255, blue : 0x80/255, alpha : 1), foregroundColor: UIColor(red : 0xF9/255, green : 0xF9/255, blue : 0xE3/255, alpha : 0.5))
```


此时已完成基本的面板：在主面板中有一个游戏面板，该游戏面板包含4*4个半透明圆角区块。

















    
    

