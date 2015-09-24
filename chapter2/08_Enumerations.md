> 翻譯：[tommy60703](https://github.com/tommy60703)


# 列舉（Enumerations）
---

本頁內容包含：

- [列舉語法（Enumeration Syntax）](#enumeration_syntax)
- [匹配列舉值與 Swith 語句（Matching Enumeration Values with a Switch Statement）](#matching_enumeration_values_with_a_switch_statement)
- [實例值（Associated Values）](#associated_values)
- [原始值（Raw Values）](#raw_values)
- [遞迴列舉（Recursive Enumerations）](#revursive_enumerations)

列舉定義了一個通用型別的一組相關的值，使你可以在你的程式碼中以一個安全的方式來使用這些值。

如果你熟悉 C 語言，你就會知道，在 C 語言中列舉指定相關名稱為一組整型值。Swift 中的列舉更加靈活，不必給每一個列舉成員提供一個值。如果一個值（被認為是「原始」值）被提供給每個列舉成員，則該值可以是一個字串，一個字元，或是一個整型值或浮點值。

此外，列舉成員可以指定任何型別的實例值儲存到列舉成員值中，就像其他語言中的聯合體（unions）和變體（variants）。你可以定義一組通用的相關成員作為列舉的一部分，每一組都有不同的一組與它相關的適當型別的數值。

在 Swift 中，列舉型別是第一等型別（first-class）。它們采用了很多傳統上只被類別（class)所支援的特征，例如計算型屬性（computed properties)，用於提供關於列舉當前值的附加資訊， 實例方法（instance methods），用於提供和列舉所代表的值相關聯的功能。列舉也可以定義建構函式（initializers）來提供一個初始成員值；可以在原始的實作基礎上擴展它們的功能；可以遵守協定（protocols）來提供標準的功能。

欲了解更多相關功能，請參見[屬性（Properties）](10_Properties.html)，[方法（Methods）](11_Methods.html)，[建構過程（Initialization）](14_Initialization.html)，[擴展（Extensions）](20_Extensions.html)和[協定（Protocols）](21_Protocols.html)。

<a name="enumeration_syntax"></a>
## 列舉語法

使用`enum`關鍵詞並且把它們的整個定義放在一對大括號內：

```swift
enum SomeEnumeration {
  // enumeration definition goes here
}
```

以下是指南針四個方向的一個範例：

```swift
enum CompassPoint {
  case North
  case South
  case East
  case West
}
```

一個列舉中被定義的值（例如 `North`，`South`，`East`和`West`）是列舉的_成員值_（或者_成員_）。`case`關鍵詞表明新的一行成員值將被定義。

> 注意：  
> 不像 C 和 Objective-C 一樣，Swift 的列舉成員在被創建時不會被賦予一個預設的整數值。在上面的`CompassPoints`範例中，`North`，`South`，`East`和`West`不是隱式的等於`0`，`1`，`2`和`3`。相反的，這些不同的列舉成員在`CompassPoint`的一種顯示定義中擁有各自不同的值。  

多個成員值可以出現在同一行上，用逗號隔開：

```swift
enum Planet {
  case Mercury, Venus, Earth, Mars, Jupiter, Saturn, Uranus, Nepturn
}
```

每個列舉定義了一個全新的型別。像 Swift 中其他型別一樣，它們的名字（例如`CompassPoint`和`Planet`）必須以一個大寫字母開頭。給列舉型別起一個單數名字而不是復數名字，以便於讀起來更加容易理解：

```swift
var directionToHead = CompassPoint.West
```

`directionToHead`的型別在它被`CompassPoint`的一個可能值初始化時會被推斷。一旦`directionToHead`被宣告為一個`CompassPoint`，你可以使用更短的點語法將其設置為另一個`CompassPoint`的值：

```swift
directionToHead = .East
```

`directionToHead`的型別已知時，當設定它的值時，你可以不再寫型別名。使用顯式型別的列舉值可以讓程式碼具有更好的可讀性。

<a name="matching_enumeration_values_with_a_switch_statement"></a>
## 匹配列舉值和 Switch 語句

你可以匹配單個列舉值和`switch`語句：

```swift
directionToHead = .South
switch directionToHead {
case .North:
    print("Lots of planets have a north")
case .South:
    print("Watch out for penguins")
case .East:
    print("Where the sun rises")
case .West:
    print("Where the skies are blue")
}
// prints "Watch out for penguins"
```

你可以如此理解這段程式碼：

「考慮`directionToHead`的值。當它等於`.North`，列印`Lots of planets have a north`。當它等於`.South`，列印`Watch out for penguins`。」

...依次類推。

正如在[控制流程（Control Flow）](05_Control_Flow.html)中介紹，當考慮一個列舉的成員們時，一個`switch`語句必須全面。如果忽略了`.West`這種情況，上面那段程式碼將無法通過編譯，因為它沒有考慮到`CompassPoint`的全部成員。全面性的要求確保了列舉成員不會被意外遺漏。

當不需要匹配每個列舉成員的時候，你可以提供一個預設`default`分支來涵蓋所有未明確被提出的任何成員：

```swift
let somePlanet = Planet.Earth
switch somePlanet {
case .Earth:
    print("Mostly harmless")
default:
    print("Not a safe place for humans")
}
// prints "Mostly harmless"
```

<a name="associated_values"></a>
## 關聯值（Associated Values）

上一小節的範例演示了一個列舉的成員是如何被定義（分類別）的。你可以為`Planet.Earth`設置一個常數或變數，並且在之後查看這個值。然而，有時候會很有用如果能夠把其他型別的實例值和成員值一起儲存起來。這能讓你隨著成員值儲存額外的自定義資訊，並且當每次你在程式碼中利用該成員時允許這個資訊產生變化。

你可以定義 Swift 的列舉儲存任何型別的實例值，如果需要的話，每個成員的資料型別可以是各不相同的。列舉的這種特性跟其他語言中的可辨識聯合（discriminated unions），標籤聯合（tagged unions），或者變體（variants）相似。

例如，假設一個庫存追蹤系統需要利用兩種不同型別的條形碼來追蹤商品。有些商品上標有 UPC-A 格式的一維條碼，它使用數字 0 到 9。每一個條碼都有一個代表「數字系統」的數字，該數字後接 5 個「製造商識別碼」和 5 個「商品識別碼」的數字。最後一個數字是「檢查碼」，用來驗證程式碼是否被正確掃描：

<img width="252" height="120" alt="" src="https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Art/barcode_UPC_2x.png">

其他商品上標有 QR 碼格式的二維碼，它可以使用任何 ISO8859-1 字元，並且可以編碼一個最多擁有 2,953 字元的字串:

<img width="169" height="169" alt="" src="https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Art/barcode_QR_2x.png">

對於庫存追蹤系統來說，能夠把 UPC-A 碼作為四個整型值的 tuple，和把 QR 碼作為一個任何長度的字串儲存起來是方便的。

在 Swift 中，用來定義兩種商品條碼的列舉是這樣子的：

```swift
enum Barcode {
  case UPCA(Int, Int, Int, Int)
  case QRCode(String)
}
```

以上程式碼可以這麼理解：

「定義一個名為`Barcode`的列舉型別，它可以是`UPCA`的一個實例值（`Int`, `Int`, `Int`, `Int`），或者`QRCode`的一個字串型別（`String`）實例值。」

這個定義不提供任何`Int`或`String`的實際值，它只是定義了，當`Barcode`常數和變數等於`Barcode.UPCA`或`Barcode.QRCode`時，實例值的型別。

然後可以使用任何一種條碼型別創建新的條碼，如：

```swift
var productBarcode = Barcode.UPCA(8, 85909, 51226, 3)
```

以上範例創建了一個名為`productBarcode`的新變數，並且賦給它一個`Barcode.UPCA`的 tuple `(8, 85909, 51226, 3)`。

同一個商品可以被分配給一個不同型別的條形碼，如：

```swift
productBarcode = .QRCode("ABCDEFGHIJKLMNOP")
```

這時，原始的`Barcode.UPCA`和其整數值被新的`Barcode.QRCode`和其字串值所替代。條碼的常數和變數可以儲存一個`.UPCA`或者一個`.QRCode`（連同它的關聯值），但是在任何指定時間只能儲存其中之一。

像以前那樣，不同的條碼型別可以使用一個 switch 語句來檢查，然而這次關聯值可以被提取作為 switch 語句的一部分。你可以在`switch`的 case 分支程式碼中提取每個關聯值作為一個常數（用`let`前綴）或者作為一個變數（用`var`前綴）來使用：

```swift
switch productBarcode {
case .UPCA(let numberSystem, let manufacturer, let product, let check):
    print("UPC-A with value of \(numberSystem), \(manufacturer), \(product), \(check).")
case .QRCode(let productCode):
    print("QR code with value of \(productCode).")
}
// prints "QR code with value of ABCDEFGHIJKLMNOP."
```

如果一個列舉成員的所有關聯值被提取為常數，或者它們全部被提取為變數，為了簡潔，你可以只放置一個`var`或者`let`標注在成員名稱前：

```swift
switch productBarcode {
case let .UPCA(numberSystem, manufacturer, product, check):
    print("UPC-A with value of \(numberSystem), \(manufacturer), \(product), \(check).")
case let .QRCode(productCode):
    print("QR code with value of \(productCode).")
}
// 輸出 "QR code with value of ABCDEFGHIJKLMNOP."
```

<a name="raw_values"></a>
## 原始值 (Raw Values)

在上個小節的條碼範例中展示了一個列舉的成員如何宣告它們儲存不同型別的關聯值。作為關聯值的替代，列舉成員可以被預設值（稱為原始值）預先填充，其中這些原始值具有相同的型別。

這裡是一個列舉成員儲存原始 ASCII 值的範例：

```swift
enum ASCIIControlCharacter: Character {
    case Tab = "\t"
    case LineFeed = "\n"
    case CarriageReturn = "\r"
}
```

在這裡，稱為`ASCIIControlCharacter`的列舉的原始值型別被定義為字元`Character`，並被設置了一些比較常見的 ASCII 控制字元。字元值的描述請詳見字串和字元[`Strings and Characters`](03_Strings_and_Characters.html)部分。

原始值的可以是字串、字元或是任何的整數或浮點數型別，列舉中定義的每一個原始值必須是唯一的。

> 注意
> 原始值和關聯值是_不相同的_。當你開始在你的程式碼中定義列舉的時候原始值是被預先填充的值，像上述三個 ASCII 碼，對於一個特定的列舉成員，它的原始值始終是相同的；關聯值是當你在創建一個基於列舉成員的新常數或變數時才會被設置，並且每次當你這麼做得時候，它的值可以是不同的。

### 原始值的隱式賦值 (Implicitly Assigned Raw Values)

在使用原始值為整數或是字串類型的列舉時，不需要替每一個成員賦值，Swift 會自動幫你完成。

例如，當使用整數為原始值時，隱式賦值的值會依序遞增1，如過第一個值沒有初始值，會自動被設為`0`。

下面的列舉是對之前`Planet`這個列舉的一個細化，利用原始整型值來表示每個 planet 在太陽系中的順序：

```swift
enum Planet: Int {
    case Mercury = 1, Venus, Earth, Mars, Jupiter, Saturn, Uranus, Neptune
}
```

自動遞增意味著`Planet.Venus`的原始值是`2`，依次類別推。


當使用字串作為原始值時，每個成員的隱式初始值則為該成員的名稱。

下面是`CompassPoint`的精簡版，使用字串為初始值類型：

```swift
enum CompassPoint: String {
	case North, South, East, West
}
```

上面的例子中，`CompassPoint.South`的原始值是`South`，依此類推。

使用列舉成員的`rawValue`屬性可以存取該列舉成員的原始值：

```swift
let earthsOrder = Planet.Earth.awValue
// earthsOrder is 3

let sunsetDirection = CompassPoint.West.rawValue
// sunsetDirection is "West"
```

### 使用原始值初始化 (Initializing from a Raw Value)

如果你在定義列舉時使用原始值，那麼你會自動得到一個建構式(initializer)，這個方法將原始值類型做為參數，回傳列舉的成員或是`nil`。你可以使用這個建構式來建立一個新的列舉變數。

這個範例使用原始值`7`來建立列舉：

```swift
let possiblePlanet = Planet(rawValue: 7)
// possiblePlanet is of type Planet? and equals Planet.Uranus
```

然而，並非所有可能的`Int`值都可以找到一個匹配的行星。正因為如此，建構式回傳的是一個的 _optional_ 的列舉成員。在上面的範例中，`possiblePlanet`是`Planet?`型別。

> 注意
> 初始值建構式是一個可失敗的建構式(failable initializer)，因為並不是每個原始值都可以對應到一個列舉成員。更多資訊請參考[可失敗建構式](../chapter03/05_Declarations.md)

如果你試圖尋找一個位置為`9`的行星，初始值建構式會回傳`nil`：

```swift
let positionToFind = 9
if let somePlanet = Planet(rawValue: positionToFind) {
    switch somePlanet {
    case .Earth:
        println("Mostly harmless")
    default:
        println("Not a safe place for humans")
    }
} else {
    print("There isn't a planet at position \(positionToFind)")
}
// prints "There isn't a planet at position 9"
```

這個範例使用 optional binding，透過原始值`9`試圖存取一個行星。`if let somePlanet = Planet(rawValue: 9)`表達式獲得一個 optional `Planet`，如果 optional `Planet`可以被獲得，把`somePlanet`設置成該可選`Planet`的內容。在這個範例中，無法檢索到位置為`9`的行星，所以`else`分支被執行。

<a name="recursive_enumerations"></a>
## 遞迴列舉 (Recursive Enumerations)

在對資料建立模型的時候，如果需要考慮的情況是固定的，例如對整數的數學運算，使用列舉類型可以有很好的效果。
這些運算可以將兩個由數字組成的算數表達式連接起來，例如，將`5`連接到一些更複雜的表達式`5 + 4`.

算數表達式的一個重要特性是，表達式可以嵌套使用。例如，表達式`(5 + 4) * 2`乘號右邊是一個數字，左邊則是另一個表達式。因為資料是嵌套的，因而用來存儲資料的列舉類型也許要支援這種嵌套————這表示列舉類型需要支援遞迴。

遞迴列舉（recursive enumeration）是一種列舉類型，表示它的列舉中，有一個或多個列舉成員擁有該列舉的其他成員作為關聯值。使用遞迴列舉時，編譯器會插入一個中間層。你可以在列舉成員前加上`indirect`來表示這成員可遞迴。

例如，下面的例子中，列舉類型儲存了簡單的算數表達式：

```swift
enum ArithmeticExpression {
    case Number(Int)
    indirect case Addition(ArithmeticExpression, ArithmeticExpression)
    indirect case Multiplication(ArithmeticExpression, ArithmeticExpression)
}
```

你也可以在列舉類型開頭加上`indirect`關鍵字來表示它的所有成員都是可遞迴的：

```swift
indirect enum ArithmeticExpression {
    case Number(Int)
    case Addition(ArithmeticExpression, ArithmeticExpression)
    case Multiplication(ArithmeticExpression, ArithmeticExpression)
}
```

上面定義的列舉類型可以儲存三種算數表達式：純數字、兩個表達式的相加、兩個表達式相乘。`Addition`和` Multiplication`成員的關聯值也是算數表達式——這些關聯值讓我們可以使用遞迴表達式。

遞迴函數可以很直覺地使用具有遞迴性質的資料結構。例如，下面是一個計算算數表達式的函數：

```swift
func evaluate(expression: ArithmeticExpression) -> Int {
    switch expression {
    case .Number(let value):
        return value
    case .Addition(let left, let right):
        return evaluate(left) + evaluate(right)
    case .Multiplication(let left, let right):
        return evaluate(left) * evaluate(right)
    }
}

// 計算 (5 + 4) * 2
let five = ArithmeticExpression.Number(5)
let four = ArithmeticExpression.Number(4)
let sum = ArithmeticExpression.Addition(five, four)
let product = ArithmeticExpression.Multiplication(sum, ArithmeticExpression.Number(2))
print(evaluate(product))
// 輸出 "18"
```

這個函數如果遇到純數字，就直接回傳該數字的值。如果遇到的是加法或乘法運算，則分別計算左邊表達式和右邊表達式的值，然後相加或相乘。
