新建一个GameModel.swift作为游戏区域的model层，来记录当前游戏的状态以及提供一些游戏自身的操作等。

###一、新建GameModel类
在GameModel类中添加如下代码：
 

```
   let demension:Int
    let threshold:Int
    var gamebord:SequeceGameBord<TileEnum>
    unowned let delegate:GameModelProtocol
    var score:Int=0{
        didSet{
            delegate.changeScore(score)
        }
    }
    init(demension:Int,threshold:Int,delegate:GameModelProtocol){
        self.demension=demension
        self.threshold=threshold
        self.delegate=delegate
        gamebord=SequeceGameBord(demension: demension, initValue: TileEnum.Empty)
        super.init()
    }
```

###二、获取空闲块
在GameModel类类下创建下面的方法以便获取空闲块,即通过遍历`SequenceGamebord<TileEnum>`，将不为空的位置组成一个(Int , Int)的字典数组返回：


```
    func getEmptyPosition()->[(Int,Int)]{
        var emptyArr:[(Int,Int)]=[]
        for i in 0..<demension{
            for j in 0..<demension{
                if case .Empty=gamebord[i,j]{
                    emptyArr.append((i,j))
                }
                
            }
        }
        return emptyArr
    }
```
###三、随机插入方法
在GameModel类类下创建下面的方法以便实现随机插入数字块的功能，即取出当前所有的空的位置数组，在随机一个数组中的位置，之后赋值给gamebord以及调用游戏视图层渲染出新的游戏区块：


```
 func insertRandomPositionTile(value:Int){
        let emptyArr=getEmptyPosition()
        if emptyArr.isEmpty{
            return
        }
        let randomPositon=Int(arc4random_uniform(UInt32(emptyArr.count - 1)))
        let (x,y)=emptyArr[randomPositon]
        gamebord[(x,y)]=TileEnum.Tile(value)
        delegate.insertTile((x,y), value: value)
    }

```
###四、调用随机插入方法
在NumbertailGameController类中调用上面的随机插入方法，首先先在NumbertailGameController类中添加GameModle的实例gameModle：
 

```
var gameModel:GameModel?

```
在init方法中对gameModel赋值,且该赋值必须在语句`view.backgroundColor=UIColor(red: 0xE6/255, green: 0xE2/255, blue: 0xD4/255, alpha: 1)`之前，否则会gameModel为nil的错误：


```
gameModel=GameModel(demension: demension, threshold: threshold, delegate: self)
```

在setupGame()方法调用view.addSubview(gamebord)之前，将


```
        gamebord.insertTile((3,1), value: 2)
        gamebord.insertTile((1,3), value: 2)
```

换成如下代码：


```
        assert(gameModel != nil)
        let model=gameModel
        model?.insertRandomPositionTile(2)
        model?.insertRandomPositionTile(2)
        model?.insertRandomPositionTile(2)

```

此时可看到在随机位置插入了三个数字为2的数字块。

































