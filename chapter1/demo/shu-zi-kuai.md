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
为了将数字块添加到游戏面板中，在GamebordView.swift中新增如下方法


func positionIsValied(position : (Int , Int)) -> Bool{
    let (x , y) = position
    return x >= 0 && x < dimension && y >= 0 && y < dimension
}



为方便根据数值使得数字块、数值显示各种不同的颜色，先在GamebordView类中添加实例：
为了将数字块添加到游戏面板中，在GamebordView.swift中新增如下方
 `let provider = AppearanceProvider()`
为了将数字块添加到游戏面板中，在GamebordView.swift中新增如下方法



UITableView声明了一个NSIndexPath的类别，主要用 来标识当前cell的在tableView中的位置，该类别有section和row两个属性，前者标识当前cell处于第几个section中，后者代 表在该section中的第几行。
































