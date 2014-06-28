> 翻譯：[lyuka](https://github.com/lyuka)
> 校對：[Hawstein](https://github.com/Hawstein)

#擴展（Extensions）
----

本頁包含內容：

- [擴展語法](#extension_syntax)
- [計算型屬性](#computed_properties)
- [建構器](#initializers)
- [方法](#methods)
- [下標](#subscripts)
- [嵌套型別](#nested_types)

*擴展*就是向一個已有的類別、結構或列舉型別添加新功能（functionality）。這包括在沒有權限獲取原始源程式碼的情況下擴展型別的能力（即*逆向建模*）。擴展和 Objective-C 中的分類別（categories）類似。（不過與Objective-C不同的是，Swift 的擴展沒有名字。）

Swift 中的擴展可以：

- 添加計算型屬性和計算靜態屬性
- 定義實例方法和型別方法
- 提供新的建構器
- 定義下標
- 定義和使用新的嵌套型別
- 使一個已有型別符合某個協定


>注意：  
如果你定義了一個擴展向一個已有型別添加新功能，那麼這個新功能對該型別的所有已有實例中都是可用的，即使它們是在你的這個擴展的前面定義的。

<a name="extension_syntax"></a>
## 擴展語法（Extension Syntax）

宣告一個擴展使用關鍵字`extension`：

```swift
extension SomeType {
    // 加到SomeType的新功能寫到這裡
}
```

一個擴展可以擴展一個已有型別，使其能夠適配一個或多個協定（protocol）。當這種情況發生時，協定的名字應該完全按照類別或結構的名字的方式進行書寫：

```swift
extension SomeType: SomeProtocol, AnotherProctocol {
    // 協定實作寫到這裡
}
```

按照這種方式添加的協定遵循者（protocol conformance）被稱之為[在擴展中添加協定遵循者](21_Protocols.html#adding_protocol_conformance_with_an_extension)

<a name="computed_properties"></a>
## 計算型屬性（Computed Properties）

擴展可以向已有型別添加計算型實例屬性和計算型型別屬性。下面的範例向 Swift 的內建`Double`型別添加了5個計算型實例屬性，從而提供與距離單位協作的基本支援。

```swift
extension Double {
    var km: Double { return self * 1_000.0 }
    var m : Double { return self }
    var cm: Double { return self / 100.0 }
    var mm: Double { return self / 1_000.0 }
    var ft: Double { return self / 3.28084 }
}
let oneInch = 25.4.mm
println("One inch is \(oneInch) meters")
// 列印輸出："One inch is 0.0254 meters"
let threeFeet = 3.ft
println("Three feet is \(threeFeet) meters")
// 列印輸出："Three feet is 0.914399970739201 meters"
```

這些計算屬性表達的含義是把一個`Double`型的值看作是某單位下的長度值。即使它們被實作為計算型屬性，但這些屬性仍可以接一個帶有dot語法的浮點型字面值，而這恰恰是使用這些浮點型字面量實作距離轉換的方式。

在上述範例中，一個`Double`型的值`1.0`被用來表示「1米」。這就是為什麼`m`計算型屬性回傳`self`——表達式`1.m`被認為是計算`1.0`的`Double`值。

其它單位則需要一些轉換來表示在米下測量的值。1千米等於1,000米，所以`km`計算型屬性要把值乘以`1_000.00`來轉化成單位米下的數值。類似地，1米有3.28024英尺，所以`ft`計算型屬性要把對應的`Double`值除以`3.28024`來實作英尺到米的單位換算。

這些屬性是唯讀的計算型屬性，所有從簡考慮它們不用`get`關鍵字表示。它們的回傳值是`Double`型，而且可以用於所有接受`Double`的數學計算中：

```swift
let aMarathon = 42.km + 195.m
println("A marathon is \(aMarathon) meters long")
// 列印輸出："A marathon is 42495.0 meters long"
```


>注意：  
擴展可以添加新的計算屬性，但是不可以添加儲存屬性，也不可以向已有屬性添加屬性觀測器(property observers)。

<a name="initializers"></a>
## 建構器（Initializers）

擴展可以向已有型別添加新的建構器。這可以讓你擴展其它型別，將你自己的定制型別作為建構器參數，或者提供該型別的原始實作中沒有包含的額外初始化選項。  

擴展能向類別中添加新的便利建構器，但是它們不能向類別中添加新的指定建構器或析構函式。指定建構器和析構函式必須總是由原始的類別實作來提供。

> 注意：  
如果你使用擴展向一個值型別添加一個建構器，該建構器向所有的儲存屬性提供預設值，而且沒有定義任何定制建構器（custom initializers），那麼對於來自你的擴展建構器中的值型別，你可以呼叫預設建構器(default initializers)和逐一成員建構器(memberwise initializers)。  
正如在值型別的建構器授權中描述的，如果你已經把建構器寫成值型別原始實作的一部分，上述規則不再適用。

下面的範例定義了一個用於描述幾何矩形的定制結構`Rect`。這個範例同時定義了兩個輔助結構`Size`和`Point`，它們都把`0.0`作為所有屬性的預設值：

```swift
struct Size {
    var width = 0.0, height = 0.0
}
struct Point {
    var x = 0.0, y = 0.0
}
struct Rect {
    var origin = Point()
    var size = Size()
}
```

因為結構`Rect`提供了其所有屬性的預設值，所以正如預設建構器中描述的，它可以自動接受一個預設的建構器和一個成員級建構器。這些建構器可以用於建構新的`Rect`實例：

```swift
let defaultRect = Rect()
let memberwiseRect = Rect(origin: Point(x: 2.0, y: 2.0),
    size: Size(width: 5.0, height: 5.0))
```

你可以提供一個額外的使用特殊中心點和大小的建構器來擴展`Rect`結構：

```swift
extension Rect {
    init(center: Point, size: Size) {
        let originX = center.x - (size.width / 2)
        let originY = center.y - (size.height / 2)
        self.init(origin: Point(x: originX, y: originY), size: size)
    }
}
```

這個新的建構器首先根據提供的`center`和`size`值計算一個合適的原點。然後呼叫該結構自動的成員建構器`init(origin:size:)`，該建構器將新的原點和大小存到了合適的屬性中：

```swift
let centerRect = Rect(center: Point(x: 4.0, y: 4.0),
    size: Size(width: 3.0, height: 3.0))
// centerRect的原點是 (2.5, 2.5)，大小是 (3.0, 3.0)
```


>注意：  
如果你使用擴展提供了一個新的建構器，你依舊有責任保證建構過程能夠讓所有實例完全初始化。

<a name="methods"></a>
## 方法（Methods）

擴展可以向已有型別添加新的實例方法和型別方法。下面的範例向`Int`型別添加一個名為`repetitions`的新實例方法：

```swift
extension Int {
    func repetitions(task: () -> ()) {
        for i in 0..self {
            task()
        }
    }
}
```

這個`repetitions`方法使用了一個`() -> ()`型別的單參數（single argument），表明函式沒有參數而且沒有回傳值。

定義該擴展之後，你就可以對任意整數呼叫`repetitions`方法,實作的功能則是多次執行某任務：

```swift
3.repetitions({
    println("Hello!")
    })
// Hello!
// Hello!
// Hello!
```

可以使用 trailing 閉包使呼叫更加簡潔：

```swift
3.repetitions{
    println("Goodbye!")
}
// Goodbye!
// Goodbye!
// Goodbye!
```

<a name="mutating_instance_methods"></a>
### 修改實例方法（Mutating Instance Methods）

通過擴展添加的實例方法也可以修改該實例本身。結構和列舉型別中修改`self`或其屬性的方法必須將該實例方法標注為`mutating`，正如來自原始實作的修改方法一樣。

下面的範例向Swift的`Int`型別添加了一個新的名為`square`的修改方法，來實作一個原始值的平方計算：

```swift
extension Int {
    mutating func square() {
        self = self * self
    }
}
var someInt = 3
someInt.square()
// someInt 現在值是 9
```

<a name="subscripts"></a>
## 下標（Subscripts）

擴展可以向一個已有型別添加新下標。這個範例向Swift內建型別`Int`添加了一個整型下標。該下標`[n]`回傳十進制數字從右向左數的第n個數字

- 123456789[0]回傳9
- 123456789[1]回傳8

...等等

```swift
extension Int {
    subscript(digitIndex: Int) -> Int {
        var decimalBase = 1
            for _ in 1...digitIndex {
                decimalBase *= 10
            }
        return (self / decimalBase) % 10
    }
}
746381295[0]
// returns 5
746381295[1]
// returns 9
746381295[2]
// returns 2
746381295[8]
// returns 7
```

如果該`Int`值沒有足夠的位數，即下標越界，那麼上述實作的下標會回傳0，因為它會在數字左邊自動補0：

```swift
746381295[9]
//returns 0, 即等同於：
0746381295[9]
```

<a name="nested_types"></a>
## 嵌套型別（Nested Types）

擴展可以向已有的類別、結構和列舉添加新的嵌套型別：

```swift
extension Character {
    enum Kind {
        case Vowel, Consonant, Other
    }
    var kind: Kind {
        switch String(self).lowercaseString {
        case "a", "e", "i", "o", "u":
            return .Vowel
        case "b", "c", "d", "f", "g", "h", "j", "k", "l", "m",
             "n", "p", "q", "r", "s", "t", "v", "w", "x", "y", "z":
            return .Consonant
        default:
            return .Other
        }
    }
}
```

該範例向`Character`添加了新的嵌套列舉。這個名為`Kind`的列舉表示特定字元的型別。具體來說，就是表示一個標準的拉丁腳本中的字元是母音還是子音（不考慮口語和地方變種），或者是其它型別。

這個類別子還向`Character`添加了一個新的計算實例屬性，即`kind`，用來回傳合適的`Kind`列舉成員。

現在，這個嵌套列舉可以和一個`Character`值聯合使用了：

```swift
func printLetterKinds(word: String) {
    println("'\\(word)' is made up of the following kinds of letters:")
    for character in word {
        switch character.kind {
        case .Vowel:
            print("vowel ")
        case .Consonant:
            print("consonant ")
        case .Other:
            print("other ")
        }
    }
    print("\n")
}
printLetterKinds("Hello")
// 'Hello' is made up of the following kinds of letters:
// consonant vowel consonant consonant vowel
```

函式`printLetterKinds`的輸入是一個`String`值並對其字元進行迭代。在每次迭代過程中，考慮當前字元的`kind`計算屬性，並列印出合適的類別別描述。所以`printLetterKinds`就可以用來列印一個完整單詞中所有字母的型別，正如上述單詞`"hello"`所展示的。

>注意：  
由於已知`character.kind`是`Character.Kind`型，所以`Character.Kind`中的所有成員值都可以使用`switch`語句裡的形式簡寫，比如使用 `.Vowel`代替`Character.Kind.Vowel`

