> 翻譯：[tommy60703](https://github.com/tommy60703)

# 基本運算子
-----------------

本頁包含內容：

- [術語](#terminology)
- [指派運算子](#assignment_operator)
- [數值運算子](#arithmetic_operators)
- [複合指派運算子（Compound Assignment Operators）](#compound_assignment_operators)
- [比較運算子](#comparison_operators)
- [三元條件運算子（Ternary Conditional Operator）](#ternary_conditional_operator)
- [空值聚合運算子（Nil Coalescing Operator）](#nil_coalescing_operators)
- [區間運算子](#range_operators)
- [邏輯運算子](#logical_operators)

運算子是檢查、改變、合並值的特殊符號或短語。例如，加號`+`將兩個數相加（如`let i = 1 + 2`）。複雜些的運算例如邏輯 AND 運算子`&&`（如`if enteredDoorCode && passedRetinaScan`），又或直接讓 i 值加 1 的累加運算子`++i`等。

Swift 支援大部分標準 C 語言的運算子，且改進許多特性來減少常見的編碼錯誤。如，指派運算子（`=`）不回傳值，以防止把想要判斷相等運算子（`==`）的地方寫成指派運算子導致的錯誤。數值運算子（`+`，`-`，`*`，`/`，`%`等）會檢測並不允許溢位，以此來避免保存變數時由於變數大於或小於其型別所能儲存的範圍時導致的異常結果。當然允許你使用 Swift 的溢位運算子來實作溢位。詳情參見[溢位運算子](23_Advanced_Operators.html#overflow_operators)。

有別於 C 語言，在 Swift 中你可以對浮點數進行餘數運算（`%`），Swift 還提供了 C 語言所沒有的，用來表達兩數之間的值的區間運算子，（`a..<b`和`a...b`），這方便我們表達一個區間內的數值。

本章節只描述了 Swift 中的基本運算子，[進階運算子](23_Advanced_Operators.html)包含了進階運算子、如何自定義運算子，以及如何進行自定義型別的運算子多載。

<a name="terminology"></a>
## 術語

運算子有一元，二元和三元運算子。

- 一元運算子對單一操作物件操作（如`-a`）。一元運算子分前綴運算子和後綴運算子，前綴運算子需要緊跟在操作物件之前（如`!b`），後綴運算子需要緊跟在操作物件之後（如`i++`）。
- 二元運算子操作兩個操作物件（如`2 + 3`），是中綴的，因為它們出現在兩個操作物件之間。
- 三元運算子操作三個操作物件，和 C 語言一樣，Swift 只有一個三元運算子，就是三元條件運算子（`a ? b : c`）。

受運算子影響的值叫運算元，在表達式`1 + 2`中，加號`+`是二元運算子，它的兩個運算元是值`1`和`2`。

<a name="assignment_operator"></a>
## 指派運算子

指派運算（`a = b`），表示用`b`的值來初始化或更新`a`的值：

```swift
let b = 10
var a = 5
a = b
// a 現在等於 10
```

如果指派的右邊是一個 tuple，它的元素可以馬上被分解多個常數或變數：

```swiflt
let (x, y) = (1, 2)
// 現在 x 等於 1, y 等於 2
```

與 C 語言和 Objective-C 不同，Swift 的指派運算子並不回傳任何值。所以以下程式碼是錯誤的：

```swift
if x = y {
    // 此句錯誤, 因為 x = y 並不回傳任何值
}
```

這個特性保護你不會把（`==`）錯寫成（`=`）了，由於`if x = y`是錯誤程式碼，Swift 從底層幫你避免了這些程式碼錯誤。

<a name="arithmetic_operators"></a>
## 數值運算子

Swift 支援所有數值型別基本的四則運算：

- 加法（`+`）
- 減法（`-`）
- 乘法（`*`）
- 除法（`/`）

```swift
1 + 2       // 等於 3
5 - 3       // 等於 2
2 * 3       // 等於 6
10.0 / 2.5  // 等於 4.0
```

與 C 語言和 Objective-C 不同的是，Swift 預設不允許在數值運算中出現溢位情況。但你可以使用 Swift 的溢位運算子來達到你有目的的溢位（如`a &+ b`）。詳情參見[溢位運算子](23_Advanced_Operators.html#overflow_operators)。

加法運算子也用於`String`的拼接：

```swift
"hello, " + "world"  // 等於 "hello, world"
```

### 餘數運算

餘數運算（`a % b`）是計算`b`的多少倍剛剛好可以容入`a`，回傳多出來的那部分（餘數）。

>注意：
餘數運算（`%`）在其他語言也叫取模運算。然而嚴格說來，我們看該運算子對負數的操作結果，"餘數"比"取模"更合適些。

我們來談談取餘數是怎麼回事，計算`9 % 4`，你先計算出`4`的多少倍會剛好可以容入`9`中：

![Art/remainderInteger_2x.png](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Art/remainderInteger_2x.png "Art/remainderInteger_2x.png")

在 Swift 中這麼來表達：

```swift
9 % 4    // 等於 1
```

為了得到`a % b`的結果，`%`計算了以下等式，並輸出`餘數`作為結果：

*a = (b × 倍數) + 餘數*

當`倍數`取最大值的時候，就會剛好可以容入`a`中。

把`9`和`4`代入等式中，我們得`1`：

```swift
9 = (4 × 2) + 1
```

同樣的方法，我來們計算 `-9 % 4`：

```swift
-9 % 4   // 等於 -1
```

把`-9`和`4`代入等式，`-2`是取到的最大整數：

```swift
-9 = (4 × -2) + -1
```

餘數是`-1`。

在對負數`b`餘數時，`b`的符號會被忽略。這意味著 `a % b` 和 `a % -b`的結果是相同的。

### 浮點數餘數計算

不同於 C 語言和 Objective-C，Swift 是可以對浮點數取餘數的。

```swift
8 % 2.5 // 等於 0.5
```

這個範例中，`8`除於`2.5`等於`3`余`0.5`，所以結果是一個`Double`值`0.5`。

![Art/remainderFloat_2x.png](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Art/remainderFloat_2x.png "Art/remainderFloat_2x.png")

### 累加和累減運算

和 C 語言一樣，Swift 也提供了方便對變數本身加 1 或減 1 的累加（`++`）和累減（`--`）運算子。其操作物件可以是整數和浮點數。
‌
```swift
var i = 0
++i      // 現在 i = 1
```

每呼叫一次`++i`，`i`的值就會加 1。實際上，`++i`是`i = i + 1`的簡寫，而`--i`是`i = i - 1`的簡寫。

`++`和`--`既是前綴又是後綴運算子。`++i`，`i++`，`--i`和`i--`都是有效的寫法。

我們需要注意的是這些運算子修改了`i`後有一個回傳值。如果你只想修改`i`的值，那你就可以忽略這個回傳值。但如果你想使用回傳值，你就需要留意前綴和後綴操作的回傳值是不同的。

- 當`++`前綴的時候，先自増再回傳。

- 當`++`後綴的時候，先回傳再累加。

例如：

```swift
var a = 0
let b = ++a // a 和 b 現在都是 1
let c = a++ // a 現在 2, 但 c 是 a 累加前的值 1
```

上述範例，`let b = ++a`先把`a`加 1 了再回傳`a`的值。所以`a`和`b`都是新值`1`。

而`let c = a++`，是先回傳了`a`的值，然後`a`才加 1。所以`c`得到了`a`的舊值 1，而`a`加 1 後變成 2。

除非你需要使用`i++`的特性，不然推薦你使用`++i`和`--i`，因為先修改後回傳這樣的行為更符合我們的邏輯。


### 一元負號

數值的正負號可以使用前綴`-`（即一元負號）來切換：

```swift
let three = 3
let minusThree = -three       // minusThree 等於 -3
let plusThree = -minusThree   // plusThree 等於 3, 或 "負負3"
```

一元負號（`-`）寫在運算元之前，中間沒有空格。

### 一元正號

一元正號（`+`）不做任何改變地回傳運算元的值。

```swift
let minusSix = -6
let alsoMinusSix = +minusSix  // alsoMinusSix 等於 -6
```
雖然一元`+`做無用功，但當你在使用一元負號來表達負數時，你可以使用一元正號來表達正數，如此你的程式碼會具有對稱之美。


<a name="compound_assignment_operators"></a>
## 複合指派（Compound Assignment Operators）

如同強大的 C 語言，Swift 也提供把其他運算子和指派運算（`=`）組合起來的複合指派運算子，加賦運算（`+=`）是其中一個範例：

```swift
var a = 1
a += 2 // a 現在是 3
```

表達式`a += 2`是`a = a + 2`的簡寫，一個加賦運算就把加法和指派兩件事完成了。

>注意：
複合指派運算沒有回傳值，`let b = a += 2`這類別程式碼是錯誤。這不同於上面提到的累加和累減運算子。

在[表達式](../chapter3/04_Expressions.html)章節裡有複合運算子的完整列表。
‌
<a name="comparison_operators"></a>
## 比較運算

所有標準 C 語言中的比較運算都可以在 Swift 中使用。

- 等於（`a == b`）
- 不等於（`a != b`）
- 大於（`a > b`）
- 小於（`a < b`）
- 大於等於（`a >= b`）
- 小於等於（`a <= b`）

> 注意：
Swift 也提供恆等`===`和不恆等`!==`這兩個比較符來判斷兩個物件是否參考同一個物件實例。更多細節在[類別別與結構](09_Classes_and_Structures.html)。

每個比較運算都回傳了一個顯示表達式是否成立的布林值：

```swift
1 == 1   // true, 因為 1 等於 1
2 != 1   // true, 因為 2 不等於 1
2 > 1    // true, 因為 2 大於 1
1 < 2    // true, 因為 1 小於2
1 >= 1   // true, 因為 1 大於等於 1
2 <= 1   // false, 因為 2 並不小於等於 1
```

比較運算多用於條件語句，如`if`條件：

```swift
let name = "world"
if name == "world" {
    print("hello, world")
} else {
    print("I'm sorry \(name), but I don't recognize you")
}
// 輸出 "hello, world", 因為 `name` 就是等於 "world"
```

關於`if`語句，請看[控制流程](05_Control_Flow.html)。

<a name="ternary_conditional_operator"></a>
## 三元條件運算(Ternary Conditional Operator)

三元條件運算的特殊在於它是有三個運算元的運算子，它的原型是 `問題 ? 答案1 : 答案2`。它簡潔地表達根據`問題`成立與否作出二選一的操作。如果`問題`成立，回傳`答案1`的結果; 如果不成立，回傳`答案2`的結果。

使用三元條件運算簡化了以下程式碼：

```swift
if question {
    answer1
} else {
    answer2
}
```

這裡有個計算表格行高的範例。如果有表頭，那行高應比內容高度要高出 50 像素; 如果沒有表頭，只需高出 20 像素。

```swift
let contentHeight = 40
let hasHeader = true
let rowHeight = contentHeight + (hasHeader ? 50 : 20)
// rowHeight 現在是 90
```

這樣寫會比下面的程式碼簡潔：

```swift
let contentHeight = 40
let hasHeader = true
var rowHeight = contentHeight
if hasHeader {
    rowHeight = rowHeight + 50
} else {
    rowHeight = rowHeight + 20
}
// rowHeight 現在是 90
```

第一段程式碼範例使用了三元條件運算，所以一行程式碼就能讓我們得到正確答案。這比第二段程式碼簡潔得多，無需將`rowHeight`定義成變數，因為它的值無需在`if`語句中改變。

三元條件運算提供有效率且便捷的方式來表達二選一的選擇。需要注意的事，過度使用三元條件運算就會由簡潔的程式碼變成難懂的程式碼。我們應避免在一個組合語句使用多個三元條件運算子。

<a name="nil_coalescing_operators"></a>
## 空值聚合運算子

空值聚合（nil coalescing）運算子（`a ?? b`）如果`optional a`有值的話就解析，但若`a`是空值，就回傳預設值`b`。
這運算元`a`永遠是個optional型別。而運算子`b`必須得式`a`的型別。

空值聚合運算子是以下表達式的簡碼。

```swift
a != nil ? a! : b
```

上述程式碼使用三元條件運算子，並且當`a`不是空值時，強制解析（`a!`）來存取`a`，若`a`是空值則回傳`b`。空值聚合運算子題空一個更優雅的方式，並以一個明確可讀的形式來封裝類似的條件確認及解析。

> 注意：
若a是非空值，那就不會運算b。這就是所謂的_最小化求值_(短路求值)。

下面這個例子使用空值聚合運算子來選擇預設顏色值或是一個optional使用者定義的顏色值。

```swift
let defaultColorName = "red"
var userDefinedColorName: String?   // 預設為空值

var colorNameToUse = userDefinedColorName ?? defaultColorName
// userDefinedColorName 是空值, 所以 colorNameToUse 就被設成預設的「red」。
```

變數`userDefinedColorName`是定義成optional字串型別，且其預設值為空值。因為`userDefinedColorName`變數是個optional型別，你可以使用空值聚合運算子來決定他的值。在上面的例子當中，這運算子被用來定義一個Sting型別`colorNameToUse`變數的初始值。因為`userDefinedColorName`是空值，這表達式` userDefinedColorName ?? defaultColorName` 回傳 `defaultColorName`，也就是「red」。

如果你把`userDefinedColorName`設成非空值，並且執行空值聚合運算子來確認，則`userDefinedColorName`就會被解析，而不是使用預設值：

```swift
userDefinedColorName = "green"
colorNameToUse = userDefinedColorName ?? defaultColorName
// userDefinedColorName非空值，所以colorNameToUse被設成「green」。
```

<a name="range_operators"></a>
## 區間運算子

Swift 提供了兩個方便表達一個區間的值的運算子。

### 閉區間運算子
閉區間運算子（`a...b`）定義一個包含從`a`到`b`(包括`a`和`b`)的所有值的區間。`a`的值必不大於`b`。
‌
閉區間運算子在迭代一個區間的所有值時是非常有用的，如在`for-in`迴圈中：

```swift
for index in 1...5 {
    print("\(index) * 5 = \(index * 5)")
}
// 1 * 5 = 5
// 2 * 5 = 10
// 3 * 5 = 15
// 4 * 5 = 20
// 5 * 5 = 25
```

關於`for-in`，請看[控制流程](05_Control_Flow.html)。

### 半閉區間運算子

半閉區間運算子（`a..<b`）定義一個從`a`到`b`但不包括`b`的區間。
之所以稱為半閉區間，是因為該區間包含第一個值而不包括最後的值。就像閉區間運算子一樣，`a`的值必不大於`b`。如果`a`跟`b`相等，那結果的區間就會是空。

半閉區間運算子的實用性在於當你使用一個 0 始的列表(如陣列)時，非常方便地從 0 數到列表的長度。

```swift
let names = ["Anna", "Alex", "Brian", "Jack"]
let count = names.count
for i in 0..count {
    print("第 \(i + 1) 個人叫 \(names[i])")
}
// 第 1 個人叫 Anna
// 第 2 個人叫 Alex
// 第 3 個人叫 Brian
// 第 4 個人叫 Jack
```

陣列有 4 個元素，但`0..<count`只數到 3(最後一個元素的索引)，因為它是半閉區間。關於陣列，請查閱[陣列](04_Collection_Types.html#arrays)。

<a name="logical_operators"></a>
## 邏輯運算

邏輯運算的操作物件是邏輯布林值。Swift 支援基於 C 語言的三個標準邏輯運算。

- 邏輯非（`!a`）
- 邏輯且（`a && b`）
- 邏輯或（`a || b`）

### 邏輯非

邏輯非運算子（`!a`）對一個布林值取反，使得`true`變成`false`，`false`變成`true`。

它是一個前綴運算子，需出現在運算元之前，且不加空格。讀作`非 a`，然後我們看以下範例：

```swift
let allowedEntry = false
if !allowedEntry {
    print("ACCESS DENIED")
}
// 輸出 "ACCESS DENIED"
```

`if !allowedEntry`語句可以讀作「如果非 alowed entry」，接下一行程式碼只有在如果「非 allow entry」為`true`，即`allowEntry`為`false`時被執行。

在範例程式碼中，小心地選擇布林常數或變數有助於程式碼的可讀性，並且避免使用雙重邏輯非運算，或混亂的邏輯語句。

### 邏輯且
邏輯且運算子（`a && b`）表達了只有`a`和`b`的值都為`true`時，整個表達式的值才會是`true`。

只要任意一個值為`false`，整個表達式的值就為`false`。事實上，如果第一個值為`false`，那麼是不去計算第二個值的，因為它已經不可能影響整個表達式的結果了。這被稱做「捷徑計算（short-circuit evaluation）」。

以下範例，只有兩個`Bool`值都為`true`值的時候才允許進入：

```swift
let enteredDoorCode = true
let passedRetinaScan = false
if enteredDoorCode && passedRetinaScan {
    print("Welcome!")
} else {
    print("ACCESS DENIED")
}
// 輸出 "ACCESS DENIED"
```

### 邏輯或
邏輯或運算子（`a || b`）是一個由兩個連續的`|`組成的中綴運算子。它表示了兩個邏輯表達式的其中一個為`true`，整個表達式就為`true`。

和邏輯且運算子類似，邏輯或也使用「捷徑計算」，當左端的表達式為`true`時，將不計算右邊的表達式了，因為它不可能改變整個表達式的值了。

以下範例程式碼中，第一個布林值（`hasDoorKey`）為`false`，但第二個值（`knowsOverridePassword`）為`true`，所以整個表達是`true`，於是允許進入：

```swift
let hasDoorKey = false
let knowsOverridePassword = true
if hasDoorKey || knowsOverridePassword {
    print("Welcome!")
} else {
    print("ACCESS DENIED")
}
// 輸出 "Welcome!"
```

### 組合邏輯

我們可以組合多個邏輯運算來表達一個複合邏輯：

```swift
if enteredDoorCode && passedRetinaScan || hasDoorKey || knowsOverridePassword {
    print("Welcome!")
} else {
    print("ACCESS DENIED")
}
// 輸出 "Welcome!"
```

這個範例使用了含多個`&&`和`||`的複合邏輯。但無論怎樣，`&&`和`||`始終只能操作兩個值。所以這實際是三個簡單邏輯連續操作的結果。我們來解讀一下：

如果我們輸入了正確的密碼並通過了視網膜掃描; 或者我們有一把有效的鑰匙; 又或者我們知道緊急情況下重置的密碼，我們就能把門打開進入。

前兩種情況，我們都不滿足，所以前兩個簡單邏輯的結果是`false`，但是我們是知道緊急情況下重置的密碼的，所以整個複雜表達式的值還是`true`。

### 使用括號來表明優先級

為了一個複雜表達式更容易讀懂，在合適的地方使用括號來表明優先級是很有效的，雖然它並非必要的。在上個關於門的權限的範例中，我們給第一個部分加個括號，使用它看起來邏輯更明確：

```swift
if (enteredDoorCode && passedRetinaScan) || hasDoorKey || knowsOverridePassword {
    print("Welcome!")
} else {
    print("ACCESS DENIED")
}
// 輸出 "Welcome!"
```

這括號使得前兩個值被看成整個邏輯表達中獨立的一個部分。雖然有括號和沒括號的輸出結果是一樣的，但對於讀程式碼的人來說有括號的程式碼更清晰。可讀性比簡潔性更重要，請在可以讓你程式碼變清晰地地方加個括號吧！
