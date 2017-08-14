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







