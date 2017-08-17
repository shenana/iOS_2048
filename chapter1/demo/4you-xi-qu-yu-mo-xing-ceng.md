接下来，我们需要新建一个GameModel.swift充当游戏区域的model层，来记录当前游戏的状态以及提供一些游戏自身的操作等。

###一、建立GameModelProtocol协议
在此之前，先在NumbertailGameController.swift文件下建立GameModelProtocol协议:


```
protocol GameModelProtocol:class{
    func changeScore(score:Int)
    func insertTile(position:(Int,Int),value:Int)
    func moveOneTile(from:(Int,Int),to:(Int,Int),value:Int)
    func moveTwoTile(from:((Int,Int),(Int,Int)),to:(Int,Int),value:Int)
}
```
###二、实例化ScoreView
在主控制器NumbertailGameController中初始化ScoreView的实例，先在NumbertailGameController类中添加属性：


```
var scoreV : ScoreView?
```


并在NumbertailGameController类的setupGame方法调用view.addSubview(scoreView)后对scorV进行赋值：


```
scoreV=scoreView
```


###三、令NumbertailGameController类继承该GameModelProtocol协议
先令NumbertailGameController类继承GameModelProtocol协议，接着必须在类里实现该协议的方法。先实现两个方法： 


```
    func changeScore(score:Int){
        assert(scoreV != nil)
        let s=scoreV!
        s.scoreChanged(newScore: score)
    }
    //该insetTile是GameModelProtocol协议的
    func insertTile(position:(Int,Int),value:Int){
           assert(bord != nil)
           let b=bord!
           b.insertTile(position, value: value)
    }

```
此时还有GameModelProtocol的moveOneTile和moveTwoTile两个方法未实现，在此之前先GamebordView类中实现这两个方法，首先是moveOneTile方法：


```
    func moveOneTile(from:(Int,Int),to:(Int,Int),value:Int){
        let (fx,fy)=from
        let (tx,ty)=to
        let fromKey=NSIndexPath(forRow: fx, inSection: fy)
        let toKey=NSIndexPath(forRow: tx, inSection: ty)
        //取出from位置的数字块
        guard let tile=tiles[fromKey] else{
            assert(false,"not exit tile")
        }
        //取出to位置的数字块
        let endTile=tiles[toKey]
        //将from位置的数字块的位置定到to位置
        var changeFrame=tile.frame
        changeFrame.origin.x=tilePadding+CGFloat(tx)*(tilePadding+tileWidth)
        changeFrame.origin.y=tilePadding+CGFloat(ty)*(tilePadding+tileWidth)
        //删除from位置的值
        tiles.removeValueForKey(fromKey)
        //将to位置存储为from位置的值
        tiles[toKey]=tile
    
    }

```

在moveOneTile方法里添加动画以及给新位置的数字块赋值：




接着是moveTwoTile方法：



在moveTwoTile方法里添加动画以及给新位置的数字块赋值：












