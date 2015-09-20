> 翻譯：[tommy60703](https://github.com/tommy60703)

# 字串和字元（Strings and Characters）
-----------------

本頁包含內容：

- [字串字面量](#string_literals)
- [初始化空字串](#initializing_an_empty_string)
- [字串可變性](#string_mutability)
- [字串是實值型別](#strings_are_value_types)
- [使用字元](#working_with_characters)
- [連接字串和字元](#concatenating_strings_and_characters)
- [字串插值](#string_interpolation)
- [Unicode](#unicode)
- [計算字元數量](#counting_characters)
- [存取和修改字串](#accesing_and_modifying_a_string)
- [比較字串](#comparing_strings)
- [字串的Unicode表示法](#)


`String`是例如 "hello, world"、"albatross" 這樣的有序的`Character`（字元）型別的值的集合，透過`String`型別來表示。

Swift 的`String`和`Character`型別提供了一個快速的、相容 Unicode 的方式來處理程式碼中的文字訊息。
創建和操作字串的語法與 C 語言中字串操作相似，輕量並且易讀。
字串連接操作只需要簡單地透過`+`號將兩個字串相連即可。
與 Swift 中其他值一樣，能否更改字串的值，取決於其被定義為常數還是變數。

儘管語法簡單，但`String`型別是一種快速、現代化的字串實作。
每一個字串都是由獨立編碼的 Unicode 字元組成，並支援以不同 Unicode 表示（representations）來存取這些字元。

Swift 可以在常數、變數、字面量和表達式中進行字串插值操作，可以輕鬆創建用於顯示、儲存和列印的自定義字串。

> 注意：
Swift 的`String`型別與 Foundation `NSString`類別進行了無縫接軌。如果你利用 Cocoa 或 Cocoa Touch 中的 Foundation Framework 進行工作，所有`NSString` API 都可以呼叫你創建的任意`String`型別的值。除此之外，還可以使用本章介紹的`String`特性。你也可以在任意要求傳入`NSString`實體作為參數的 API 中使用`String`型別的值來替代。
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

> 注意：
更多關於字串中使用特殊字元的資訊，請查看[字串字面量的特殊字元](#unicode_special_characters)。


<a name="initializing_an_empty_string"></a>
## 初始化空字串 (Initializing an Empty String)

為了建構一個很長的字串，可以創建一個空字串作為初始值。
可以將空的字串字面量指派給變數，也可以初始化一個新的`String`實體：

```swift
var emptyString = ""               // 空字串字面量
var anotherEmptyString = String()  // 初始化 String 實體
// 兩個字串均為空並等價。
```

你可以透過檢查其`Boolean`型別的`isEmpty`屬性來判斷該字串是否為空：

```swift
if emptyString.isEmpty {
    print("Nothing to see here")
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
如果你創建了一個新的字串，那麼當其進行常數、變數指派操作或傳遞給函式/方法時，字串值是被_複製_一份過去的。
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

字串可以透過傳遞一個元素型別為`Character`的陣列作為建構子參數：

```swift
let catCharacters: [Character] = ["C", "a", "t", "!", "🐱"]
let catString = String(catCharacters)
print(catString)
// 印出："Cat!🐱"
```

<a name="concatenating_strings_and_characters"></a>
## 連接字串和字元 (Concatenating Strings and Characters)

字串和字元的值可以透過加法運算子（`+`）相加在一起並創建一個新的字串：

```swift
let string1 = "hello"
let string2 = " there"
let welcon = string1 + string2
// welcome 現在是 "hello there"
```

你也可以透過加法指派運算子 (`+=`) 將一個字串或者字元添加到一個已經存在字串變數上：

```swift
var instruction = "look over"
instruction += string2
// instruction 現在等於 "look over there"
```

你可以使用`append()`方法將一個字元加到一個字串的尾端：

```swift
let exclamationMark: Character = "!"
welcome.append(exclamationMark)
// welcome 現在等於 "hello there!"
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

在上面的範例中，`multiplier`作為`\(multiplier)`被插入到一個字串字面量中。
當創建字串執行插值計算時此占位符（placeholder）會被替換為`multiplier`實際的值。

`multiplier`的值也作為字串中後面表達式的一部分。
該表達式計算`Double(multiplier) * 2.5`的值並將結果 (7.5) 插入到字串中。
在這個範例中，表達式寫為`\(Double(multiplier) * 2.5)`並包含在字串字面量中。

> 注意：
插值字串中寫在括號中的表達式不能包含非跳脫雙引號 (`"`) 和反斜線 (`\`)，並且不能包含回車 (`\r`)或換行 (`\n`) 符號。

<a name="unicode"></a>
## Unicode

Unicode 是一個國際標準，用於文字的編碼和表示。
它使你可以用標準格式表示來自任意語言幾乎所有的字元，並能夠對文字文件或網頁這樣的外部資源中的字元進行讀寫。

Swift 的字串和字元型別是完全相容 Unicode 標準的，它支援如下所述的一系列不同的 Unicode 編碼。

<a name="unicode_scalars"></a>
### Unicode Scalars

Swift的`String`是建立於 _Unicode scalar_ 的。Unicode Scalar 是對應字元或修飾符號的唯一的21位數字，例如`U+0061`表示小寫的拉丁字母 A ("a", `LATIN SMALL LETTER A`)，`U+1F425`表示小雞表情 ("🐥", `FRONT-FACING BABY CHICK`)。

> 注意：
>  Unicode 位碼(code poing) 的範圍是`U+0000`到`U+D7FF`或者`U+E000`到`U+10FFFF`。Unicode scalar 不包括 Unicode 代理項(surrogate pair) 位碼，其位碼範圍是`U+D800`到`U+DFFF`。

注意不是每個21位數的 Unicode scalar 都表示一個字元，因為有一些被保留作未來分配的，已經代表一個典型字元的 scalar 都有自己的名字，例如上面例子中的 `LATIN SMALL LETTER A`和`FRONT-FACING BABY CHICK`。

<a name="unicode_special_characters"></a>
### 字串字面量與特殊字元

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

<a name="extended_grapheme_clusters"></a>
### 擴展字形群集 (Extended Grapheme Clusters)

每一個 Swift 的`Character`代表一個擴展字形群。一個擴展字形群是一個或多個可生成人類可讀的字元 Unicode scalar 的序列。舉例來說，字母 `é` 可以用單一的 Unicode scalar `é` (`LATIN SMALL LETTER E WITH ACUTE`, 或者`U+00E9`)來表示。然而一個標準字母`e` (`LATIN SMALL LETTER E`或者`U+0065`) 加上急促重音 (`COMBINING ACTUE ACCENT`) 的 scalar (`U+0301`) 也表示同樣的字母 `é`，這個急促重音的 scalar 將 `e` 轉換成了 `é`。

在這兩種情況下，字母 `é` 代表一個單一的 Swift `Character`，同時代表一個擴展字形群。在第一種情況，這個字形群包含一個單一 scalar；在第二種情況，它是包含兩個 scalar 的字形群。

```swift
let eAcute: Character = "\u{E9}"                         // é
let combinedEAcute: Character = "\u{65}\u{301}"          // e 後面加上  ́
// eAcute 是 é, combinedEAcute 是 é
```

擴展字形群是一個靈活的方法，放許多複雜的腳本字元表示一個單一個`Character`。例如韓文字可以表示成韓文字母的有序排列，在 Swift 中都會表示為同一個`Character`：

```swift
let precomposed: Character = "\u{D55C}"                  // 한
let decomposed: Character = "\u{1112}\u{1161}\u{11AB}"   // ᄒ, ᅡ, ᆫ
// precomposed 是 한, decomposed 是 한
```

擴展字形群可以使用包圍記號（例如`COMBINING ENCLOSING CIRCLE`或者`U+20DD`）來包圍其他 Unicode scalar 作為一個 `Character`：

```swift
let enclosedEAcute: Character = "\u{E9}\u{20DD}"
// enclosedEAcute 是 é⃝
```

局部的指示符號 scalar 可以組合成單一的`Character`，例如`REGIONAL INDICATOR SYMBOL LETTER U` (`U+1F1FA`) 和`REGIONAL INDICATOR SYMBOL LETTER S` (`U+1F1F8`)：

```swift
let regionalIndicatorForUS: Character = "\u{1F1FA}\u{1F1F8}"
// regionalIndicatorForUS 是 🇺🇸
```

<a name="counting_characters"></a>
## 計算字元數量 (Counting Characters)

想要獲得一個字串中`Character`字元數量，可以使用字串中`characters`屬性的`count`屬性：

```swift
let unusualMenagerie = "Koala 🐨, Snail 🐌, Penguin 🐧, Dromedary 🐪"
print("unusualMenagerie has \(unusualMenagerie.characters.count) characters")
// prints "unusualMenagerie has 40 characters"
```

注意到在 Swift 中，使用擴展字形群作為`Character`來連接或改變字串時，不一定會改變字串的字元數量。

例如，如果用四個字元的單字`cafe`初始化一個字串，然後加上一個`COMBINING ACTUE ACCENT` (`U+0301`) 作為字串結尾，這個字串的字元數量仍然是`4`，因為第四個字元是`é`而不是`e`：

```swift
var word = "cafe"
print("the number of characters in \(word) is \(word.characters.count)")
// 印出 "the number of characters in cafe is 4"

word += "\u{301}"    // COMBINING ACUTE ACCENT, U+0301

print("the number of characters in \(word) is \(word.characters.count)")
// 印出 "the number of characters in café is 4"
```

> 注意：
> 擴展字形群可以組成一個或多個 Unicode scalar，意味著不同的 Unicode 字元以及相同 Unicode 字元的不同表示方式可能需要不同數量的記憶體空間來儲存。所以 Swift 中的字元在一個字串中並不一定占用相同的記憶體空間。因此在還沒有獲得字串的擴展字形群的範圍的時候，不能夠計算出字串的字元數量。如果你正在處理一個長字串，需要注意`caracters`屬性必須遍歷字串中的 Unicode scalar 以精準計算字串的長度。

> 另外需要注意的是透過`characters`屬性回傳回傳的字元數量並不總是與包含相同字元的`NSString`的`length`屬性相同。`NSString`的`length`屬性是基於利用 UTF-16 表示的十六位代碼單元數字，而不是基於 Unicode 擴展字形群。為了解決這個問題，`NSString`的`length`屬性在被 Swift 的`String`存取時會成為`utf16count`。

<a name="accesing_and_modifying_a_string"></a>
## 存取和修改字串 (Accessing and Modifying a String)

你可以透過字串的屬性和方法來存取它，當然也可以用下標(subscript)完成。

### 字串索引 (String Indices)

每一個`String`值都有一個關聯的索引(index)類型：`String.Index`，它對應著字串中的每一個`Character`的位置。

前面提到，不同的字元可能會占用不同數量的記憶體空間，所以要知道`Character`的確定位置，就必須從`String`開頭遍歷每一個 Unicode scalar 直到結尾。因此，Swift 的字串不能用整數(integer)做索引。

使用`startIndex`屬性可以獲取一個`String`的第一個`Character`的索引。使用`endIndex`屬性可以獲取最後一個`Character`的下一個位置的索引。因此，`endIndex`屬性不能作為一個字串的有效下標。如果`String`是空字串，`startIndex`和`endIndex`是相等的。

透過`String.Index`的`predecessor()`方法，可以得到緊鄰的前一個索引；`successor()`方法可以緊鄰的下一個索引。任何一個`String`的索引都可以通過鏈結(chaining)這些方法來獲取另一個索引，也可以透過`advancedBy(_:)`方法來獲取。但如果嘗試獲取出界的字串索引，會拋出一個執行期錯誤。

你可以使用下標來存取`String`特定索引的`Character`。

```swift
let greeting = "Guten Tag!"
greeting[greeting.startIndex]
// G
greeting[greeting.endIndex.predecessor()]
// !
greeting[greeting.startIndex.successor()]
// u
let index = greeting.startIndex.advancedBy(7)
greeting[index]
// a
```

試圖獲取出界的索引對應的`Character`，會觸發一個執行期錯誤。

```swift
greeting[greeting.endIndex] // error
greeting.endIndex.successor() // error
```

使用`characters`屬性的`indices`屬性會建立一個包含全部索引的`Range`(範圍)，用來在一個字串中存取單個字元。

```swift
for index in greeting.characters.indices {
   print("\(greeting[index]) ", terminator: "")
}
// 打印输出 "G u t e n   T a g !"
```

### 插入和删除 (Inserting and Removing)

透過`insert(_:atIndex:)`方法可以在一個字串的指定索引插入一個字元。

```swift
var welcome = "hello"
welcome.insert("!", atIndex: welcome.endIndex)
// welcome now 現在等於 "hello!"
```

透過`insertContentsOf(_:at:)`方法可以在一個字串的指定索引插入一個字串。

```swift
welcome.insertContentsOf(" there".characters, at: welcome.endIndex.predecessor())
// welcome 現在等於 "hello there!"
```

透過`removeAtIndex(_:)`方法可以在一個字串的指定索引刪除一個字元。

```swift
welcome.removeAtIndex(welcome.endIndex.predecessor())
// welcome 現在等於 "hello there"
```

透過`removeRange(_:)`方法可以在一個字串的指定索引刪除一個子字串。

```swift
let range = welcome.endIndex.advancedBy(-6)..<welcome.endIndex
welcome.removeRange(range)
// welcome 現在等於 "hello"
```

<a name="comparing_strings"></a>
## 比較字串 (Comparing Strings)

Swift 提供了三種方式來比較文字：字串與字元相等、前綴相等和後綴相等。

<a name="string_equality"></a>
### 字串與字元相等 (String and Character Equality)

字串/字元可以用等於運算子(`==`)和不等於運算子(`!=`)，詳細描述在比較運算符：

```swift
let quotation = "We're a lot alike, you and I."
let sameQuotation = "We're a lot alike, you and I."
if quotation == sameQuotation {
    print("These two strings are considered equal")
}
// prints "These two strings are considered equal"
```

如果兩個字串（或者兩個字元）的擴展字形群是標準相等的(_canonically equivalent_)，那就認定它們是相等的。在這個情況下，即使擴展字形群是有不同的 Unicode scalar 構成的，只要它們有同樣的語意和外觀，就認為它們標準相等。

例如，`LATIN SMALL LETTER E WITH ACUTE`(`U+00E9`)就是標準相等於`LATIN SMALL LETTER E`(`U+0065`)後面加上`COMBINING ACUTE ACCENT`(`U+0301`)。這兩個字元群集都是表示字元`é`的有效方式，所以它們被認為是標準相等的：

```swift
// "Voulez-vous un café?" 使用 LATIN SMALL LETTER E WITH ACUTE
let eAcuteQuestion = "Voulez-vous un caf\u{E9}?"

// "Voulez-vous un café?" 使用 LATIN SMALL LETTER E 和 COMBINING ACUTE ACCENT
let combinedEAcuteQuestion = "Voulez-vous un caf\u{65}\u{301}?"

if eAcuteQuestion == combinedEAcuteQuestion {
    print("These two strings are considered equal")
}
// prints "These two strings are considered equal"
```

相反，英文中的`LATIN CAPITAL LETTER A`(`U+0041`，或者`A`)不等於俄文中的`CYRILLIC CAPITAL LETTER A`(`U+0410`，或者`A`)。兩個字元看著是一樣的，但卻有不同的語言意義：

```swift
let latinCapitalLetterA: Character = "\u{41}"

let cyrillicCapitalLetterA: Character = "\u{0410}"

if latinCapitalLetterA != cyrillicCapitalLetterA {
    print("These two characters are not equivalent")
}
// prints "These two characters are not equivalent"
```

> 注意： 在 Swift 中，字串和字元並不區分地理區域的。

### 前綴、後綴相等 (Prefix and Suffix Equality)

透過字串的`hasPrefix(_:)`和`hasSuffix(_:)`方法來檢查字串是否擁有特定前綴或後綴，兩個方法均接收一個`String`類型的參數，並回傳一個布林值。

下面的範例以一個字串陣列表示莎士比亞話劇《羅密歐與朱麗葉》中前兩場的場景位置：

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
// prints "6 mansion scenes; 2 cell scenes
```

> 注意：
> `hasPrefix(_:)`和`hasSuffix(_:)`方法都是在每個字串中逐個字元比較其擴展字元群是否標準相等，詳細描述在[字串與字元相等](#string_equality)。


## 字串的 Unicode 表示形式（Unicode Representations of Strings）

當一個 Unicode 字串被寫進文本文件或者其他儲存時，字串中的 Unicode scalar 會用 Unicode 定義的幾種_編碼格式(encoding forms)_編碼。每一個字串中的小塊編碼都被稱為_編碼單元(code units)_。這些包括 UTF-8 編碼格式（編碼字串為8位元的編碼單元）， UTF-16 編碼格式（編碼字串為16位元的編碼單元），以及 UTF-32 編碼格式（編碼字串32位元的編碼單元）。

Swift 提供了幾種不同的方式來存取字串的 Unicode 表示形式。 你可以用`for-in`來遍歷字串中以 Unicode 擴展字形群表示的每一個`Character`。 該過程在[使用字元](#working_with_characters)中進行了描述。

另外，能夠以其他三種 Unicode 相容的方式存取字串的值：

* UTF-8 代碼單元集合 (利用字串的`utf8`屬性存取)
* UTF-16 代碼單元集合 (利用字串的`utf16`屬性存取)
* 21位的 Unicode scalar 值集合，也就是字串的 UTF-32 編碼格式 (利用字串的`unicodeScalars`屬性存取)

下面由`D`、`o`、`g`、`‼`(`DOUBLE EXCLAMATION MARK`, Unicode scalar 為`U+203C`)和🐶(`DOG FACE`，Unicode scalar 為`U+1F436`)組成的字串中的每一個字元代表著一種不同的表示：

```swift
let dogString = "Dog‼🐶"
```

### UTF-8 表示

你可以遍歷`String`的`utf8`屬性來存取它的 UTF-8 表示。 其為`String.UTF8View`型別的屬性，是無號 8 位元值 (`UInt8`) 的集合，每一個`UInt8`值都是一個字元的 UTF-8 表示：

![UTF-8 Table](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Art/UTF8_2x.png)

```swift
for codeUnit in dogString.utf8 {
    print("\(codeUnit) ", terminator: "")
}
print("")
// 68 111 103 226 128 188 240 159 144 182
```

上面的例子中，前三個10進制`codeUnit`值 (`68`, `111`, `103`) 代表了字元`D`、`o`和`g`，它們的 UTF-8 表示與 ASCII 表示相同。 接下來的三個10進制`codeUnit`值 (`226`, `128`, `188`) 是`DOUBLE EXCLAMATION MARK`的3個位元組的 UTF-8 表示。 最後的四個`codeUnit`值 (`240`, `159`, `144`, `182`) 是`DOG FACE`的4個位元組的 UTF-8 表示。

### UTF-16 表示

你可以遍歷`String`的`utf16`屬性來存取它的 UTF-16 表示。 其為`String.UTF16View`型別的屬性，是無號 16 位元值 (`UInt16`) 的集合，每一個`UInt16`都是一個字元的 UTF-16 表示：

![UTF-16 Table](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Art/UTF16_2x.png)

```swift
for codeUnit in dogString.utf16 {
    print("\(codeUnit) ", terminator: "")
}
print("")
// 68 111 103 8252 55357 56374
```

同樣，前三個`codeUnit`值 (`68`, `111`, `103`) 代表了字元`D`、`o`和`g`，它們的 UTF-16 代碼單元和 UTF-8 完全相同（因為這些 Unicode scalar 表示 ASCII 字元）。

第四個`codeUnit`值 (`8252`) 是一個等於十六進制`203C`的的十進制值。這個代表了`DOUBLE EXCLAMATION MARK`字元的 Unicode scalar 值`U+203C`。這個字元在 UTF-16 中可以用一個編碼單元表示。

第五和第六個`codeUnit`值 (`55357`和`56374`) 是`DOG FACE`字元的 UTF-16 表示。 第一個值為`U+D83D`(十進制值為`55357`)，第二個值為`U+DC36`(十進制值為`56374`)。

### Unicode scalar 表示 (Unicode Scalars Representation)

你可以遍歷`String`值的`unicodeScalars`屬性來存取它的 Unicode scalar 表示。 其為`UnicodeScalarView`類型的屬性，是`UnicodeScalar`的集合。 

每一個`UnicodeScalar`擁有一個`value`屬性，可以回傳對應的21位元數值，用`UInt32`來表示：

![UnicodeScalar Table](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Art/UnicodeScalar_2x.png)

```swift
for scalar in dogString.unicodeScalars {
    print("\(scalar.value) ", terminator: "")
}
print("")
// 68 111 103 8252 128054
```

前三個`UnicodeScalar`值(`68`, `111`, `103`)的`value`屬性仍然代表字元`D`、`o`和`g`。 第四個`codeUnit`值(`8252`)仍然是一個等於十六進制`203C`的十進制值。這個代表了`DOUBLE EXCLAMATION MARK`字元的 Unicode scalar `U+203C`。

第五個`UnicodeScalar`值的`value`屬性，`128054`，是一個十六進制`1F436`的十進制表示。其等同於`DOG FACE`的 Unicode scalar `U+1F436`。

作為查詢它們的`value`屬性的一種替代方法，每個`UnicodeScalar`值也可以用來建立一個新的`String`，比如在字串插值中使用：

```swift
for scalar in dogString.unicodeScalars {
    print("\(scalar) ")
}
// D
// o
// g
// ‼
// 🐶
```
