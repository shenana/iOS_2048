
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
####1.实现changeScore()方法和insertTile()方法
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
#####此时还有GameModelProtocol的moveOneTile和moveTwoTile两个方法未实现，在此之前先GamebordView类中实现这两个方法。

####2.实现moveOneTile方法
####2.1 在GamebordView类中实现moveOneTile方法：


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


```
     let shouldPop = endTile != nil   
     UIView.animateWithDuration(perSquareSlideDuration,
            delay: 0.0,
            options: UIViewAnimationOptions.BeginFromCurrentState,
            animations: {
                tile.frame = changeFrame
            },
            completion: { (finished: Bool) -> Void in
                //对新位置的数字块赋值
                tile.value = value
                endTile?.removeFromSuperview()
                if !shouldPop || !finished {
                    return
                }
                tile.layer.setAffineTransform(CGAffineTransformMakeScale(self.tileMergeStartScale, self.tileMergeStartScale))
                UIView.animateWithDuration(self.tileMergeExpandTime,
                    animations: {
                        tile.layer.setAffineTransform(CGAffineTransformMakeScale(self.tilePopMaxScale, self.tilePopMaxScale))
                    },
                    completion: { finished in
                        UIView.animateWithDuration(self.tileMergeContractTime) {
                            tile.layer.setAffineTransform(CGAffineTransformIdentity)
                        }
                })
        })
```
同时得在GamebordView类中添加属性：

```
let perSquareSlideDuration: NSTimeInterval = 0.08
let tileMergeStartScale: CGFloat = 1.0
let tileMergeExpandTime: NSTimeInterval = 0.08
let tileMergeContractTime: NSTimeInterval = 0.08
```
####2.2 在NumbertailGameController类中实现moveOneTile方法：


```
 func moveOneTile(from:(Int,Int),to:(Int,Int),value:Int){
        assert(bord != nil)
        let b=bord!
        b.moveOneTile(from, to: to, value: value)
    }
```







####3.实现moveTwoTile方法
####3.1 在GamebordView类中实现moveTwoTile方法：

```
    func moveTwoTile(from:((Int,Int),(Int,Int)),to:(Int,Int), value:Int){ 
        assert(positionIsValied(from.0)&&positionIsValied(from.1)&&positionIsValied(to))
        let (fromKeyAx,fromKeyAy)=from.0
        let (fromKeyBx,fromKeyBy)=from.1
        let (toKeyX,toKeyY)=to
        let fromKeyA=NSIndexPath(forRow: fromKeyAx, inSection: fromKeyAy)
        let fromKeyB=NSIndexPath(forRow: fromKeyBx, inSection: fromKeyBy)
        let endTile=NSIndexPath(forRow: toKeyX, inSection: toKeyY)
        
        guard let tileA=tiles[fromKeyA] else{
            assert(false,"no exit tile")
        }
        guard let tileB=tiles[fromKeyB] else{
            assert(false, "no exit tile")
        }
        
        var changeTile=tileA.frame
        changeTile.origin.x=tilePadding+CGFloat(toKeyX)*(tileWidth+tilePadding)
        changeTile.origin.y=tilePadding+CGFloat(toKeyY)*(tileWidth+tilePadding)
        
        let oldTile=tiles[endTile]
        oldTile?.removeFromSuperview()
        tiles.removeValueForKey(fromKeyA)
        tiles.removeValueForKey(fromKeyB)
        tiles[endTile]=tileA
    }
```
在moveTwoTile方法里添加动画以及给新位置的数字块赋值：


```
    UIView.animateWithDuration(perSquareSlideDuration,
            delay: 0.0,
            options: UIViewAnimationOptions.BeginFromCurrentState,
            animations: {
                tileA.frame = changeTile
                tileB.frame = changeTile
            },
            completion: { finished in
                //赋值
                tileA.value = value
                tileB.removeFromSuperview()
                if !finished {
                    return
                }
                tileA.layer.setAffineTransform(CGAffineTransformMakeScale(self.tileMergeStartScale, self.tileMergeStartScale))
                UIView.animateWithDuration(self.tileMergeExpandTime,
                    animations: {
                        tileA.layer.setAffineTransform(CGAffineTransformMakeScale(self.tilePopMaxScale, self.tilePopMaxScale))
                    },
                    completion: { finished in
                        UIView.animateWithDuration(self.tileMergeContractTime) {
                            tileA.layer.setAffineTransform(CGAffineTransformIdentity)
                        }
                })
        })


```
####3.2 在NumbertailGameController类中实现moveTwoTile方法：


```
 func moveTwoTile(from:((Int,Int),(Int,Int)),to:(Int,Int),value:Int){
        assert(bord != nil)
        let b=bord!
        b.moveTwoTile(from, to: to, value: value)
    }

```














