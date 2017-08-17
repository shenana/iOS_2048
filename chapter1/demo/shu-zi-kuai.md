###一、定义颜色显示协议
新建AppearanceProvider.swift文件，该文件定义了一些颜色，可以根据当前的数字值来取不同的颜色。在该文件输入如下代码即可：


```
import UIKit
protocol AppearanceProviderProtocol: class {
    func tileColor(value: Int) -> UIColor
    func numberColor(value: Int) -> UIColor
    func fontForNumbers() -> UIFont
}

class AppearanceProvider: AppearanceProviderProtocol {
    
    // Provide a tile color for a given value
    func tileColor(value: Int) -> UIColor {
        switch value {
        case 2:
            return UIColor(red: 238.0/255.0, green: 228.0/255.0, blue: 218.0/255.0, alpha: 1.0)
        case 4:
            return UIColor(red: 237.0/255.0, green: 224.0/255.0, blue: 200.0/255.0, alpha: 1.0)
        case 8:
            return UIColor(red: 242.0/255.0, green: 177.0/255.0, blue: 121.0/255.0, alpha: 1.0)
        case 16:
            return UIColor(red: 245.0/255.0, green: 149.0/255.0, blue: 99.0/255.0, alpha: 1.0)
        case 32:
            return UIColor(red: 246.0/255.0, green: 124.0/255.0, blue: 95.0/255.0, alpha: 1.0)
        case 64:
            return UIColor(red: 246.0/255.0, green: 94.0/255.0, blue: 59.0/255.0, alpha: 1.0)
        case 128, 256, 512, 1024, 2048:
            return UIColor(red: 237.0/255.0, green: 207.0/255.0, blue: 114.0/255.0, alpha: 1.0)
        default:
            return UIColor.greenColor()
        }
    }
    
    // Provide a numeral color for a given value
    func numberColor(value: Int) -> UIColor {
        switch value {
        case 2, 4:
            return UIColor(red: 119.0/255.0, green: 110.0/255.0, blue: 101.0/255.0, alpha: 1.0)
        default:
            return UIColor.whiteColor()
        }
    }
    
    // Provide the font to be used on the number tiles
    func fontForNumbers() -> UIFont {
        if let font = UIFont(name: "HelveticaNeue-Bold", size: 20) {
            return font
        }
        return UIFont.systemFontOfSize(20)
    }
}
```
###二、新建数字块
新建TileView.swift文件，TileView类继承UIView，为该类添加如下属性：


```
    //一个数字块就是一个lable
    var lable:UILabel
    //提供颜色选择
    unowned let delegate:AppearanceProviderProtocol
    //数字块中的值
    var value:Int=0{
        didSet{
            backgroundColor=delegate.tileColor(value)
            lable.textColor=delegate.numberColor(value)
            lable.text="\(value)"
        }
    }
```
为该类新建init()方法：


```
init(position:CGPoint,width:CGFloat,value:Int,delegate d:AppearanceProviderProtocol){
        delegate=d
        self.value=value
        lable=UILabel(frame: CGRectMake(0,0,width,width))
        lable.textAlignment=NSTextAlignment.Center
        lable.minimumScaleFactor=0.5//使得UIlabel根据字数多少来减小字体大小，使得UIlabel能够显示全所有的文字。
        lable.font = UIFont(name: "HelveticaNeue-Bold", size: 15) ?? UIFont.systemFontOfSize(15)
        super.init(frame: CGRectMake(position.x, position.y, width, width))
        addSubview(lable)
        lable.layer.cornerRadius=6
        backgroundColor=delegate.tileColor(value)//根据数值来选择区块的背景颜色
        lable.textColor=delegate.numberColor(value)//根据数值来选择字体的颜色
        lable.text="\(value)"//显示数值
    }
```
###三、将数字块添加到游戏面板中
首先，在GamebordView.swift中新增如下方法，以确保某个点位置属于游戏面板内的：


```
func positionIsValied(position : (Int , Int)) -> Bool{
    let (x , y) = position
    return x >= 0 && x < dimension && y >= 0 && y < dimension
}
```
为方便根据数值使得数字块、数值显示各种不同的颜色，先在GamebordView类中添加实例：
 `let provider = AppearanceProvider()`
取出数字块相对于游戏区块的坐标，先在GamebordView类中添加实例tiles从而使得tiles能保存数字块的坐标：
  `var tiles : Dictionary<NSIndexPath , TileView>`
并在init方法中对其初始化为空字典：

```
tiles = Dictionary()
```
接着，在GamebordView.swift中新增如下方法，在此方法中，将取出数字块相对于游戏区块的坐标放入tiles中，然后初始化出数字块，添加到游戏面板中，且将其置于上层：

```
func insertTile(position:(Int,Int),value:Int){
        assert(positionIsValied(position))
        let(row,col)=position
        let x=tilePadding+CGFloat(row)*(tilePadding+tileWidth)
        let y=tilePadding+CGFloat(col)*(tilePadding+tileWidth)
        let tileView=TileView(position: CGPointMake(x, y), width: tileWidth, value: value, delegate: provider)
        addSubview(tileView)
        bringSubviewToFront(tileView)
        tiles[NSIndexPath(forRow : row , inSection:  col)] = tileView
        //UITableView声明了一个NSIndexPath的类别，主要用 来标识当前cell的在tableView中的位置，该类别有section和row两个属性，前者标识当前cell处于第几个section中，后者代 表在该section中的第几行。
}
```

###四、为数字块添加动画效果
首先，在GamebordView类中添加如下属性： 
     
    
    ```
    let tilePopDelay: NSTimeInterval = 0.05
    let tileExpandTime: NSTimeInterval = 0.18
    let tilePopMaxScale: CGFloat = 1.1
    let tileContractTime: NSTimeInterval = 0.08
      
    ```
    
    
    
接着，在上面的insertTile方法的尾部加入以下代码：
 

```
UIView.animateWithDuration(tileExpandTime, delay: tilePopDelay, options: UIViewAnimationOptions.TransitionNone,
        animations: {            tileView.layer.setAffineTransform(CGAffineTransformMakeScale(self.tilePopMaxScale, self.tilePopMaxScale))
        },
        completion: { finished in
            UIView.animateWithDuration(self.tileContractTime, animations: { () -> Void in
            tileView.layer.setAffineTransform(CGAffineTransformIdentity)
        })
    })
```

###五、调用数字块
在主控制器NumbertailGameController中初始化GamebordView的实例，先在NumbertailGameController类中添加属性：


```
var bord : GamebordView?
```


并在NumbertailGameController类的setupGame方法调用view.addSubview(gamebord)后对bord进行赋值：


```
bord = gamebord
```

##删掉下面
从而可以通过GamebordView的实例bord来调用GamebordView的insertTile(pos, value: value)方法，为此在NumbertailGameController类中也添加一个insertTile方法:


```
  func insertTile(position:(Int,Int),value:Int){
        assert(bord != nil)
        let b=bord!
        b.insertTile(position, value: value)
    }
```

##删掉上面
在setupGame方法中调用view.addSubview(gamebord)前调用insetTile方法，从而在（1，3）和（3，1）位置上新建小数字块：


```
     gamebord.insertTile((3,1), value: 2)
     gamebord.insertTile((1,3), value: 2)
```


运行程序，此时在主面板上有一个计分板以及一个游戏面板，在游戏面板可看到两个数值为2的数字块。

###六、随机插入数字块
每当用户滑动一次屏幕时，都应该在空闲的位置随机抽取一块添加数值为2的数字块。为此，选用一个数组结构来存储已经添加进游戏的数字块，数组中存储当前块的值，通过当前值是否为空来判断这个位置是否空闲。新建一个BaseModle.swift文件，将该文件自动生成的类删除，添加一下代码：


```
//数组中存放的枚举，要么空要么一个带值的Tile
//存放实际值的数组
enum TileEnum{
    case Empty
    case Tile(Int)
}

struct SequeceGameBord<T> {
    var demension:Int
    //存放实际值的数组
    var tileArray:[T]
    
    init(demension d:Int,initValue:T){
        self.demension=d
        tileArray=[T](count: d*d, repeatedValue : initValue)//设置tileArray包含b*b个元素，每个元素初始值为initValue
    }
    //subscript就是给结构体定义下标访问方式，通过当前的x,y坐标来计算存储和取出的位置
    subscript(row:Int,col:Int)->T{
        get{
            assert(row>=0&&row<demension&&col>=0&&col<demension)
            return tileArray[row*demension+col]
        }
        set{
            assert(row>=0&&row<demension&&col>=0&&col<demension)
            tileArray[row*demension+col]=newValue
        }
    }
    //mutating是结构体在修改自身属性时必须要加的。
    mutating func setAll(value:T){
        for i in 0..<demension{
            for j in 0..<demension{
                self[i,j]=value
            }
        }
    
    }
    
}
```

此时可知，存放整个数字块状态的结构体就是一个`SequenceGamebord<TileEnum>`。







##问题：
1.import Foundation需要吗？
2. enum TileEnum{
    case Empty
    case Tile(Int)
}
中的Tile（Int）什么意思？？































