> 翻譯：[honghaoz](https://github.com/honghaoz)
> 校對：[LunaticM](https://github.com/LunaticM)

# 函式（Functions）
-----------------

本頁包含內容：

- [函式定義與呼叫（Defining and Calling Functions）](#Defining_and_Calling_Functions)
- [函式參數與回傳值（Function Parameters and Return Values）](#Function_Parameters_and_Return_Values)
- [函式參數名稱（Function Parameter Names）](#Function_Parameter_Names)
- [函式型別（Function Types）](#Function_Types)
- [函式嵌套（Nested Functions）](#Nested_Functions)

函式是用來完成特定任務的獨立的程式碼塊。你給一個函式起一個合適的名字，用來標示函式做什麼，並且當函式需要執行的時候，這個名字會被“呼叫”。

Swift 統一的函式語法足夠靈活，可以用來表示任何函式，包括從最簡單的沒有參數名字的 C 風格函式，到複雜的帶局部和外部參數名的 Objective-C 風格函式。參數可以提供預設值，以簡化函式呼叫。參數也可以既當做傳入參數，也當做傳出參數，也就是說，一旦函式執行結束，傳入的參數值可以被修改。

在 Swift 中，每個函式都有一種型別，包括函式的參數值型別和回傳值型別。你可以把函式型別當做任何其他普通變數型別一樣處理，這樣就可以更簡單地把函式當做別的函式的參數，也可以從其他函式中回傳函式。函式的定義可以寫在在其他函式定義中，這樣可以在嵌套函式範圍內實作功能封裝。

<a name="Defining_and_Calling_Functions"></a>
## 函式的定義與呼叫（Defining and Calling Functions）

當你定義一個函式時，你可以定義一個或多個有名字和型別的值，作為函式的輸入（稱為參數，parameters），也可以定義某種型別的值作為函式執行結束的輸出（稱為回傳型別）。

每個函式有個函式名，用來描述函式執行的任務。要使用一個函式時，你用函式名“呼叫”，並傳給它匹配的輸入值（稱作實參，arguments）。一個函式的實參必須與函式參數表裡參數的順序一致。

在下面範例中的函式叫做`"greetingForPerson"`，之所以叫這個名字是因為這個函式用一個人的名字當做輸入，並回傳給這個人的問候語。為了完成這個任務，你定義一個輸入參數-一個叫做 `personName` 的 `String` 值，和一個包含給這個人問候語的 `String` 型別的回傳值：

```swift
func sayHello(personName: String) -> String {
    let greeting = "Hello, " + personName + "!"
    return greeting
}
```

所有的這些資訊彙總起來成為函式的定義，並以 `func` 作為前綴。指定函式回傳型別時，用回傳箭頭 `->`（一個連字元後跟一個右角括號）後跟回傳型別的名稱的方式來表示。

該定義描述了函式做什麼，它期望接收什麼和執行結束時它回傳的結果是什麼。這樣的定義使的函式可以在別的地方以一種清晰的方式被呼叫：

```swift
println(sayHello("Anna"))
// prints "Hello, Anna!"
println(sayHello("Brian"))
// prints "Hello, Brian!
```

呼叫 `sayHello` 函式時，在圓括號中傳給它一個 `String` 型別的實參。因為這個函式回傳一個 `String` 型別的值，`sayHello` 可以被包含在 `println` 的呼叫中，用來輸出這個函式的回傳值，正如上面所示。

在 `sayHello` 的函式體中，先定義了一個新的名為 `greeting` 的 `String` 常數，同時賦值了給 `personName` 的一個簡單問候消息。然後用 `return` 關鍵字把這個問候回傳出去。一旦 `return greeting` 被呼叫，該函式結束它的執行並回傳 `greeting` 的當前值。

你可以用不同的輸入值多次呼叫 `sayHello`。上面的範例展示的是用`"Anna"`和`"Brian"`呼叫的結果，該函式分別回傳了不同的結果。

為了簡化這個函式的定義，可以將問候消息的創建和回傳寫成一句：

```swift
func sayHelloAgain(personName: String) -> String {
    return "Hello again, " + personName + "!"
}
println(sayHelloAgain("Anna"))
// prints "Hello again, Anna!
```

<a name="Function_Parameters_and_Return_Values"></a>
## 函式參數與回傳值（Function Parameters and Return Values）

函式參數與回傳值在Swift中極為靈活。你可以定義任何型別的函式，包括從只帶一個未名參數的簡單函式到複雜的帶有表達性參數名和不同參數選項的複雜函式。

### 多重輸入參數（Multiple Input Parameters）

函式可以有多個輸入參數，寫在圓括號中，用逗號分隔。

下面這個函式用一個半開區間的開始點和結束點，計算出這個範圍內包含多少數字：

```swift
func halfOpenRangeLength(start: Int, end: Int) -> Int {
    return end - start
}
println(halfOpenRangeLength(1, 10))
// prints "9
```

### 無參數函式（Functions Without Parameters）

函式可以沒有參數。下面這個函式就是一個無參函式，當被呼叫時，它回傳固定的 `String` 消息：

```swift
func sayHelloWorld() -> String {
    return "hello, world"
}
println(sayHelloWorld())
// prints "hello, world
```

儘管這個函式沒有參數，但是定義中在函式名後還是需要一對圓括號。當被呼叫時，也需要在函式名後寫一對圓括號。

### 無回傳值函式（Functions Without Return Values）

函式可以沒有回傳值。下面是 `sayHello` 函式的另一個版本，叫 `waveGoodbye`，這個函式直接輸出 `String` 值，而不是回傳它：

```swift
func sayGoodbye(personName: String) {
    println("Goodbye, \(personName)!")
}
sayGoodbye("Dave")
// prints "Goodbye, Dave!
```

因為這個函式不需要回傳值，所以這個函式的定義中沒有回傳箭頭（->）和回傳型別。

> 注意：  
> 嚴格上來說，雖然沒有回傳值被定義，`sayGoodbye` 函式依然回傳了值。沒有定義回傳型別的函式會回傳特殊的值，叫 `Void`。它其實是一個空的元組（tuple），沒有任何元素，可以寫成`()`。  

被呼叫時，一個函式的回傳值可以被忽略：

```swift
func printAndCount(stringToPrint: String) -> Int {
    println(stringToPrint)
    return countElements(stringToPrint)
}
func printWithoutCounting(stringToPrint: String) {
    printAndCount(stringToPrint)
}
printAndCount("hello, world")
// prints "hello, world" and returns a value of 12
printWithoutCounting("hello, world")
// prints "hello, world" but does not return a value
```

第一個函式 `printAndCount`，輸出一個字串並回傳 `Int` 型別的字元數。第二個函式 `printWithoutCounting`呼叫了第一個函式，但是忽略了它的回傳值。當第二個函式被呼叫時，消息依然會由第一個函式輸出，但是回傳值不會被用到。

> 注意：  
> 回傳值可以被忽略，但定義了有回傳值的函式必須回傳一個值，如果在函式定義底部沒有回傳任何值，這叫導致編譯錯誤（compile-time error）。  

### 多重回傳值函式（Functions with Multiple Return Values）

你可以用元組（tuple）型別讓多個值作為一個複合值從函式中回傳。

下面的這個範例中，`count` 函式用來計算一個字串中元音，輔音和其他字母的個數（基於美式英語的標準）。

```swift
func count(string: String) -> (vowels: Int, consonants: Int, others: Int) {
    var vowels = 0, consonants = 0, others = 0
    for character in string {
        switch String(character).lowercaseString {
        case "a", "e", "i", "o", "u":
            ++vowels
        case "b", "c", "d", "f", "g", "h", "j", "k", "l", "m",
          "n", "p", "q", "r", "s", "t", "v", "w", "x", "y", "z":
            ++consonants
        default:
            ++others
        }
    }
    return (vowels, consonants, others)
}
```

你可以用 `count` 函式來處理任何一個字串，回傳的值將是一個包含三個 `Int` 型值的元組（tuple）：

```swift
let total = count("some arbitrary string!")
println("\(total.vowels) vowels and \(total.consonants) consonants")
// prints "6 vowels and 13 consonants
```

需要注意的是，元組的成員不需要在函式中回傳時命名，因為它們的名字已經在函式回傳型別有有了定義。

<a name="Function_Parameter_Names"></a>
## 函式參數名稱（Function Parameter Names）

以上所有的函式都給它們的參數定義了`參數名（parameter name）`：

```swift
func someFunction(parameterName: Int) {
    // function body goes here, and can use parameterName
    // to refer to the argument value for that parameter
}
```

但是，這些參數名僅在函式體中使用，不能在函式呼叫時使用。這種型別的參數名被稱作`局部參數名（local parameter name）`，因為它們只能在函式體中使用。

### 外部參數名（External Parameter Names）

有時候，呼叫函式時，給每個參數命名是非常有用的，因為這些參數名可以指出各個實參的用途是什麼。

如果你希望函式的使用者在呼叫函式時提供參數名字，那就需要給每個參數除了局部參數名外再定義一個`外部參數名`。外部參數名寫在局部參數名之前，用空格分隔。

```swift
func someFunction(externalParameterName localParameterName: Int) {
    // function body goes here, and can use localParameterName
    // to refer to the argument value for that parameter
}
```

> 注意：  
> 如果你提供了外部參數名，那麼函式在被呼叫時，必須使用外部參數名。  

以下是個範例，這個函式使用一個`結合者（joiner）`把兩個字串聯在一起：

```swift
func join(s1: String, s2: String, joiner: String) -> String {
    return s1 + joiner + s2
}
```

當你呼叫這個函式時，這三個字串的用途是不清楚的：

```swift
join("hello", "world", ", ")
// returns "hello, world
```

為了讓這些字串的用途更為明顯，我們為 `join` 函式添加外部參數名：

```swift
func join(string s1: String, toString s2: String, withJoiner joiner: String) -> String {
    return s1 + joiner + s2
}
```

在這個版本的 `join` 函式中，第一個參數有一個叫 `string` 的外部參數名和 `s1` 的局部參數名，第二個參數有一個叫 `toString` 的外部參數名和 `s2` 的局部參數名，第三個參數有一個叫 `withJoiner` 的外部參數名和 `joiner` 的局部參數名。

現在，你可以使用這些外部參數名以一種清晰地方式來呼叫函式了：

```swift
join(string: "hello", toString: "world", withJoiner: ", ")
// returns "hello, world
```

使用外部參數名讓第二個版本的 `join` 函式的呼叫更為有表現力，更為通順，同時還保持了函式體是可讀的和有明確意圖的。

> 注意：  
> 當其他人在第一次讀你的程式碼，函式參數的意圖顯得不明顯時，考慮使用外部參數名。如果函式參數名的意圖是很明顯的，那就不需要定義外部參數名了。  

### 簡寫外部參數名（Shorthand External Parameter Names）

如果你需要提供外部參數名，但是局部參數名已經定義好了，那麼你不需要寫兩次這些參數名。相反，只寫一次參數名，並用`井號（#）`作為前綴就可以了。這告訴 Swift 使用這個參數名作為局部和外部參數名。

下面這個範例定義了一個叫 `containsCharacter` 的函式，使用`井號（#）`的方式定義了外部參數名：

```swift
func containsCharacter(#string: String, #characterToFind: Character) -> Bool {
    for character in string {
        if character == characterToFind {
            return true
        }
    }
    return false
}
```

這樣定義參數名，使得函式體更為可讀，清晰，同時也可以以一個不含糊的方式被呼叫：

```swift
let containsAVee = containsCharacter(string: "aardvark", characterToFind: "v")
// containsAVee equals true, because "aardvark" contains a "v”
```

### 預設參數值（Default Parameter Values）

你可以在函式體中為每個參數定義`預設值`。當預設值被定義後，呼叫這個函式時可以略去這個參數。

> 注意：  
> 將帶有預設值的參數放在函式參數表的最後。這樣可以保證在函式呼叫時，非預設參數的順序是一致的，同時使得相同的函式在不同情況下呼叫時顯得更為清晰。  

以下是另一個版本的`join`函式，其中`joiner`有了預設參數值：

```swift
func join(string s1: String, toString s2: String, withJoiner joiner: String = " ") -> String {
    return s1 + joiner + s2
}
```

像第一個版本的 `join` 函式一樣，如果 `joiner` 被賦值時，函式將使用這個字串值來連接兩個字串：

```swift
join(string: "hello", toString: "world", withJoiner: "-")
// returns "hello-world
```

當這個函式被呼叫時，如果 `joiner` 的值沒有被指定，函式會使用預設值（" "）：

```swift
join(string: "hello", toString:"world")
// returns "hello world"
```

### 預設值參數的外部參數名（External Names for Parameters with Default Values）

在大多數情況下，給帶預設值的參數起一個外部參數名是很有用的。這樣可以保證當函式被呼叫且帶預設值的參數被提供值時，參數的意圖是明顯的。

為了使定義外部參數名更加簡單，當你未給帶預設值的參數提供外部參數名時，Swift 會自動提供外部名字。此時外部參數名與局部名字是一樣的，就像你已經在局部參數名前寫了`井號（#）`一樣。

下面是 `join` 函式的另一個版本，這個版本中並沒有為它的參數提供外部參數名，但是 `joiner` 參數依然有外部參數名：

```swift
func join(s1: String, s2: String, joiner: String = " ") -> String {
    return s1 + joiner + s2
}
```

在這個範例中，Swift 自動為 `joiner` 提供了外部參數名。因此，當函式呼叫時，外部參數名必須使用，這樣使得參數的用途變得清晰。

```swift
join("hello", "world", joiner: "-")
// returns "hello-world"
```

> 注意：  
> 你可以使用`下劃線（_）`作為預設值參數的外部參數名，這樣可以在呼叫時不用提供外部參數名。但是給帶預設值的參數命名總是更加合適的。  

### 可變參數（Variadic Parameters）

一個`可變參數（variadic parameter）`可以接受一個或多個值。函式呼叫時，你可以用可變參數來傳入不確定數量的輸入參數。通過在變數型別名後面加入`（...）`的方式來定義可變參數。

傳入可變參數的值在函式體內當做這個型別的一個陣列。例如，一個叫做 `numbers` 的 `Double...` 型可變參數，在函式體內可以當做一個叫 `numbers` 的 `Double[]` 型的陣列常數。

下面的這個函式用來計算一組任意長度數字的算術平均數：

```swift
func arithmeticMean(numbers: Double...) -> Double {
    var total: Double = 0
    for number in numbers {
        total += number
    }
    return total / Double(numbers.count)
}
arithmeticMean(1, 2, 3, 4, 5)
// returns 3.0, which is the arithmetic mean of these five numbers
arithmeticMean(3, 8, 19)
// returns 10.0, which is the arithmetic mean of these three numbers
```

> 注意：  
> 一個函式至多能有一個可變參數，而且它必須是參數表中最後的一個。這樣做是為了避免函式呼叫時出現歧義。  

如果函式有一個或多個帶預設值的參數，而且還有一個可變參數，那麼把可變參數放在參數表的最後。

### 常數參數和變數參數（Constant and Variable Parameters）

函式參數預設是常數。試圖在函式體中更改參數值將會導致編譯錯誤。這意味著你不能錯誤地更改參數值。

但是，有時候，如果函式中有傳入參數的變數值副本將是很有用的。你可以通過指定一個或多個參數為變數參數，從而避免自己在函式中定義新的變數。變數參數不是常數，你可以在函式中把它當做新的可修改副本來使用。

通過在參數名前加關鍵字 `var` 來定義變數參數：

```swift
func alignRight(var string: String, count: Int, pad: Character) -> String {
    let amountToPad = count - countElements(string)
    for _ in 1...amountToPad {
        string = pad + string
    }
    return string
}
let originalString = "hello"
let paddedString = alignRight(originalString, 10, "-")
// paddedString is equal to "-----hello"
// originalString is still equal to "hello"
```

這個範例中定義了一個新的叫做 `alignRight` 的函式，用來右對齊輸入的字串到一個長的輸出字串中。左側空余的地方用指定的填充字元填充。這個範例中，字串`"hello"`被轉換成了`"-----hello"`。

`alignRight` 函式將參數 `string` 定義為變數參數。這意味著 `string` 現在可以作為一個局部變數，用傳入的字串值初始化，並且可以在函式體中進行操作。

該函式首先計算出多少個字元需要被添加到 `string` 的左邊，以右對齊到總的字串中。這個值存在局部常數 `amountToPad` 中。這個函式然後將 `amountToPad` 多的填充（pad）字元填充到 `string` 左邊，並回傳結果。它使用了 `string` 這個變數參數來進行所有字串操作。

> 注意：  
> 對變數參數所進行的修改在函式呼叫結束後便消失了，並且對於函式體外是不可見的。變數參數僅僅存在於函式呼叫的生命周期中。  

### 輸入輸出參數（In-Out Parameters）

變數參數，正如上面所述，僅僅能在函式體內被更改。如果你想要一個函式可以修改參數的值，並且想要在這些修改在函式呼叫結束後仍然存在，那麼就應該把這個參數定義為輸入輸出參數（In-Out Parameters）。

定義一個輸入輸出參數時，在參數定義前加 `inout` 關鍵字。一個輸入輸出參數有傳入函式的值，這個值被函式修改，然後被傳出函式，替換原來的值。

你只能傳入一個變數作為輸入輸出參數。你不能傳入常數或者字面量（literal value），因為這些量是不能被修改的。當傳入的參數作為輸入輸出參數時，需要在參數前加`&`符，表示這個值可以被函式修改。

> 注意：  
> 輸入輸出參數不能有預設值，而且可變參數不能用 `inout` 標記。如果你用 `inout` 標記一個參數，這個參數不能被 `var` 或者 `let` 標記。  

下面是範例，`swapTwoInts` 函式，有兩個分別叫做 `a` 和 `b` 的輸出輸出參數：

```swift
func swapTwoInts(inout a: Int, inout b: Int) {
    let temporaryA = a
    a = b
    b = temporaryA
}
```

這個 `swapTwoInts` 函式僅僅交換 `a` 與 `b` 的值。該函式先將 `a` 的值存到一個暫時常數 `temporaryA` 中，然後將 `b` 的值賦給 `a`，最後將 `temporaryA` 幅值給 `b`。

你可以用兩個 `Int` 型的變數來呼叫 `swapTwoInts`。需要注意的是，`someInt` 和 `anotherInt` 在傳入 `swapTwoInts` 函式前，都加了 `&` 的前綴：

```swift
var someInt = 3
var anotherInt = 107
swapTwoInts(&someInt, &anotherInt)
println("someInt is now \(someInt), and anotherInt is now \(anotherInt)")
// prints "someInt is now 107, and anotherInt is now 3”
```

從上面這個範例中，我們可以看到 `someInt` 和 `anotherInt` 的原始值在 `swapTwoInts` 函式中被修改，儘管它們的定義在函式體外。

> 注意：  
> 輸出輸出參數和回傳值是不一樣的。上面的 `swapTwoInts` 函式並沒有定義任何回傳值，但仍然修改了 `someInt` 和 `anotherInt` 的值。輸入輸出參數是函式對函式體外產生影響的另一種方式。  

<a name="Function_Types"></a>
## 函式型別（Function Types）

每個函式都有種特定的函式型別，由函式的參數型別和回傳型別組成。

例如：

```swift
func addTwoInts(a: Int, b: Int) -> Int {
    return a + b
}
func multiplyTwoInts(a: Int, b: Int) -> Int {
    return a * b
}
```

這個範例中定義了兩個簡單的數學函式：`addTwoInts` 和 `multiplyTwoInts`。這兩個函式都傳入兩個 `Int` 型別， 回傳一個合適的`Int`值。

這兩個函式的型別是 `(Int, Int) -> Int`，可以讀作“這個函式型別，它有兩個 `Int` 型的參數並回傳一個 `Int` 型的值。”。

下面是另一個範例，一個沒有參數，也沒有回傳值的函式：

```swift
func printHelloWorld() {
    println("hello, world")
}
```

這個函式的型別是：`() -> ()`，或者叫“沒有參數，並回傳 `Void` 型別的函式。”。沒有指定回傳型別的函式總回傳 `Void`。在Swift中，`Void` 與空的元組是一樣的。

### 使用函式型別（Using Function Types）

在 Swift 中，使用函式型別就像使用其他型別一樣。例如，你可以定義一個型別為函式的常數或變數，並將函式賦值給它：

```swift
var mathFunction: (Int, Int) -> Int = addTwoInts
```

這個可以讀作：

“定義一個叫做 `mathFunction` 的變數，型別是‘一個有兩個 `Int` 型的參數並回傳一個 `Int` 型的值的函式’，並讓這個新變數指向 `addTwoInts` 函式”。

`addTwoInts` 和 `mathFunction` 有同樣的型別，所以這個賦值過程在 Swift 型別檢查中是允許的。

現在，你可以用 `mathFunction` 來呼叫被賦值的函式了：

```swift
println("Result: \(mathFunction(2, 3))")
// prints "Result: 5
```

有相同匹配型別的不同函式可以被賦值給同一個變數，就像非函式型別的變數一樣：

```swift
mathFunction = multiplyTwoInts
println("Result: \(mathFunction(2, 3))")
// prints "Result: 6"
```

就像其他型別一樣，當賦值一個函式給常數或變數時，你可以讓 Swift 來推斷其函式型別：

```swift
let anotherMathFunction = addTwoInts
// anotherMathFunction is inferred to be of type (Int, Int) -> Int
```

### 函式型別作為參數型別（Function Types as Parameter Types）

你可以用`(Int, Int) -> Int`這樣的函式型別作為另一個函式的參數型別。這樣你可以將函式的一部分實作交由給函式的呼叫者。

下面是另一個範例，正如上面的函式一樣，同樣是輸出某種數學運算結果：

```swift
func printMathResult(mathFunction: (Int, Int) -> Int, a: Int, b: Int) {
    println("Result: \(mathFunction(a, b))")
}
printMathResult(addTwoInts, 3, 5)
// prints "Result: 8”
```

這個範例定義了 `printMathResult` 函式，它有三個參數：第一個參數叫 `mathFunction`，型別是`(Int, Int) -> Int`，你可以傳入任何這種型別的函式；第二個和第三個參數叫 `a` 和 `b`，它們的型別都是 `Int`，這兩個值作為已給的函式的輸入值。

當 `printMathResult` 被呼叫時，它被傳入 `addTwoInts` 函式和整數`3`和`5`。它用傳入`3`和`5`呼叫 `addTwoInts`，並輸出結果：`8`。

`printMathResult` 函式的作用就是輸出另一個合適型別的數學函式的呼叫結果。它不關心傳入函式是如何實作的，它只關心這個傳入的函式型別是正確的。這使得 `printMathResult` 可以以一種型別安全（type-safe）的方式來保證傳入函式的呼叫是正確的。

### 函式型別作為回傳型別（Function Type as Return Types）

你可以用函式型別作為另一個函式的回傳型別。你需要做的是在回傳箭頭（`->`）後寫一個完整的函式型別。

下面的這個範例中定義了兩個簡單函式，分別是 `stepForward` 和`stepBackward`。`stepForward` 函式回傳一個比輸入值大一的值。`stepBackward` 函式回傳一個比輸入值小一的值。這兩個函式的型別都是 `(Int) -> Int`：

```swift
func stepForward(input: Int) -> Int {
    return input + 1
}
func stepBackward(input: Int) -> Int {
    return input - 1
}
```

下面這個叫做 `chooseStepFunction` 的函式，它的回傳型別是 `(Int) -> Int` 的函式。`chooseStepFunction` 根據布林值 `backwards` 來回傳 `stepForward` 函式或 `stepBackward` 函式：

```swift
func chooseStepFunction(backwards: Bool) -> (Int) -> Int {
    return backwards ? stepBackward : stepForward
}
```

你現在可以用 `chooseStepFunction` 來獲得一個函式，不管是那個方向：

```swift
var currentValue = 3
let moveNearerToZero = chooseStepFunction(currentValue > 0)
// moveNearerToZero now refers to the stepBackward() function
```

上面這個範例中計算出從 `currentValue` 逐漸接近到`0`是需要向正數走還是向負數走。`currentValue` 的初始值是`3`，這意味著 `currentValue > 0` 是真的（`true`），這將使得 `chooseStepFunction` 回傳 `stepBackward` 函式。一個指向回傳的函式的參考保存在了 `moveNearerToZero` 常數中。

現在，`moveNearerToZero` 指向了正確的函式，它可以被用來數到`0`：

```swift
println("Counting to zero:")
// Counting to zero:
while currentValue != 0 {
    println("\(currentValue)... ")
    currentValue = moveNearerToZero(currentValue)
}
println("zero!")
// 3...
// 2...
// 1...
// zero!
```

<a name="Nested_Functions"></a>
## 嵌套函式（Nested Functions）

這章中你所見到的所有函式都叫全域函式（global functions），它們定義在全域域中。你也可以把函式定義在別的函式體中，稱作嵌套函式（nested functions）。

預設情況下，嵌套函式是對外界不可見的，但是可以被他們封閉函式（enclosing function）來呼叫。一個封閉函式也可以回傳它的某一個嵌套函式，使得這個函式可以在其他域中被使用。

你可以用回傳嵌套函式的方式重寫 `chooseStepFunction` 函式：

```swift
func chooseStepFunction(backwards: Bool) -> (Int) -> Int {
    func stepForward(input: Int) -> Int { return input + 1 }
    func stepBackward(input: Int) -> Int { return input - 1 }
    return backwards ? stepBackward : stepForward
}
var currentValue = -4
let moveNearerToZero = chooseStepFunction(currentValue > 0)
// moveNearerToZero now refers to the nested stepForward() function
while currentValue != 0 {
    println("\(currentValue)... ")
    currentValue = moveNearerToZero(currentValue)
}
println("zero!")
// -4...
// -3...
// -2...
// -1...
// zero!
```
