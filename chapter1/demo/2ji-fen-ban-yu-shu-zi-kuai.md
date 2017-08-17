###一、添加属性
创建ScoreView.swift文件，ScoreView类继承UIView。为该类添加属性：


```
    var lable:UILabel//计分板本身是个lable，作用是显示分数
    var score:Int=0{
        didSet{
            lable.text="Score:\(score)"
        }
    }
    let defaultFrame=CGRectMake(0, 0, 140, 40)//用作积分器的矩形

```
###二、创建init方法
 在ScoreView类中添加init方法如下： 
 

```
init(backgroundColor bc:UIColor,textColor tc:UIColor,font :UIFont){
        lable=UILabel(frame: defaultFrame)//生成lable
        lable.textAlignment=NSTextAlignment.Center//设置文字的对齐方式
        super.init(frame: defaultFrame)
        backgroundColor=bc
        lable.textColor=tc
        lable.font=font//设置文字的字体
        lable.layer.cornerRadius=6//设置圆角正方形
        self.addSubview(lable)    
    }
```
点击Xcode提示的错误可自动生成：


```
 required init?(coder aDecoder: NSCoder) {
        fatalError("init(coder:) has not been implemented")
    }
```
###三、添加协议

在该文件中（即声明在ScoreView类外）添加协议从而方便别的类中调用计分板的scoreChanged方法：


```
protocol scoreProtocol{
    func scoreChanged(newScore ns:Int)
}
```

此时应该让ScoreView类继承该协议，即将ScoreView类写成：


```
class ScoreView: UIView ,scoreProtocol{
    ......
}
```
这个时候Xcode会提醒出错，因为还未实现scoreProtocol中的scoreChanged(newScore ns:Int)方法，因此在类ScoreView中实现scoreChanged方法如下：


```
 func scoreChanged(newScore ns: Int) {
        score=ns
    }

```
###四、在主面板添加计分板
在主控制器NumbertailGameController中初始化一个计分板，即在NumbertailGameController类中的setupGame()方法尾部添加如下代码：


```
    //初始化一个ScoreView
    let scoreView=ScoreView(backgroundColor: UIColor(red: 0xE0/255, green: 0xE9/255, blue: 0xD9/255, alpha: 1), textColor: UIColor(red: 0x80/255, green: 0x80/255, blue: 0x80/255, alpha: 0.5), font: UIFont(name: "HelveticaNeue-Bold", size: 16.0) ?? UIFont.systemFontOfSize(16.0))
    //当前主面板上包含游戏面板与计分板
    let views2=[scoreView,gamebord]
    //定位计分板在主面板中左上角的绝对位置
    var f2=scoreView.frame
    f2.origin.x=xPositionOfGameViewStartPoint(view: scoreView)
    f2.origin.y=yposition2Center(0, views: views2)-30//如果不-30的话 游戏面板会与计分板重叠
    scoreView.frame=f2
    //调用其自身方法来初始化一个分数
    scoreView.scoreChanged(newScore: 1531300)
    //将计分板添加进当前面板
    view.addSubview(scoreView)
```















