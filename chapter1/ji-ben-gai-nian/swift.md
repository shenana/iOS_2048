* 不需要为了输入输出或者字符串处理导入一个单独的库。全局作用域中的代码会被自动当
做程序的入口点，所以你也不需要main函数。你同样不需要在每个语句结尾写上分号。
* 使用let来声明常量，使用var来声明变量。
  * 声明时类型是可选的，声明的同时赋值的话，编译器会自动推断类型。
  * 如果初始值没有提供足够的信息（或者没有初始值），则需要在变量后面声明类型，用冒号分割。

 
   ```
      let implicitInteger = 70
      let implicitDouble = 70.0
      let explicitDouble: Double = 70 
    ```

* 有一种简单的把值转换成字符串的方法：把值写到括号中，并且在括号之前写一个反斜杠。例如：


```
let apples = 3
let oranges = 5
let appleSummary = "I have \(apples) apples."
let fruitSummary = "I have \(apples + oranges) pieces of fruit."
```

* 要创建一个空数组或者字典，使用初始化语法。


```
let emptyArray = [String]()
let emptyDictionary = Dictionary<String, Float>()
```


如果类型信息可以被推断出来，你可以用[]和[:]来创建空数组和空字典——就像你声明变量或者给函数
传参数的时候一样。


```
shoppingList = [] // 去逛街并买点东西
```
* 在if语句中，条件必须是一个布尔表达式

* 可以在循环中使用`..<`来表示范围，但是创建的范围不包含上界，如果想包含的话需要使用`...`。


```
    var firstForLoop = 0
    for i in 0..<3 {
    firstForLoop += i
    }
    firstForLoop

```








