> 翻譯：[tommy60703](https://github.com/tommy60703)

# 字串和字元（Strings and Characters）
-----------------

本頁包含內容：

- [字串字面量](#string_literals)
- [初始化空字串](#initializing_an_empty_string)
- [字串可變性](#string_mutability)
- [字串是實值型別](#strings_are_value_types)
- [使用字元](#working_with_characters)
- [計算字元數量](#counting_characters)
- [連接字串和字元](#concatenating_strings_and_characters)
- [字串插值](#string_interpolation)
- [比較字串](#comparing_strings)
- [字串大小寫](#uppercase_and_lowercase_strings)
- [Unicode](#unicode)

`String`是例如 "hello, world"、"albatross" 這樣的有序的`Character`（字元）型別的值的集合，透過`String`型別來表示。

Swift 的`String`和`Character`型別提供了一個快速的、相容 Unicode 的方式來處理程式碼中的文字訊息。
創建和操作字串的語法與 C 語言中字串操作相似，輕量並且易讀。
字串連接操作只需要簡單地透過`+`號將兩個字串相連即可。
與 Swift 中其他值一樣，能否更改字串的值，取決於其被定義為常數還是變數。

盡管語法簡單，但`String`型別是一種快速、現代化的字串實作。
每一個字串都是由獨立編碼的 Unicode 字元組成，並支援以不同 Unicode 表示（representations）來存取這些字元。

Swift 可以在常數、變數、字面量和表達式中進行字串插值操作，可以輕鬆創建用於顯示、儲存和列印的自定義字串。

> 注意：  
Swift 的`String`型別與 Foundation `NSString`類進行了無縫橋接。如果你利用 Cocoa 或 Cocoa Touch 中的 Foundation Framework 進行工作。所有`NSString` API 都可以呼叫你創建的任意`String`型別的值。除此之外，還可以使用本章介紹的`String`特性。你也可以在任意要求傳入`NSString`實體作為參數的 API 中使用`String`型別的值來替代。
>更多關於在 Foundation 和 Cocoa 中使用`String`的資訊請查看 [Using Swift with Cocoa and Objective-C](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/BuildingCocoaApps/index.html#//apple_ref/doc/uid/TP40014216)。  

<a name="string_literals"></a>
## 字串字面量（String Literals）

你可以在你的程式碼中包含一段預先定義的字串值作為字串字面量。
字串字面量是由雙引號 ("") 包著的具有固定順序的文字字元集。

字串字面量可以用於為常數和變數提供初始值。

```swift
let someString = "Some string literal value"
```

> 注意：  
`someString`變數透過字串字面量進行初始化，Swift 因此推斷該變數為`String`型別。

字串字面量可以包含以下特殊字元：

* 跳脫字元`\0`(空字元)、`\\`(反斜線)、`\t`(水平 tab)、`\n`(換行)、`\r`(回車)、`\"`(雙引號)、`\'`(單引號)。
* 單位元組 Unicode 純量，寫成`\xnn`，其中`nn`為兩位十六進制數。
* 雙位元組 Unicode 純量，寫成`\unnnn`，其中`nnnn`為四位十六進制數。
* 四位元組 Unicode 純量，寫成`\Unnnnnnnn`，其中`nnnnnnnn`為八位十六進制數。

下面的程式碼為各種特殊字元的使用範例。
`wiseWords`常數包含了兩個跳脫字元 (雙括號)；
`dollarSign`、`blackHeart`和`sparklingHeart`常數展示了三種不同格式的 Unicode 純量：

```swift
let wiseWords = "\"Imagination is more important than knowledge\" - Einstein
// "\"Imagination is more important than knowledge\" - Einstein
let dollarSign = "\x24"             // $,  Unicode 純量 U+0024
let blackHeart = "\u2665"           // ♥,  Unicode 純量 U+2665
let sparklingHeart = "\U0001F496"  // 💖, Unicode 純量 U+1F496
```

<a name="initializing_an_empty_string"></a>
## 初始化空字串 (Initializing an Empty String)

為了構造一個很長的字串，可以創建一個空字串作為初始值。
可以將空的字串字面量指派給變數，也可以初始化一個新的`String`實體：

```swift
var emptyString = ""               // 空字串字面量
var anotherEmptyString = String()  // 初始化 String 實體
// 兩個字串均為空並等價。
```

你可以透過檢查其`Boolean`型別的`isEmpty`屬性來判斷該字串是否為空：

```swift
if emptyString.isEmpty {
    println("Nothing to see here")
}
// prints "Nothing to see here"
```

<a name="string_mutability"></a>
## 字串可變性 (String Mutability)

你可以透過將一個特定字串分配給一個變數來對其進行修改，或者分配給一個常數來保證其不會被修改：

```swift
var variableString = "Horse"
variableString += " and carriage"
// variableString 現在為 "Horse and carriage"
let constantString = "Highlander"
constantString += " and another Highlander"
// 這會回報一個編譯錯誤 (compile-time error) - 常數不可以被修改。
```

> 注意：  
在 Objective-C 和 Cocoa 中，你透過選擇兩個不同的類別(`NSString`和`NSMutableString`)來指定該字串是否可以被修改，Swift 中的字串是否可以修改僅透過定義的是變數還是常數來決定。

<a name="strings_are_value_types"></a>
## 字串是實值型別（Strings Are Value Types）

Swift 的`String`型別是實值型別。
如果你創建了一個新的字串，那麼當其進行常數、變數指派操作或傳遞給函式/方法時，字串值是被複製一份過去的。
任何情況下，都會對現存字串的值創建新副本，並對該新副本進行傳遞或指派操作。
實值型別在 [結構和列舉是實值型別](09_Classes_and_Structures.html#structures_and_enumerations_are_value_types) 中進行了說明。

> 注意：  
與 Cocoa 中的`NSString`不同，當你在 Cocoa 中創建了一個`NSString`實體，並將其傳遞給一個函式/方法，或者指派給一個變數，你傳遞或指派的是該`NSString`實體的參考，除非你特別要求，否則字串不會生成新的副本來進行指派操作。

Swift 預設字串複製的方式保證了在函式/方法中傳遞的是字串的值。
很明顯無論該值來自於哪裡，都是你獨自擁有的。
你傳遞的字串本身不會被更改，除非你主動更改它。

在實際編譯時，Swift 編譯器會最佳化字串的使用，使實際的複製只發生在絕對必要的情況下，這意味著你將字串作為實值型別的同時可以獲得極高的效能。

<a name="working_with_characters"></a>
## 使用字元（Working with Characters）

Swift 的`String`型別表示特定序列的`Character`（字元） 型別的值的集合。
每一個字元值代表一個 Unicode 字元。
你可利用`for-in`迴圈來遍歷字串中的每一個字元：

```swift
for character in "Dog!🐶" {
    println(character)
}
// D
// o
// g
// !
// 🐶
```

for-in 迴圈在 [For 迴圈](05_Control_Flow.html#for_loops) 中進行了詳細介紹。

另外，透過標明一個`Character`型別註解並透過字元字面量進行指派，可以建立一個獨立的字元常數或變數：

```swift
let yenSign: Character = "¥"
```

<a name="counting_characters"></a>
## 計算字元數量 (Counting Characters)

透過呼叫全域`countElements`函式，並將字串作為參數傳入，可以獲取該字串的字元數量。

```swift
let unusualMenagerie = "Koala 🐨, Snail 🐌, Penguin 🐧, Dromedary 🐪"
println("unusualMenagerie has \(countElements(unusualMenagerie)) characters")
// prints "unusualMenagerie has 40 characters"
```

> 注意：  
不同的 Unicode 字元以及相同 Unicode 字元的不同表示方式可能需要不同數量的記憶體空間來儲存。所以 Swift 中的字元在一個字串中並不一定占用相同的記憶體空間。因此字串的長度不得不透過遍歷字串中每一個字元的長度來進行計算。如果你正在處理一個長字串，需要注意`countElements`函式必須遍歷字串中的字元以精準計算字串的長度。

> 另外需要注意的是透過`countElements`回傳的字元數量並不總是與包含相同字元的`NSString`的`length`屬性相同。`NSString`的`length`屬性是基於利用 UTF-16 表示的十六位程式碼單元數字，而不是基於 Unicode 字元。為了解決這個問題，`NSString`的`length`屬性在被 Swift 的`String`存取時會成為`utf16count`。  

<a name="concatenating_strings_and_characters"></a>
## 連接字串和字元 (Concatenating Strings and Characters)

字串和字元的值可以透過加法運算子（`+`）相加在一起並創建一個新的字串值：

```swift
let string1 = "hello"
let string2 = " there"
let character1: Character = "!"
let character2: Character = "?"

let stringPlusCharacter = string1 + character1        // 等於 "hello!"
let stringPlusString = string1 + string2              // 等於 "hello there"
let characterPlusString = character1 + string1        // 等於 "!hello"
let characterPlusCharacter = character1 + character2  // 等於 "!?"
```

你也可以透過加法指派運算子 (`+=`) 將一個字串或者字元添加到一個已經存在字串變數上：

```swift
var instruction = "look over"
instruction += string2
// instruction 現在等於 "look over there"

var welcome = "good morning"
welcome += character1
// welcome 現在等於 "good morning!"
```

> 注意：  
你不能將一個字串或者字元添加到一個已經存在的字元變數上，因為字元變數只能包含一個字元。

<a name="string_interpolation"></a>
## 字串插值 (String Interpolation)

字串插值是一種構建新字串的方式，可以在其中包含常數、變數、字面量和表達式。
你插入的字串字面量的每一項都被包裹在以反斜線為前綴的圓括號中：

```swift
let multiplier = 3
let message = "\(multiplier) 乘以 2.5 是 \(Double(multiplier) * 2.5)"
// message 是 "3 乘以 2.5 是 7.5"
```

在上面的例子中，`multiplier`作為`\(multiplier)`被插入到一個字串字面量中。
當創建字串執行插值計算時此占位符（placeholder）會被替換為`multiplier`實際的值。

`multiplier`的值也作為字串中後面表達式的一部分。
該表達式計算`Double(multiplier) * 2.5`的值並將結果 (7.5) 插入到字串中。
在這個例子中，表達式寫為`\(Double(multiplier) * 2.5)`並包含在字串字面量中。

> 注意：  
插值字串中寫在括號中的表達式不能包含非跳脫雙引號 (`"`) 和反斜杠 (`\`)，並且不能包含回車 (`\r`)或換行 (`\n`) 符號。

<a name="comparing_strings"></a>
## 比較字串 (Comparing Strings)

Swift 提供了三種方式來比較字串的值：字串相等、前綴相等和後綴相等。

<a name="string_equality"></a>
### 字串相等 (String Equality)

如果兩個字串以同一順序包含完全相同的字元，則認為兩者字串相等：

```swift
let quotation = "We're a lot alike, you and I."
let sameQuotation = "We're a lot alike, you and I."
if quotation == sameQuotation {
    println("These two strings are considered equal")
}
// prints "These two strings are considered equal"
```

<a name="prefix_and_suffix_equality"></a>
### 前綴/後綴相等 (Prefix and Suffix Equality)

透過呼叫字串的`hasPrefix`/`hasSuffix`方法來檢查字串是否擁有特定前綴/後綴。
兩個方法均需要以字串作為參數傳入並回傳布林值。
兩個方法均執行基本字串和前綴/後綴字串之間逐個字元的比較。

下面的例子以一個字串陣列表示莎士比亞話劇《羅密歐與朱麗葉》中前兩場的場景位置：

```swift
let romeoAndJuliet = [
    "Act 1 Scene 1: Verona, A public place",
    "Act 1 Scene 2: Capulet's mansion",
    "Act 1 Scene 3: A room in Capulet's mansion",
    "Act 1 Scene 4: A street outside Capulet's mansion",
    "Act 1 Scene 5: The Great Hall in Capulet's mansion",
    "Act 2 Scene 1: Outside Capulet's mansion",
    "Act 2 Scene 2: Capulet's orchard",
    "Act 2 Scene 3: Outside Friar Lawrence's cell",
    "Act 2 Scene 4: A street in Verona",
    "Act 2 Scene 5: Capulet's mansion",
    "Act 2 Scene 6: Friar Lawrence's cell"
]
```

你可以利用`hasPrefix`方法來計算話劇中第一幕的場景數：

```swift
var act1SceneCount = 0
for scene in romeoAndJuliet {
    if scene.hasPrefix("Act 1 ") {
        ++act1SceneCount
    }
}
println("There are \(act1SceneCount) scenes in Act 1")
// prints "There are 5 scenes in Act 1"
```

類似地，你可以用`hasSuffix`方法來計算發生在不同地方的場景數：

```swift
var mansionCount = 0
var cellCount = 0
for scene in romeoAndJuliet {
    if scene.hasSuffix("Capulet's mansion") {
        ++mansionCount
    } else if scene.hasSuffix("Friar Lawrence's cell") {
        ++cellCount
    }
}
println("\(mansionCount) mansion scenes; \(cellCount) cell scenes")
// prints "6 mansion scenes; 2 cell scenes”
```

<a name="uppercase_and_lowercase_strings"></a>
### 大寫和小寫字串（Uppercase and Lowercase Strings）

你可以透過字串的`uppercaseString`和`lowercaseString`屬性來存取大寫/小寫版本的字串。

```swift
let normal = "Could you help me, please?"
let shouty = normal.uppercaseString
// shouty 值為 "COULD YOU HELP ME, PLEASE?"
let whispered = normal.lowercaseString
// whispered 值為 "could you help me, please?"
```

<a name="unicode"></a>
## Unicode

Unicode 是一個國際標準，用於文字的編碼和表示。
它使你可以用標準格式表示來自任意語言幾乎所有的字元，並能夠對文字文件或網頁這樣的外部資源中的字元進行讀寫。

Swift 的字串和字元型別是完全相容 Unicode 標準的，它支援如下所述的一系列不同的 Unicode 編碼。

<a name="unicode_terminology"></a>
### Unicode 術語（Unicode Terminology）

Unicode 中每一個字元都可以被解釋為一個或多個 unicode 純量。
字元的 unicode 純量是一個唯一的 21 位元數字 (和名稱)，例如`U+0061`表示小寫的拉丁字母 A ("a")，`U+1F425`表示小雞表情 ("🐥")。

當 Unicode 字串被寫進程式本文檔或其他儲存結構當中，這些 unicode 純量將會按照 Unicode 定義的格式之一進行編碼。其包括`UTF-8`（以 8 位元程式碼單元進行編碼） 和`UTF-16`（以 16 位元程式碼單元進行編碼）。

<a name="unicode_representations_of_strings"></a>
### 字串的 Unicode 表示（Unicode Representations of Strings）

Swift 提供了幾種不同的方式來存取字串的 Unicode 表示。

你可以利用`for-in`來對字串進行遍歷，從而以 Unicode 字元的方式存取每一個字元值。
該過程在 [使用字元](#working_with_characters) 中進行了介紹。

另外，能夠以其他三種 Unicode 相容的方式存取字串的值：

* UTF-8 程式碼單元集合 (利用字串的`utf8`屬性進行存取)
* UTF-16 程式碼單元集合 (利用字串的`utf16`屬性進行存取)
* 21 位元的 Unicode 純量值集合 (利用字串的`unicodeScalars`屬性進行存取)

下面由`D``o``g``!`和`🐶`(`DOG FACE`，Unicode 純量為`U+1F436`)組成的字串中的每一個字元代表著一種不同的表示：

```swift
let dogString = "Dog!🐶"
```

<a name="UTF-8"></a>
### UTF-8

你可以透過遍歷字串的`utf8`屬性來存取它的`UTF-8`表示。
其為`UTF8View`型別的屬性，`UTF8View`是無號 8 位元 (`UInt8`) 值的集合，每一個`UInt8`值都是一個字元的 UTF-8 表示：

```swift
for codeUnit in dogString.utf8 {
    print("\(codeUnit) ")
}
print("\n")
// 68 111 103 33 240 159 144 182
```

上面的例子中，前四個 10 進制程式碼單元值 (68, 111, 103, 33) 代表了字元`D` `o` `g`和`!`，它們的 UTF-8 表示與 ASCII 表示相同。
後四個程式碼單元值 (240, 159, 144, 182) 是`DOG FACE`的 4 位元組 UTF-8 表示。

<a name="UTF-16"></a>
### UTF-16

你可以透過遍歷字串的`utf16`屬性來存取它的`UTF-16`表示。
其為`UTF16View`型別的屬性，`UTF16View`是無號 16 位元 (`UInt16`) 值的集合，每一個`UInt16`都是一個字元的 UTF-16 表示：

```swift
for codeUnit in dogString.utf16 {
    print("\(codeUnit) ")
}
print("\n")
// 68 111 103 33 55357 56374
```

同樣，前四個程式碼單元值 (68, 111, 103, 33) 代表了字元`D` `o` `g`和`!`，它們的 UTF-16 程式碼單元和 UTF-8 完全相同。

第五和第六個程式碼單元值 (55357 和 56374) 是`DOG FACE`字元的 UTF-16 表示。
第一個值為`U+D83D`(十進制值為 55357)，第二個值為`U+DC36`(十進制值為 56374)。

<a name="unicode_scalars"></a>
### Unicode 純量 (Unicode Scalars)

你可以透過遍歷字串的`unicodeScalars`屬性來存取它的 Unicode 純量表示。
其為`UnicodeScalarView`型別的屬性， `UnicodeScalarView`是`UnicodeScalar`的集合。
`UnicodeScalar`是 21 位元的 Unicode 程式碼點。

每一個`UnicodeScalar`擁有一個值屬性，可以回傳對應的 21 位元數值，用`UInt32`來表示。

```swift
for scalar in dogString.unicodeScalars {
    print("\(scalar.value) ")
}
print("\n")
// 68 111 103 33 128054
```

同樣，前四個程式碼單元值 (68, 111, 103, 33) 代表了字元`D` `o` `g`和`!`。
第五位數值，128054，是一個十六進制 1F436 的十進制表示。
其等同於`DOG FACE`的 Unicode 純量 U+1F436。

作為查詢字元值屬性的一種替代方法，每個`UnicodeScalar`值也可以用來構建一個新的字串值，比如在字串插值中使用：

```swift
for scalar in dogString.unicodeScalars {
    println("\(scalar) ")
}
// D
// o
// g
// !
// 🐶
```
