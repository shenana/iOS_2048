* 使用func来声明一个函数，使用名字和参数来调用函数。使用->来指定函数返回值。


```
    //声明
    func greet(name: String, day: String) -> String {
    return "Hello \(name), today is \(day)."
    }
    //调用
    greet(name: "Bob", day: "Tuesday")

```
* 使用一个元组来返回多个值。
 

```
    //声明
    func getGasPrices() -> (Double, Double, Double) {
    return (3.59, 3.69, 3.79)
    }
    //调用
    getGasPrices()
```

* 函数可以带有可变个数的参数，这些参数在函数内表现为数组的形式：


```
    func sumOf(numbers: Int...) -> Int {
    var sum = 0
    for number in numbers {
    sum += number
    }
    return sum
    }
    sumOf()
    sumOf(numbers:42, 597, 12)

```
* 函数也可以当做参数传入另一个函数。


```
    func hasAnyMatches(list: [Int], condition: (Int) -> Bool) -> Bool {
    for item in list {
    if condition(item) {
    return true
    }
    }
    return false
    }
    func lessThanTen(number: Int) -> Bool {
    return number < 10
    }
    var numbers = [20, 19, 7, 12]
    hasAnyMatches(list: numbers, condition: lessThanTen)
```

* 函数实际上是一种特殊的闭包，你可以使用{}来创建一个匿名闭包。使用in将参数和返回值类型声明与闭
包函数体进行分离。
    
    
    ```
    numbers.map({
    (number: Int) -> Int in
    let result = 3 * number
    return result
    })
    ```

* 如果一个闭包的类型已知，比如作为一个回调函数，你可以忽略参数的类型和
返回值。单个语句闭包会把它语句的值当做结果返回。


```
    numbers.map({ number in 3 * number })
```

* 属性可以有 getter 和 setter 。例如某个类中有属性var的声明如下：


```
    var perimeter: Double {
    get {
    return 3.0 * sideLength
    }
    set {
    sideLength = newValue / 3.0
    }
    }
```

* 类中的方法和一般的函数有一个重要的区别，函数的参数名只在函数内部使用，但是方法的参数名需要在
调用的时候显式说明（除了第一个参数）。默认情况下，方法的参数名和它在方法内部的名字一样，不过
你也可以定义第二个名字，这个名字被用在方法内部。


```
class Counter {
var count: Int = 0
func incrementBy(amount: Int, numberOfTimes times: Int) {
count += amount * times
}
}
var counter = Counter()
counter.incrementBy(2, numberOfTimes: 7)


```



    
    






