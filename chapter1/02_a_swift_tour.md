> 翻譯：[tommy60703](https://github.com/tommy60703)

# Swift 初見

---

本頁內容包括：

-   [簡單值（Simple Values）](#simple_values)
-   [控制流程（Control Flow）](#control_flow)
-   [函式和閉包（Functions and Closures）](#functions_and_closures)
-   [物件和類別（Objects and Classes）](#objects_and_classes)
-   [列舉和結構（Enumerations and Structures）](#enumerations_and_structures)
-   [協議和擴展（Protocols and Extensions）](#protocols_and_extensions)
-   [泛型（Generics）](#generics)

通常來說，程式語言教程中的第一個程式應該在螢幕上列印 “Hello, world”。在 Swift 中，可以用一行程式碼實現：

```swift
println("Hello, world")
```

如果你寫過 C 或者 Objective-C 程式，那你應該很熟悉這種形式——在 Swift 中，這行程式碼就是一個完整的程式。你不需要為了輸入輸出或者字串處理導入一個單獨的函式庫。全域作用域中的程式碼會被自動當做程式的入口點，所以你也不需要`main`函式。你同樣不需要在每個語句結尾寫上分號。

這個教程會通過一系列程式例子來讓你對 Swift 有初步了解，如果你有什麼不理解的地方也不用擔心——任何本章介紹的內容都會在後面的章節中詳細講解。

> 注意：  
> 為了獲得最好的體驗，在 Xcode 當中使用程式碼預覽功能。程式碼預覽功能可以讓你編輯程式碼並即時看到執行結果。

<a name="simple_values"></a>
## 簡單值

使用`let`來宣告常數，使用`var`來宣告變數。一個常數的值，在編譯的時候，並不需要有明確的值，但是你只能為它指派一次。也就是說你可以用常數來表示這樣一個值：你只需要決定一次，但是需要使用很多次。

```swift
var myVariable = 42
myVariable = 50
let myConstant = 42
```

常數或者變數的型別必須和你指派它們的值一樣。然而，宣告時型別是可選的，宣告的同時指派值的話，編譯器會自動推斷型別。在上面的例子中，編譯器推斷出`myVariable`是一個整數（integer）因為它的初始值是整數。

如果初始值沒有提供足夠的信息（或者沒有初始值），那你需要在變數後面宣告型別，用冒號分開。

```swift
let implicitInteger = 70
let implicitDouble = 70.0
let explicitDouble: Double = 70
```

> 練習：  
> 創建一個常數，顯式指定型別行別為`Float`並指派初始值為 4。

值永遠不會被隱式轉換為其他型別行別。如果你需要把一個值轉換成其他型別行別，請顯式轉換。

```swift
let label = "The width is"
let width = 94
let widthLabel = label + String(width)
```
> 練習：  
> 刪除最後一行中的`String`，錯誤提示是什麼？

有一種更簡單的把數值轉換成字串的方法：把數值寫到括號中，並且在括號之前寫一個反斜線。例如：

```swift
let apples = 3
let oranges = 5
let appleSummary = "I have \(apples) apples."
let fruitSummary = "I have \(apples + oranges) pieces of fruit."
```

> 練習：  
> 使用`\()`來把一個浮點數計算轉換成字串，並加上某人的名字，和他打個招呼。

使用方括號`[]`來創建陣列和字典，並使用下標或者鍵值（key）來存取元素。

```swift
var shoppingList = ["catfish", "water", "tulips", "blue paint"]
shoppingList[1] = "bottle of water"
```

```swift
var occupations = [
    "Malcolm": "Captain",
    "Kaylee": "Mechanic",
]
occupations["Jayne"] = "Public Relations"
```

要創建一個空陣列或者空字典，使用初始化語法。

```swift
let emptyArray = String[]()
let emptyDictionary = Dictionary<String, Float>()
```

如果型別訊息可以被推斷出來，你可以用`[]`和`[:]`來創建空陣列和空字典——就像你宣告變數或者給函式傳參數的時候一樣。

```swift
shoppingList = []   // 去逛街並買點東西
```

<a name="control_flow"></a>
## 控制流程

使用`if`和`switch`來進行條件操作，使用`for-in`、`for`、`while`和`do-while`來進行迴圈。包住條件和迴圈變數的小括號可以省略，但是語句體的大括號是必須的。

```swift
let individualScores = [75, 43, 103, 87, 12]
var teamScore = 0
for score in individualScores {
    if score > 50 {
        teamScore += 3
    } else {
        teamScore += 1
    }
}
teamScore
```

在`if`語句中，條件必須是一個布林表達式——這意味著像`if score { ... }`這樣的程式碼將報錯，而不會隱性地與 0 做對比。

你可以一起使用`if`和`let`來處理可能沒有值的情況。有些變數的值是可選的。一個可選的值可能是一個具體的值或者是`nil`，表示沒有值。在型別後面加一個問號來表示這個變數的值是可選的。

```swift
var optionalString: String? = "Hello"
optionalString == nil

var optionalName: String? = "John Appleseed"
var greeting = "Hello!"
if let name = optionalName {
    greeting = "Hello, \(name)"
}
```

> 練習：  
> 把`optionalName`改成`nil`，greeting 會是什麼？增加一個`else`語句，當`optionalName`是`nil`時給 greeting 指派一個不同的值。

如果變數的可選值是`nil`，條件會判斷為`false`，大括號中的程式碼會被跳過。如果不是`nil`，會將值指派給`let`後面的常數，這樣程式碼區塊中就可以使用這個值了。

`switch`支援任意型別的資料以及各種比較操作——不僅僅是整數以及相等測試。

```swift
let vegetable = "red pepper"
switch vegetable {
case "celery":
    let vegetableComment = "Add some raisins and make ants on a log."
case "cucumber", "watercress":
    let vegetableComment = "That would make a good tea sandwich."
case let x where x.hasSuffix("pepper"):
    let vegetableComment = "Is it a spicy \(x)?"
default:
    let vegetableComment = "Everything tastes good in soup."
}
```

> 練習：  
> 刪除`default`語句，看看會有什麼錯誤？

執行`switch`中相符的子句之後，程式會退出`switch`語句，並不會繼續向下執行，所以不需要在每個子句結尾寫`break`。

你可以使用`for-in`來迭代字典，需要兩個變數來表示每個鍵值組。

```swift
let interestingNumbers = [
    "Prime": [2, 3, 5, 7, 11, 13],
    "Fibonacci": [1, 1, 2, 3, 5, 8],
    "Square": [1, 4, 9, 16, 25],
]
var largest = 0
for (kind, numbers) in interestingNumbers {
    for number in numbers {
        if number > largest {
            largest = number
        }
    }
}
largest
```

> 練習：  
> 增加另一個變數來記錄哪種型別的數字是最大的。

使用`while`來重復執行一段程式碼直到條件不滿足。迴圈條件可以在開頭也可以在結尾。

```swift
var n = 2
while n < 100 {
    n = n * 2
}
n

var m = 2
do {
    m = m * 2
} while m < 100
m
```

你可以在迴圈中使用`..`來表示範圍，也可以使用傳統的寫法，兩者是等價的：

```swift
var firstForLoop = 0
for i in 0..3 {
    firstForLoop += i
}
firstForLoop

var secondForLoop = 0
for var i = 0; i < 3; ++i {
    secondForLoop += 1
}
secondForLoop
```

使用`..`創建的範圍不包含上界，如果想包含的話需要使用`...`。

<a name="functions_and_closures"></a>
## 函式和閉包

使用`func`來宣告一個函式，使用名字和參數來呼叫函式。使用`->`來指定函式回傳值。

```swift
func greet(name: String, day: String) -> String {
    return "Hello \(name), today is \(day)."
}
greet("Bob", "Tuesday")
```

> 練習：  
> 刪除`day`參數，增加一個參數來表示今天吃了什麼午飯。

使用一個 tuple 來回傳多個值。

```swift
func getGasPrices() -> (Double, Double, Double) {
    return (3.59, 3.69, 3.79)
}
getGasPrices()
```

函式可以帶有可變個數的參數，這些參數在函式內表現為 tuple 的形式：

```swift
func sumOf(numbers: Int...) -> Int {
    var sum = 0
    for number in numbers {
        sum += number
    }
    return sum
}
sumOf()
sumOf(42, 597, 12)
```

> 練習：  
> 寫一個計算參數平均值的函式。

函式可以是巢狀的。巢狀的函式可以存取外側函式的變數，你可以使用巢狀函式來重構一個太長或者太復雜的函式。

```swift
func returnFifteen() -> Int {
    var y = 10
    func add() {
        y += 5
    }
    add()
    return y
}
returnFifteen()
```

函式是 first-class 型別，這意味著函式可以作為另一個函式的回傳值。

```swift
func makeIncrementer() -> (Int -> Int) {
    func addOne(number: Int) -> Int {
        return 1 + number
    }
    return addOne
}
var increment = makeIncrementer()
increment(7)
```

函式也可以當做參數傳入另一個函式。

```swift
func hasAnyMatches(list: Int[], condition: Int -> Bool) -> Bool {
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
hasAnyMatches(numbers, lessThanTen)
```

函式實際上是一種特殊的閉包，你可以使用`{}`來創建一個匿名閉包。使用`in`分離參數和回傳值型別宣告與閉包函式主體。

```swift
numbers.map({
    (number: Int) -> Int in
    let result = 3 * number
    return result
})
```

> 練習：  
> 重寫閉包，對所有奇數回傳 0.

有很多種創建閉包的方法。如果一個閉包的型別已知，比如作為一個 callback 函式，你可以忽略參數的型別和回傳值。單個語句閉包會把它語句的值當做結果返回。

```swift
numbers.map({ number in 3 * number })
```

你可以透過參數位置而不是參數名字來取用參數——這個方法在非常短的閉包中非常有用。當一個閉包作為最後一個參數傳給一個函式的時候，它可以直接跟在括號後面。

```swift
sort([1, 5, 3, 12, 2]) { $0 > $1 }
```

<a name="objects_and_classes"></a>
## 物件和類別

使用`class`和類別名稱來創建一個類別。類別中屬性的宣告和常數、變數聲明一樣，唯一的區別就是它們的上下文（context）是類別。同樣，方法和函式宣告也一樣。

```swift
class Shape {
    var numberOfSides = 0
    func simpleDescription() -> String {
        return "A shape with \(numberOfSides) sides."
    }
}
```

> 練習：  
> 使用`let`增加一個常數屬性，再增加一個接收一個參數的方法。

要創建一個類別的實例，在類別名稱後面加上括號。使用點運算子來存取實例的屬性和方法。

```swift
var shape = Shape()
shape.numberOfSides = 7
var shapeDescription = shape.simpleDescription()
```

這個版本的`Shape`類別缺少了一些重要的東西：一個初始化函式來初始化類別實例。使用`init`來創建一個初始化函式。

```swift
class NamedShape {
    var numberOfSides: Int = 0
    var name: String

    init(name: String) {
        self.name = name
    }

    func simpleDescription() -> String {
        return "A shape with \(numberOfSides) sides."
    }
}
```

注意`self`被用來區別實例變數和初始化函式的參數。當你創建實例的時候，像傳入函式參數一樣給類別傳入初始化函式的參數。每個屬性都需要指派值——無論是透過宣告式（就像`numberOfSides`）還是透過初始化函式（就像`name`）。

如果你需要在刪除物件之前進行一些清理工作，使用`deinit`創建一個反初始化函式。

子類別的定義方法是在它們的類別名稱後面加上父類別的名稱，並用冒號分開。類別並不一定需要一個標準的根類別，所以你可以忽略父類別。

子類別如果要覆寫父類別的方法的話，需要用`override`標記——如果沒有添加`override`就覆寫父類別方法的話，編譯器會報錯。編譯器同樣會檢測`override`標記的方法是否確實存在父類別中。

```swift
class Square: NamedShape {
    var sideLength: Double

    init(sideLength: Double, name: String) {
        self.sideLength = sideLength
        super.init(name: name)
        numberOfSides = 4
    }

    func area() ->  Double {
        return sideLength * sideLength
    }

    override func simpleDescription() -> String {
        return "A square with sides of length \(sideLength)."
    }
}
let test = Square(sideLength: 5.2, name: "my test square")
test.area()
test.simpleDescription()
```

> 練習：  
> 創建`NamedShape`的另一個子類別`Circle`，初始化函式接收半徑和名稱兩個參數，實做`area`和`describe`方法。

屬性可以有 getter 和 setter 。

```swift
class EquilateralTriangle: NamedShape {
    var sideLength: Double = 0.0

    init(sideLength: Double, name: String) {
        self.sideLength = sideLength
        super.init(name: name)
        numberOfSides = 3
    }

    var perimeter: Double {
    get {
        return 3.0 * sideLength
    }
    set {
        sideLength = newValue / 3.0
    }
    }

    override func simpleDescription() -> String {
        return "An equilateral triagle with sides of length \(sideLength)."
    }
}
var triangle = EquilateralTriangle(sideLength: 3.1, name: "a triangle")
triangle.perimeter
triangle.perimeter = 9.9
triangle.sideLength
```

在`perimeter`的 setter 中，新值的名稱是`newValue`。你可以在`set`之後顯式的提供一個名稱。

注意`EquilateralTriangle`類別的初始化函式執行了三步：

1. 設置子類別宣告的屬性值
2. 呼叫父類別的初始化函式
3. 改變父類別定義的屬性值。其他的工作比如呼叫方法、getters 和 setters 也可以在這個階段完成。

如果你不需要計算屬性，但是仍然需要在設置一個新值之前或者之後執行程式碼，使用`willSet`和`didSet`。

比如，下面的類別確保三角形的邊長總是和正方形的邊長相同。

```swift
class TriangleAndSquare {
    var triangle: EquilateralTriangle {
    willSet {
        square.sideLength = newValue.sideLength
    }
    }
    var square: Square {
    willSet {
        triangle.sideLength = newValue.sideLength
    }
    }
    init(size: Double, name: String) {
        square = Square(sideLength: size, name: name)
        triangle = EquilateralTriangle(sideLength: size, name: name)
    }
}
var triangleAndSquare = TriangleAndSquare(size: 10, name: "another test shape")
triangleAndSquare.square.sideLength
triangleAndSquare.triangle.sideLength
triangleAndSquare.square = Square(sideLength: 50, name: "larger square")
triangleAndSquare.triangle.sideLength
```

類別中的方法和一般的函式有一個重要的區別，函式的參數名稱只在函式內部使用，但是方法的參數名稱需要在呼叫的時候也會被使用（除了第一個參數）。預設情況下，方法的參數名稱和它在方法內部的名稱一樣，不過你也可以定義第二個名稱，這個名稱被用在方法內部。

```swift
class Counter {
    var count: Int = 0
    func incrementBy(amount: Int, numberOfTimes times: Int) {
        count += amount * times
    }
}
var counter = Counter()
counter.incrementBy(2, numberOfTimes: 7)
```

處理可選值時，你可以在操作（比如方法、屬性和 Subscript）之前加`?`。如果`?`之前的值是`nil`，`?`後面的東西都會被忽略，並且整個表達式回傳`nil`。否則，`?`之後的東西都會被執行。在這兩種情況下，整個表達式的值也是一個可選值。

```swift
let optionalSquare: Square? = Square(sideLength: 2.5, name: "optional square")
let sideLength = optionalSquare?.sideLength
```

<a name="enumerations_and_structure"></a>
## 列舉和結構

使用`enum`來創建一個列舉。就像類別和其他所有命名型別一樣，列舉可以包含方法。

```swift
enum Rank: Int {
    case Ace = 1
    case Two, Three, Four, Five, Six, Seven, Eight, Nine, Ten
    case Jack, Queen, King
    func simpleDescription() -> String {
        switch self {
        case .Ace:
            return "ace"
        case .Jack:
            return "jack"
        case .Queen:
            return "queen"
        case .King:
            return "king"
        default:
            return String(self.toRaw())
        }
    }
}
let ace = Rank.Ace
let aceRawValue = ace.toRaw()
```

> 練習：  
> 寫一個函式，透過比較它們的原始值來比較兩個`Rank`值。

在上面的例子中，列舉原始值的型別行別是`Int`，所以你只需要設置第一個原始值。剩下的原始值會按照順序被指派。你也可以使用字串或者浮點數作為列舉的原始值。

使用`toRaw`和`fromRaw`函式來在原始值和列舉值之間進行轉換。

```swift
if let convertedRank = Rank.fromRaw(3) {
    let threeDescription = convertedRank.simpleDescription()
}
```

列舉的成員值是實際值，並不是原始值的另一種表達方法。實際上，如果原始值沒有意義，你不需要提供。

```swift
enum Suit {
    case Spades, Hearts, Diamonds, Clubs
    func simpleDescription() -> String {
        switch self {
        case .Spades:
            return "spades"
        case .Hearts:
            return "hearts"
        case .Diamonds:
            return "diamonds"
        case .Clubs:
            return "clubs"
        }
    }

}
let hearts = Suit.Hearts
let heartsDescription = hearts.simpleDescription()
```

> 練習：  
> 給`Suit`增加一個`color`方法，對`spades`和`clubs`回傳 “black”，對`hearts`和`diamonds`回傳 “red”。

注意，有兩種方式可以指向`Hearts`成員：指派值給`hearts`常數時，需要用全名來指向列舉成員`Suit.Hearts`，因為常數沒有顯式指定型別。在`switch`裡，可以使用縮寫`.Hearts`來指向列舉成員，因為`self`的值已經知道是一個`suit`。已知變數型別的情況下你可以使用縮寫。

使用`struct`來創建一個結構。結構和類別有很多相同的地方，比如方法和初始化函式。它們之間最大的一個區別就是
結構是傳值，類別是傳參考。

```swift
struct Card {
    var rank: Rank
    var suit: Suit
    func simpleDescription() -> String {
        return "The \(rank.simpleDescription()) of \
        (suit.simpleDescription())"
    }
}
let threeOfSpades = Card(rank: .Three, suit: .Spades)
let threeOfSpadesDescription = threeOfSpades.simpleDescription()
```

> 練習：  
> 給`Card`增加一個方法，創建一副完整的撲克牌並把每張牌的 rank 和 suit 對應起來。

一個列舉成員的實例可以有實例值。相同列舉成員的實例可以有不同的值。創建實例的時候傳入值即可。實例值和原始值是不同的：列舉成員的原始值對於所有實例都是相同的，而且你是在定義列舉的時候設定原始值。

例如，考慮從伺服器獲取日出和日落的時間。伺服器會回傳正常結果或者錯誤信息。

```swift
enum ServerResponse {
    case Result(String, String)
    case Error(String)
}

let success = ServerResponse.Result("6:00 am", "8:09 pm")
let failure = ServerResponse.Error("Out of cheese.")

switch success {
case let .Result(sunrise, sunset):
    let serverResponse = "Sunrise is at \(sunrise) and sunset is at \(sunset)."
case let .Error(error):
    let serverResponse = "Failure...  \(error)"
}
```

> 練習：  
> 給`ServerResponse`和`switch`增加第三種情況。

注意如何從`ServerResponse`中提取日出和日落時間。

<a name="protocols_and_extensions"></a>
## 協定和擴展

使用`protocol`來宣告一個協定。

```swift
protocol ExampleProtocol {
    var simpleDescription: String { get }
    mutating func adjust()
}
```

類別、列舉和結構都可以實做協定。

```swift
class SimpleClass: ExampleProtocol {
    var simpleDescription: String = "A very simple class."
    var anotherProperty: Int = 69105
    func adjust() {
        simpleDescription += "  Now 100% adjusted."
    }
}
var a = SimpleClass()
a.adjust()
let aDescription = a.simpleDescription

struct SimpleStructure: ExampleProtocol {
    var simpleDescription: String = "A simple structure"
    mutating func adjust() {
        simpleDescription += " (adjusted)"
    }
}
var b = SimpleStructure()
b.adjust()
let bDescription = b.simpleDescription
```

> 練習：
>
> 寫一個實做這個協定的列舉。

注意宣告`SimpleStructure`的時候`mutating`關鍵字用來標記一個會修改結構的方法。`SimpleClass`的宣告不需要標記任何方法因為類別中的方法經常會修改類別。

使用`extension`來為現有的型別增加功能，比如新的方法和參數。你可以使用擴展來在別處增加協定一致性，甚至是從外部函式庫或框架引入的一個型別，使得這個型別遵循某個協定。

```swift
extension Int: ExampleProtocol {
    var simpleDescription: String {
    return "The number \(self)"
    }
    mutating func adjust() {
        self += 42
    }
}
7.simpleDescription
```

> 練習：
>
> 給`Double`型別寫一個擴展，增加`absoluteValue`功能。

你可以像使用其他命名型別一樣使用協定名稱——例如，創建一個有不同型別但是都實做一個協定的物件集合。當你處理型別是協定的值時，協定外定義的方法是不可用的。

```swift
let protocolValue: ExampleProtocol = a
protocolValue.simpleDescription
// protocolValue.anotherProperty  // Uncomment to see the error
```

即使`protocolValue`變數執行時的型別是`simpleClass`，編譯器會把它的型別當做`ExampleProtocol`。這表示你不能呼叫類別在它實做的協定之外實做的方法或者屬性。

<a name="generics"></a>
## 泛型

在尖括號裡寫一個名稱來創建一個泛型函式或者型別。

```swift
func repeat<ItemType>(item: ItemType, times: Int) -> ItemType[] {
    var result = ItemType[]()
    for i in 0..times {
        result += item
    }
    return result
}
repeat("knock", 4)
```

你也可以創建泛型類別、列舉和結構。

```swift
// Reimplement the Swift standard library's optional type
enum OptionalValue<T> {
    case None
    case Some(T)
}
var possibleInteger: OptionalValue<Int> = .None
possibleInteger = .Some(100)
```

在型別名稱後面使用`where`來指定對型別的需求，比如，限定型別有實現某一個協定，限定兩個型別是相同的，或者限定某個類別必須有一個特定的父類別

```swift
func anyCommonElements <T, U where T: Sequence, U: Sequence, T.GeneratorType.Element: Equatable, T.GeneratorType.Element == U.GeneratorType.Element> (lhs: T, rhs: U) -> Bool {
    for lhsItem in lhs {
        for rhsItem in rhs {
            if lhsItem == rhsItem {
                return true
            }
        }
    }
    return false
}
anyCommonElements([1, 2, 3], [3])
```

> 練習：  
> 修改`anyCommonElements`函式來創建一個函式，回傳一個陣列，內容是兩個序列的共有元素。

簡單起見，你可以忽略`where`，只在冒號後面寫協定或者類別名稱。` <T: Equatable>`和`<T where T: Equatable>`是等價的。
