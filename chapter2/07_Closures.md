> 翻譯：[wh1100717](https://github.com/wh1100717)
> 校對：[lyuka](https://github.com/lyuka)

# 閉包（Closures）
-----------------

本頁包含內容：

- [閉包表達式（Closure Expressions）](#closure_expressions)
- [尾隨閉包（Trailing Closures）](#trailing_closures)
- [值捕獲（Capturing Values）](#capturing_values)
- [閉包是參考型別（Closures Are Reference Types）](#closures_are_reference_types)

閉包是自包含的函式程式碼塊，可以在程式碼中被傳遞和使用。
Swift 中的閉包與 C 和 Objective-C 中的程式碼塊（blocks）以及其他一些程式語言中的 lambdas 函式比較相似。

閉包可以捕獲和儲存其所在上下文中任意常數和變數的參考。
這就是所謂的閉合並包裹著這些常數和變數，俗稱閉包。Swift 會為你管理在捕獲過程中涉及到的所有內存操作。

> 注意：  
> 如果你不熟悉捕獲（capturing）這個概念也不用擔心，你可以在 [值捕獲](#capturing_values) 章節對其進行詳細了解。  

在[函式](../chapter2/06_Functions.html) 章節中介紹的全域和嵌套函式實際上也是特殊的閉包，閉包采取如下三種形式之一：

* 全域函式是一個有名字但不會捕獲任何值的閉包
* 嵌套函式是一個有名字並可以捕獲其封閉函式域內值的閉包
* 閉包表達式是一個利用輕量級語法所寫的可以捕獲其上下文中變數或常數值的匿名閉包

Swift 的閉包表達式擁有簡潔的風格，並鼓勵在常見場景中進行語法優化，主要優化如下：

* 利用上下文推斷參數和回傳值型別
* 隱式回傳單表達式閉包，即單表達式閉包可以省略`return`關鍵字
* 參數名稱縮寫
* 尾隨（Trailing）閉包語法

<a name="closure_expressions"></a>
## 閉包表達式（Closure Expressions）


[嵌套函式](../chapter2/06_Functions.html#nested_function) 是一個在較複雜函式中方便進行命名和定義自包含程式碼模塊的方式。當然，有時候撰寫小巧的沒有完整定義和命名的類別函式結構也是很有用處的，尤其是在你處理一些函式並需要將另外一些函式作為該函式的參數時。

閉包表達式是一種利用簡潔語法構建內聯閉包的方式。
閉包表達式提供了一些語法優化，使得撰寫閉包變得簡單明了。
下面閉包表達式的範例通過使用幾次迭代展示了`sort`函式定義和語法優化的方式。
每一次迭代都用更簡潔的方式描述了相同的功能。

<a name="the_sort_function"></a>
### sort 函式（The Sort Function）

Swift 標準函式庫提供了`sort`函式，會根據你提供的基於輸出型別排序的閉包函式將已知型別陣列中的值進行排序。
一旦排序完成，函式會回傳一個與原陣列大小相同的新陣列，該陣列中包含已經正確排序的同型別元素。

下面的閉包表達式示例使用`sort`函式對一個`String`型別的陣列進行字母逆序排序，以下是初始陣列值：

```swift
let names = ["Chris", "Alex", "Ewa", "Barry", "Daniella"]
```

`sort`函式需要傳入兩個參數：

* 已知型別的陣列
* 閉包函式，該閉包函式需要傳入與陣列型別相同的兩個值，並回傳一個布林型別值來告訴`sort`函式當排序結束後傳入的第一個參數排在第二個參數前面還是後面。如果第一個參數值出現在第二個參數值前面，排序閉包函式需要回傳`true`，反之回傳`false`。

該範例對一個`String`型別的陣列進行排序，因此排序閉包函式型別需為`(String, String) -> Bool`。

提供排序閉包函式的一種方式是撰寫一個符合其型別要求的普通函式，並將其作為`sort`函式的第二個參數傳入：

```swift
func backwards(s1: String, s2: String) -> Bool {
    return s1 > s2
}
var reversed = sort(names, backwards)
// reversed 為 ["Ewa", "Daniella", "Chris", "Barry", "Alex"]
```

如果第一個字串 (`s1`) 大於第二個字串 (`s2`)，`backwards`函式回傳`true`，表示在新的陣列中`s1`應該出現在`s2`前。
對於字串中的字元來說，“大於” 表示 “按照字母順序較晚出現”。
這意味著字母`"B"`大於字母`"A"`，字串`"Tom"`大於字串`"Tim"`。
其將進行字母逆序排序，`"Barry"`將會排在`"Alex"`之後。

然而，這是一個相當冗長的方式，本質上只是寫了一個單表達式函式 (a > b)。
在下面的範例中，利用閉合表達式語法可以更好的構造一個內聯排序閉包。

<a name="closure_expression_syntax"></a>
### 閉包表達式語法（Closure Expression Syntax）

閉包表達式語法有如下一般形式：

```swift
{ (parameters) -> returnType in
    statements
}
```

閉包表達式語法可以使用常數、變數和`inout`型別作為參數，不提供預設值。
也可以在參數列表的最後使用可變參數。
元組也可以作為參數和回傳值。

下面的範例展示了之前`backwards`函式對應的閉包表達式版本的程式碼：

```swift
reversed = sort(names, { (s1: String, s2: String) -> Bool in
    return s1 > s2
})
```

需要注意的是內聯閉包參數和回傳值型別宣告與`backwards`函式型別宣告相同。
在這兩種方式中，都寫成了`(s1: String, s2: String) -> Bool`。
然而在內聯閉包表達式中，函式和回傳值型別都寫在大括號內，而不是大括號外。

閉包的函式體部分由關鍵字`in`引入。
該關鍵字表示閉包的參數和回傳值型別定義已經完成，閉包函式體即將開始。

因為這個閉包的函式體部分如此短以至於可以將其改寫成一行程式碼：

```swift
reversed = sort(names, { (s1: String, s2: String) -> Bool in return s1 > s2 } )
```

這說明`sort`函式的整體呼叫保持不變，一對圓括號仍然包裹住了函式中整個參數集合。而其中一個參數現在變成了內聯閉包（相比於`backwards`版本的程式碼）。

<a name="inferring_type_from_context"></a>
### 根據上下文推斷型別（Inferring Type From Context）

因為排序閉包函式是作為`sort`函式的參數進行傳入的，Swift可以推斷其參數和回傳值的型別。
`sort`期望第二個參數是型別為`(String, String) -> Bool`的函式，因此實際上`String`,`String`和`Bool`型別並不需要作為閉包表達式定義中的一部分。
因為所有的型別都可以被正確推斷，回傳箭頭 (`->`) 和圍繞在參數周圍的括號也可以被省略：

```swift
reversed = sort(names, { s1, s2 in return s1 > s2 } )
```

實際上任何情況下，通過內聯閉包表達式構造的閉包作為參數傳遞給函式時，都可以推斷出閉包的參數和回傳值型別，這意味著你幾乎不需要利用完整格式構造任何內聯閉包。

<a name="implicit_returns_from_single_expression_closures"></a>
### 單表達式閉包隱式回傳（Implicit Return From Single-Expression Clossures）

單行表達式閉包可以通過隱藏`return`關鍵字來隱式回傳單行表達式的結果，如上版本的範例可以改寫為：

```swift
reversed = sort(names, { s1, s2 in s1 > s2 } )
```

在這個範例中，`sort`函式的第二個參數函式型別明確了閉包必須回傳一個`Bool`型別值。
因為閉包函式體只包含了一個單一表達式 (`s1 > s2`)，該表達式回傳`Bool`型別值，因此這裡沒有歧義，`return`關鍵字可以省略。

<a name="shorthand_argument_names"></a>
### 參數名稱縮寫（Shorthand Argument Names）

Swift 自動為內聯函式提供了參數名稱縮寫功能，你可以直接通過`$0`,`$1`,`$2`來順序呼叫閉包的參數。

如果你在閉包表達式中使用參數名稱縮寫，你可以在閉包參數列表中省略對其的定義，並且對應參數名稱縮寫的型別會通過函式型別進行推斷。
`in`關鍵字也同樣可以被省略，因為此時閉包表達式完全由閉包函式體構成：

```swift
reversed = sort(names, { $0 > $1 } )
```

在這個範例中，`$0`和`$1`表示閉包中第一個和第二個`String`型別的參數。

<a name="operator_functions"></a>
### 運算子函式（Operator Functions）

實際上還有一種更簡短的方式來撰寫上面範例中的閉包表達式。
Swift 的`String`型別定義了關於大於號 (`>`) 的字串實作，其作為一個函式接受兩個`String`型別的參數並回傳`Bool`型別的值。
而這正好與`sort`函式的第二個參數需要的函式型別相符合。
因此，你可以簡單地傳遞一個大於號，Swift可以自動推斷出你想使用大於號的字串函式實作：

```swift
reversed = sort(names, >)
```

更多關於運算子表達式的內容請查看 [運算子函式](../chapter2/23_Advanced_Operators.html#operator_functions)。

<a name="trailing_closures"></a>
## 尾隨閉包（Trailing Closures）


如果你需要將一個很長的閉包表達式作為最後一個參數傳遞給函式，可以使用尾隨閉包來增強函式的可讀性。
尾隨閉包是一個書寫在函式括號之後的閉包表達式，函式支援將其作為最後一個參數呼叫。

```swift
func someFunctionThatTakesAClosure(closure: () -> ()) {
    // 函式體部分
}

// 以下是不使用尾隨閉包進行函式呼叫
someFunctionThatTakesAClosure({
    // 閉包主體部分
})

// 以下是使用尾隨閉包進行函式呼叫
someFunctionThatTakesAClosure() {
  // 閉包主體部分
}
```

> 注意：  
> 如果函式只需要閉包表達式一個參數，當你使用尾隨閉包時，你甚至可以把`()`省略掉。  

在上例中作為`sort`函式參數的字串排序閉包可以改寫為：

```swift
reversed = sort(names) { $0 > $1 }
```

當閉包非常長以至於不能在一行中進行書寫時，尾隨閉包變得非常有用。
舉例來說，Swift 的`Array`型別有一個`map`方法，其獲取一個閉包表達式作為其唯一參數。
陣列中的每一個元素呼叫一次該閉包函式，並回傳該元素所映射的值(也可以是不同型別的值)。
具體的映射方式和回傳值型別由閉包來指定。

當提供給陣列閉包函式後，`map`方法將回傳一個新的陣列，陣列中包含了與原陣列一一對應的映射後的值。

下例介紹了如何在`map`方法中使用尾隨閉包將`Int`型別陣列`[16,58,510]`轉換為包含對應`String`型別的陣列`["OneSix", "FiveEight", "FiveOneZero"]`:

```swift
let digitNames = [
    0: "Zero", 1: "One", 2: "Two",   3: "Three", 4: "Four",
    5: "Five", 6: "Six", 7: "Seven", 8: "Eight", 9: "Nine"
]
let numbers = [16, 58, 510]
```

如上程式碼創建了一個數字位和它們名字映射的英文版本字典。
同時定義了一個准備轉換為字串的整型陣列。

你現在可以通過傳遞一個尾隨閉包給`numbers`的`map`方法來創建對應的字串版本陣列。
需要注意的時呼叫`numbers.map`不需要在`map`後面包含任何括號，因為其只需要傳遞閉包表達式這一個參數，並且該閉包表達式參數通過尾隨方式進行撰寫：

```swift
let strings = numbers.map {
    (var number) -> String in
    var output = ""
    while number > 0 {
        output = digitNames[number % 10]! + output
        number /= 10
    }
    return output
}
// strings 常數被推斷為字串型別陣列，即 String[]
// 其值為 ["OneSix", "FiveEight", "FiveOneZero"]
```

`map`在陣列中為每一個元素呼叫了閉包表達式。
你不需要指定閉包的輸入參數`number`的型別，因為可以通過要映射的陣列型別進行推斷。

閉包`number`參數被宣告為一個變數參數（變數的具體描述請參看[常數參數和變數參數](../chapter2/06_Functions.html#constant_and_variable_parameters)），因此可以在閉包函式體內對其進行修改。閉包表達式制定了回傳型別為`String`，以表明儲存映射值的新陣列型別為`String`。

閉包表達式在每次被呼叫的時候創建了一個字串並回傳。
其使用取餘運算子 (number % 10) 計算最後一位數字並利用`digitNames`字典獲取所映射的字串。

> 注意：  
> 字典`digitNames`下標後跟著一個嘆號 (!)，因為字典下標回傳一個可選值 (optional value)，表明即使該 key 不存在也不會查找失敗。  
> 在上例中，它保證了`number % 10`可以總是作為一個`digitNames`字典的有效下標 key。  
> 因此嘆號可以用於強制解析 (force-unwrap) 儲存在可選下標項中的`String`型別值。  

從`digitNames`字典中獲取的字串被添加到輸出的前部，逆序建立了一個字串版本的數字。
（在表達式`number % 10`中，如果number為16，則回傳6，58回傳8，510回傳0）。

`number`變數之後除以10。
因為其是整數，在計算過程中未除盡部分被忽略。
因此 16變成了1，58變成了5，510變成了51。

整個過程重複進行，直到`number /= 10`為0，這時閉包會將字串輸出，而`map`函式則會將字串添加到所映射的陣列中。

上例中尾隨閉包語法在函式後整潔封裝了具體的閉包功能，而不再需要將整個閉包包裹在`map`函式的括號內。

<a name="capturing_values"></a>
## 捕獲值（Capturing Values）


閉包可以在其定義的上下文中捕獲常數或變數。
即使定義這些常數和變數的原域已經不存在，閉包仍然可以在閉包函式體內參考和修改這些值。

Swift最簡單的閉包形式是嵌套函式，也就是定義在其他函式的函式體內的函式。
嵌套函式可以捕獲其外部函式所有的參數以及定義的常數和變數。

下例為一個叫做`makeIncrementor`的函式，其包含了一個叫做`incrementor`嵌套函式。
嵌套函式`incrementor`從上下文中捕獲了兩個值，`runningTotal`和`amount`。
之後`makeIncrementor`將`incrementor`作為閉包回傳。
每次呼叫`incrementor`時，其會以`amount`作為增量增加`runningTotal`的值。

```swift
func makeIncrementor(forIncrement amount: Int) -> () -> Int {
    var runningTotal = 0
    func incrementor() -> Int {
        runningTotal += amount
        return runningTotal
    }
    return incrementor
}
```

`makeIncrementor`回傳型別為`() -> Int`。
這意味著其回傳的是一個函式，而不是一個簡單型別值。
該函式在每次呼叫時不接受參數只回傳一個`Int`型別的值。
關於函式回傳其他函式的內容，請查看[函式型別作為回傳型別](../chapter2/06_Functions.html#function_types_as_return_types)。

`makeIncrementor`函式定義了一個整型變數`runningTotal`(初始為0) 用來儲存當前跑步總數。
該值通過`incrementor`回傳。

`makeIncrementor`有一個`Int`型別的參數，其外部命名為`forIncrement`， 內部命名為`amount`，表示每次`incrementor`被呼叫時`runningTotal`將要增加的量。

`incrementor`函式用來執行實際的增加操作。
該函式簡單地使`runningTotal`增加`amount`，並將其回傳。

如果我們單獨看這個函式，會發現看上去不同尋常：

```swift
func incrementor() -> Int {
    runningTotal += amount
    return runningTotal
}
```

`incrementor`函式並沒有獲取任何參數，但是在函式體內存取了`runningTotal`和`amount`變數。這是因為其通過捕獲在包含它的函式體內已經存在的`runningTotal`和`amount`變數而實作。

由於沒有修改`amount`變數，`incrementor`實際上捕獲並儲存了該變數的一個副本，而該副本隨著`incrementor`一同被儲存。

然而，因為每次呼叫該函式的時候都會修改`runningTotal`的值，`incrementor`捕獲了當前`runningTotal`變數的參考，而不是僅僅復制該變數的初始值。捕獲一個參考保證了當`makeIncrementor`結束時候並不會消失，也保證了當下一次執行`incrementor`函式時，`runningTotal`可以繼續增加。

> 注意：  
> Swift 會決定捕獲參考還是拷貝值。  
> 你不需要標注`amount`或者`runningTotal`來宣告在嵌入的`incrementor`函式中的使用方式。  
> Swift 同時也處理`runingTotal`變數的內存管理操作，如果不再被`incrementor`函式使用，則會被清除。  

下面程式碼為一個使用`makeIncrementor`的範例：

```swift
let incrementByTen = makeIncrementor(forIncrement: 10)
```

該範例定義了一個叫做`incrementByTen`的常數，該常數指向一個每次呼叫會加10的`incrementor`函式。
呼叫這個函式多次可以得到以下結果：

```swift
incrementByTen()
// 回傳的值為10
incrementByTen()
// 回傳的值為20
incrementByTen()
// 回傳的值為30
```

如果你創建了另一個`incrementor`，其會有一個屬於自己的獨立的`runningTotal`變數的參考。
下面的範例中，`incrementBySevne`捕獲了一個新的`runningTotal`變數，該變數和`incrementByTen`中捕獲的變數沒有任何聯系：

```swift
let incrementBySeven = makeIncrementor(forIncrement: 7)
incrementBySeven()
// 回傳的值為7
incrementByTen()
// 回傳的值為40
```

> 注意：  
> 如果你將閉包賦值給一個類別實例的屬性，並且該閉包通過指向該實例或其成員來捕獲了該實例，你將創建一個在閉包和實例間的強參考環。  
> Swift 使用捕獲列表來打破這種強參考環。更多資訊，請參考 [閉包引起的迴圈強參考](../chapter2/16_Automatic_Reference_Counting.html#strong_reference_cycles_for_closures)。  

<a name="closures_are_reference_types"></a>
## 閉包是參考型別（Closures Are Reference Types）

上面的範例中，`incrementBySeven`和`incrementByTen`是常數，但是這些常數指向的閉包仍然可以增加其捕獲的變數值。
這是因為函式和閉包都是參考型別。

無論你將函式/閉包賦值給一個常數還是變數，你實際上都是將常數/變數的值設置為對應函式/閉包的參考。
上面的範例中，`incrementByTen`指向閉包的參考是一個常數，而並非閉包內容本身。

這也意味著如果你將閉包賦值給了兩個不同的常數/變數，兩個值都會指向同一個閉包：

```swift
let alsoIncrementByTen = incrementByTen
alsoIncrementByTen()
// 回傳的值為50
```
