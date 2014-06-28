> 翻譯：[yankuangshi](https://github.com/yankuangshi)
> 校對：[shinyzhu](https://github.com/shinyzhu)

# 列舉（Enumerations）
---

本頁內容包含：

- [列舉語法（Enumeration Syntax）](#enumeration_syntax)
- [匹配列舉值與`Swith`語句（Matching Enumeration Values with a Switch Statement）](#matching_enumeration_values_with_a_switch_statement)
- [實例值（Associated Values）](#associated_values)
- [原始值（Raw Values）](#raw_values)

列舉定義了一個通用型別的一組相關的值，使你可以在你的程式碼中以一個安全的方式來使用這些值。

如果你熟悉 C 語言，你就會知道，在 C 語言中列舉指定相關名稱為一組整型值。Swift 中的列舉更加靈活，不必給每一個列舉成員提供一個值。如果一個值（被認為是「原始」值）被提供給每個列舉成員，則該值可以是一個字串，一個字元，或是一個整型值或浮點值。

此外，列舉成員可以指定任何型別的實例值儲存到列舉成員值中，就像其他語言中的聯合體（unions）和變體（variants）。你可以定義一組通用的相關成員作為列舉的一部分，每一組都有不同的一組與它相關的適當型別的數值。

在 Swift 中，列舉型別是一等（first-class）型別。它們采用了很多傳統上只被類別（class)所支援的特征，例如計算型屬性（computed properties)，用於提供關於列舉當前值的附加資訊， 實例方法（instance methods），用於提供和列舉所代表的值相關聯的功能。列舉也可以定義建構函式（initializers）來提供一個初始成員值；可以在原始的實作基礎上擴展它們的功能；可以遵守協定（protocols）來提供標準的功能。

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

一個列舉中被定義的值（例如 `North`，`South`，`East`和`West`）是列舉的***成員值***（或者***成員***）。`case`關鍵詞表明新的一行成員值將被定義。

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

`directionToHead`的型別被推斷當它被`CompassPoint`的一個可能值初始化。一旦`directionToHead`被宣告為一個`CompassPoint`，你可以使用更短的點（.）語法將其設置為另一個`CompassPoint`的值：

```swift
directionToHead = .East
```

`directionToHead`的型別已知時，當設定它的值時，你可以不再寫型別名。使用顯示型別的列舉值可以讓程式碼具有更好的可讀性。

<a name="matching_enumeration_values_with_a_switch_statement"></a>
## 匹配列舉值和`Switch`語句

你可以匹配單個列舉值和`switch`語句：

```swift
directionToHead = .South
switch directionToHead {
case .North:
    println("Lots of planets have a north")
case .South:
    println("Watch out for penguins")
case .East:
    println("Where the sun rises")
case .West:
    println("Where the skies are blue")
}
// 輸出 "Watch out for penguins」
```

你可以如此理解這段程式碼：

「考慮`directionToHead`的值。當它等於`.North`，列印`「Lots of planets have a north」`。當它等於`.South`，列印`「Watch out for penguins」`。」

等等依次類別推。

正如在[控制流程（Control Flow）](05_Control_Flow.html)中介紹，當考慮一個列舉的成員們時，一個`switch`語句必須全面。如果忽略了`.West`這種情況，上面那段程式碼將無法通過編譯，因為它沒有考慮到`CompassPoint`的全部成員。全面性的要求確保了列舉成員不會被意外遺漏。

當不需要匹配每個列舉成員的時候，你可以提供一個預設`default`分支來涵蓋所有未明確被提出的任何成員：

```swift
let somePlanet = Planet.Earth
switch somePlanet {
case .Earth:
    println("Mostly harmless")
default:
    println("Not a safe place for humans")
}
// 輸出 "Mostly harmless」
```

<a name="associated_values"></a>
## 實例值（Associated Values）

上一小節的範例演示了一個列舉的成員是如何被定義（分類別）的。你可以為`Planet.Earth`設置一個常數或則變數，並且在之後查看這個值。然而，有時候會很有用如果能夠把其他型別的實例值和成員值一起儲存起來。這能讓你隨著成員值儲存額外的自定義資訊，並且當每次你在程式碼中利用該成員時允許這個資訊產生變化。

你可以定義 Swift 的列舉儲存任何型別的實例值，如果需要的話，每個成員的資料型別可以是各不相同的。列舉的這種特性跟其他語言中的可辨識聯合（discriminated unions），標簽聯合（tagged unions），或者變體（variants）相似。

例如，假設一個函式庫存跟蹤系統需要利用兩種不同型別的條形碼來跟蹤商品。有些商品上標有 UPC-A 格式的一維碼，它使用數字 0 到 9。每一個條形碼都有一個代表「數字系統」的數字，該數字後接 10 個代表「識別符號」的數字。最後一個數字是「檢查」位，用來驗證程式碼是否被正確掃描：

<img width="252" height="120" alt="" src="https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Art/barcode_UPC_2x.png">

其他商品上標有 QR 碼格式的二維碼，它可以使用任何 ISO8859-1 字元，並且可以編碼一個最多擁有 2,953 字元的字串:

<img width="169" height="169" alt="" src="https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Art/barcode_QR_2x.png">

對於函式庫存跟蹤系統來說，能夠把 UPC-A 碼作為三個整型值的元組，和把 QR 碼作為一個任何長度的字串儲存起來是方便的。

在 Swift 中，用來定義兩種商品條碼的列舉是這樣子的：

```swift
enum Barcode {
  case UPCA(Int, Int, Int)
  case QRCode(String)
}
```

以上程式碼可以這麼理解：

「定義一個名為`Barcode`的列舉型別，它可以是`UPCA`的一個實例值（`Int`，`Int`，`Int`），或者`QRCode`的一個字串型別（`String`）實例值。」

這個定義不提供任何`Int`或`String`的實際值，它只是定義了，當`Barcode`常數和變數等於`Barcode.UPCA`或`Barcode.QRCode`時，實例值的型別。

然後可以使用任何一種條碼型別創建新的條碼，如：

```swift
var productBarcode = Barcode.UPCA(8, 85909_51226, 3)
```

以上範例創建了一個名為`productBarcode`的新變數，並且賦給它一個`Barcode.UPCA`的實例元組值`(8, 8590951226, 3)`。提供的「識別符號」值在整數字中有一個底線，使其便於閱讀條形碼。

同一個商品可以被分配給一個不同型別的條形碼，如：

```swift
productBarcode = .QRCode("ABCDEFGHIJKLMNOP")
```

這時，原始的`Barcode.UPCA`和其整數值被新的`Barcode.QRCode`和其字串值所替代。條形碼的常數和變數可以儲存一個`.UPCA`或者一個`.QRCode`（連同它的實例值），但是在任何指定時間只能儲存其中之一。

像以前那樣，不同的條形碼型別可以使用一個 switch 語句來檢查，然而這次實例值可以被提取作為 switch 語句的一部分。你可以在`switch`的 case 分支程式碼中提取每個實例值作為一個常數（用`let`前綴）或者作為一個變數（用`var`前綴）來使用：

```swift
switch productBarcode {
case .UPCA(let numberSystem, let identifier, let check):
    println("UPC-A with value of \(numberSystem), \(identifier), \(check).")
case .QRCode(let productCode):
    println("QR code with value of \(productCode).")
}
// 輸出 "QR code with value of ABCDEFGHIJKLMNOP.」
```

如果一個列舉成員的所有實例值被提取為常數，或者它們全部被提取為變數，為了簡潔，你可以只放置一個`var`或者`let`標注在成員名稱前：

```swift
switch productBarcode {
case let .UPCA(numberSystem, identifier, check):
    println("UPC-A with value of \(numberSystem), \(identifier), \(check).")
case let .QRCode(productCode):
    println("QR code with value of \(productCode).")
}
// 輸出 "QR code with value of ABCDEFGHIJKLMNOP."
```

<a name="raw_values"></a>
## 原始值（Raw Values）

在實例值小節的條形碼範例中演示了一個列舉的成員如何宣告它們儲存不同型別的實例值。作為實例值的替代，列舉成員可以被預設值（稱為原始值）預先填充，其中這些原始值具有相同的型別。

這裡是一個列舉成員儲存原始 ASCII 值的範例：

```swift
enum ASCIIControlCharacter: Character {
    case Tab = "\t"
    case LineFeed = "\n"
    case CarriageReturn = "\r"
}
```

在這裡，稱為`ASCIIControlCharacter`的列舉的原始值型別被定義為字元型`Character`，並被設置了一些比較常見的 ASCII 控制字元。字元值的描述請詳見字串和字元[`Strings and Characters`](03_Strings_and_Characters.html)部分。

注意，原始值和實例值是不相同的。當你開始在你的程式碼中定義列舉的時候原始值是被預先填充的值，像上述三個 ASCII 碼。對於一個特定的列舉成員，它的原始值始終是相同的。實例值是當你在創建一個基於列舉成員的新常數或變數時才會被設置，並且每次當你這麼做得時候，它的值可以是不同的。

原始值可以是字串，字元，或者任何整型值或浮點型值。每個原始值在它的列舉宣告中必須是唯一的。當整型值被用於原始值，如果其他列舉成員沒有值時，它們會自動遞增。

下面的列舉是對之前`Planet`這個列舉的一個細化，利用原始整型值來表示每個 planet 在太陽系中的順序：

```swift
enum Planet: Int {
    case Mercury = 1, Venus, Earth, Mars, Jupiter, Saturn, Uranus, Neptune
}
```

自動遞增意味著`Planet.Venus`的原始值是`2`，依次類別推。

使用列舉成員的`toRaw`方法可以存取該列舉成員的原始值：

```swift
let earthsOrder = Planet.Earth.toRaw()
// earthsOrder is 3
```

使用列舉的`fromRaw`方法來試圖找到具有特定原始值的列舉成員。這個範例通過原始值`7`識別`Uranus`：

```swift
let possiblePlanet = Planet.fromRaw(7)
// possiblePlanet is of type Planet? and equals Planet.Uranus
```

然而，並非所有可能的`Int`值都可以找到一個匹配的行星。正因為如此，`fromRaw`方法可以回傳一個***可選***的列舉成員。在上面的範例中，`possiblePlanet`是`Planet?`型別，或「可選的`Planet`」。

如果你試圖尋找一個位置為9的行星，通過`fromRaw`回傳的可選`Planet`值將是`nil`：

```swift
let positionToFind = 9
if let somePlanet = Planet.fromRaw(positionToFind) {
    switch somePlanet {
    case .Earth:
        println("Mostly harmless")
    default:
        println("Not a safe place for humans")
    }
} else {
    println("There isn't a planet at position \(positionToFind)")
}
// 輸出 "There isn't a planet at position 9
```

這個範例使用可選綁定（optional binding），通過原始值`9`試圖存取一個行星。`if let somePlanet = Planet.fromRaw(9)`語句獲得一個可選`Planet`，如果可選`Planet`可以被獲得，把`somePlanet`設置成該可選`Planet`的內容。在這個範例中，無法檢索到位置為`9`的行星，所以`else`分支被執行。

