* 凡在变量或常量后加上?的都是一个可选变量/可选常量
凡在变量或常量后加上!的都是隐式可选变量/常量，首先该变量或常量满足可选类型，其主要是可被当成一般的变量/常量来使用，而不需要每次都验证是否有值。

* 如果一个隐式解包的可选类型不包含一个实际值，那么对它的访问会抛出一个运行时错误。在变量/常量名后面加！的情况也是一样的。如以下对比的两个例子：
  例一：不加！的情况，输出为nil，不报错。

        var possibleString: String? = "An optional string."  
        possibleString = nil  
        println(possibleString) </span>

  例二：加！的情况，运行到println就会crash了。会报fatal error: Can't unwrap Optional.None错误。

        var possibleString: String? = "An optional string."  
        possibleString = nil
        println(possibleString！) </span>
* swift 的nil不同于Object-C中的nil. Object-C中，nil是一个指针指向不存在的对象。Swift中，nil不是指针而是一个特定类型的空值。任何类型的可选变量都可以被设为nil，不光是指针。

* 对于var view:UIView，当整个应用中或整个类中不可能存在view = nil的情况时可以设置为var view:UIView! 否则就可声明为var view:UIView?

* 加深理解例子一：
```
      var optionVariables:String? //?相当于下面这种写法的语法糖  
      //var optionVariables : Optional<Int>  
      let value = optionVariables?.hashValue  
      /* 
      optionVariables是可选类型的字符串，如果optionVariables是nil，则hashValue也为nil 
      如果optionVariables不为nil，hashValue就是strValue字符串的哈希值 
      到这里我们看到了?的两种使用场景: 
      1.声明Optional值变量 
      2.用在对Optional值操作中，用来判断是否能响应后面的操作 
      */  
  
       //对于可选类型变量，不能直接进行操作，否则会报错  
       //let hashval = optionVariables.hashValue //'String?' does not have a member named 'hashValue'  
       //因此要访问值就需要解包，解包有两种  
       //第一种：使用if let/var xxx =   
       if let hv = optionVariables  
       {  
            //run ok;  
       }  
  
       //第二种：使用!号  
       let hv = optionVariables!.hashValue  
  
       //这里的!表示“我确定这里的的strValue一定是非nil的，尽情调用吧” ，比如这种情况:  
  
       if optionVariables {  
           let hashv = optionVariables!.hashValue  
       }  
       //{}里的optionVariables一定是非nil的，所以就能直接加上!，强制拆包(unwrap)并执行后面的操作 
```

* 加深理解例子二：
       

```
        var btn  :UIButton?      // 默认btn = nil  
        var btn2 :UIButton = UIButton()    // 默认实例化一个对对象  
        var btn3 :UIButton!     // 默认btn = nil  
          
        //运行会报错fatal error: Can't unwrap Optional.None 因btn ＝ nil  
        if var tmpbtn = btn  
        {  
            btn!.tintColor = UIColor.blackColor()  
            btn!.imageEdgeInsets = UIEdgeInsets(top:1,left:2,bottom:3,right:4)  
            btn!.frame = CGRectMake(0,0,50,40)  
        }  
          
        //运行正常  
        btn2.tintColor = UIColor.blackColor()  
        btn2.imageEdgeInsets = UIEdgeInsets(top:1,left:2,bottom:3,right:4)  
        btn2.frame = CGRectMake(0,0,50,40)  
          
        //运行会报错fatal error: Can't unwrap Optional.None 因btn3 = nil  
        if var tmpbtn = btn  
        {  
            btn3.tintColor = UIColor.blackColor()  
            btn3.imageEdgeInsets = UIEdgeInsets(top:1,left:2,bottom:3,right:4)  
            btn3.frame = CGRectMake(0,0,50,40)  
        }
```

