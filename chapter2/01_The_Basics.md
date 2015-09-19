> 翻譯：[tommy60703](https://github.com/tommy60703)
> 校對：[rocooshiang](https://github.com/rocooshiang)

# 基礎部分
-----------------

本頁包含內容：

- [常數和變數](#constants_and_variables)
- [註解](#comments)
- [分號](#semicolons)
- [整數](#integers)
- [浮點數](#floating-point_numbers)
- [型別安全和型別推斷](#type_safety_and_type_inference)
- [數值型字面量](#numeric_literals)
- [數值型別轉換](#numeric_type_conversion)
- [型別別名](#type_aliases)
- [布林值](#booleans)
- [Tuples](#tuples)
- [Optionals](#optionals)
- [Assertions](#assertions)

Swift 是開發 iOS 和 OS X 應用程式的一門新語言。然而，如果你有 C 或者 Objective-C 開發經驗的話，你會發現 Swift 的很多內容都是你熟悉的。

Swift 的型別是在 C 和 Objective-C 的基礎上提出的，`Int`是整數；`Double`和`Float`是浮點數；`Bool`是布林值；`String`是字串。Swift 還有兩個有用的集合型別，`Array`和`Dictionary`，請參考[集合型別](04_Collection_Types.html)。

就像 C 語言一樣，Swift 使用變數來進行儲存並透過變數名稱來參考值。在 Swift 中，值不可變的變數有著廣泛的應用，它們就是常數，而且比 C 語言的常數更強大。在 Swift 中，如果你要處理的值不需要改變，那使用常數可以讓你的程式碼更加安全並且更好地表達你的意圖。

除了我們熟悉的型別，Swift 還增加了 Objective-C 中沒有的型別比如 tuple。Tuple 可以讓你創建或者傳遞一組資料，比如作為函式的回傳值時，你可以用一個 tuple 回傳多個值。

Swift 還增加了 optional 型別，用於處理值不存在的情況。optional 表示 「那兒有一個值，並且它等於 x 」 或者 「那兒沒有值」。optional 有點像在 Objective-C 中使用`nil`，但是它可以用在任何型別上，不僅僅是類別。optional 型別比 Objective-C 中的`nil`指標更加安全也更具表現力，它是 Swift 許多強大特性的重要組成部分。

Swift 是一個型別安全的語言，optional 就是一個很好的範例。Swift 可以讓你清楚地知道值的型別。如果你的程式碼期望得到一個`String`，型別安全會阻止你不小心傳入一個`Int`。你可以在開發階段盡早發現並修正錯誤。

<a name="constants_and_variables"></a>
## 常數和變數

常數和變數把一個名字（比如`maximumNumberOfLoginAttempts`或者`welcomeMessage`）和一個指定型別的值（比如數字`10`或者字串`"Hello"`）關聯起來。常數的值一旦設定就不能改變，而變數的值可以隨意更改。

### 宣告常數和變數

常數和變數必須在使用前宣告，用`let`來宣告常數，用`var`來宣告變數。下面的範例展示了如何用常數和變數來記錄使用者嘗試登錄的次數：

```swift
let maximumNumberOfLoginAttempts = 10
var currentLoginAttempt = 0
```

這兩行程式碼可以被理解為：

「宣告一個新常數叫`maximumNumberOfLoginAttempts`，並給它一個值`10`。然後，宣告一個變數是`currentLoginAttempt`並將它的值初始化為`0`。」

在這個範例中，允許的最大嘗試登錄次數被宣告為一個常數，因為這個值不會改變。目前嘗試登錄次數被宣告為一個變數，因為每次嘗試登錄失敗的時候都需要增加這個值。

你可以在一行中宣告多個常數或者多個變數，用逗號隔開：

```swift
var x = 0.0, y = 0.0, z = 0.0
```

>注意：
如果你的程式碼中有不需要改變的值，請使用`let`關鍵字將它宣告為常數。只將需要改變的值宣告為變數。

### 型別標注

當你宣告常數或者變數的時候可以加上_型別標注（type annotation）_，說明常數或者變數中要儲存的值的型別。如果要添加型別標注，需要在常數或者變數名後面加上一個冒號和空格，然後加上型別名稱。

這個範例給`welcomeMessage`變數添加了型別標注，表示這個變數可以儲存`String`型別的值：

```swift
var welcomeMessage: String
```

宣告中的冒號代表著「是...型別」，所以這行程式碼可以被理解為：

「宣告一個型別為`String`，名字為`welcomeMessage`的變數。」

「型別為`String`」的意思是「可以儲存任意`String`型別的值。」
可以把他想成是「這種型別的東西」或是「這種類型的東西」可以被儲存。

`welcomeMessage`變數現在可以被設置成任意字串：

```swift
welcomeMessage = "Hello"
```

你可以在同一行定義定義多個相同行別的變數，並用逗號隔開，最後加上型別標注。

```swift
var red, green, blue: Double
```

> 注意：
一般來說你很少需要寫型別標注。如果你在宣告常數或者變數的時候指派了一個初始值，Swift可以推斷出這個常數或者變數的型別，請參考[型別安全和型別推斷](#type_safety_and_type_inference)。在上面的範例中，沒有給`welcomeMessage`指派初始值，所以變數`welcomeMessage`的型別是透過一個型別標注指定的，而不是透過初始值推斷的。

### 常數和變數的命名

你可以用任何你喜歡的字元作為常數和變數名，包括 Unicode 字元：

```swift
let π = 3.14159
let 你好 = "你好世界"
let 🐶🐮 = "dogcow"
```

常數與變數名不能包含數學符號、箭頭、保留的（或者非法的）Unicode 碼位、連線與制表字元（box-drawing characters），也不能以數字開頭，但是可以在常數與變數名的其他地方包含數字。

一旦你將常數或者變數宣告為確定的型別，你就不能使用相同的名字再次進行宣告，或者改變其儲存的值的型別。同時，你也不能將常數與變數進行互轉。

> 注意：
如果你需要使用與 Swift 保留關鍵字相同的名稱作為常數或者變數名，你可以使用反引號（`）將關鍵字包圍的方式將其作為名字使用。無論如何，你應當避免使用關鍵字作為常數或變數名，除非你別無選擇。

你可以更改現有的變數值為其他同型別的值，在下面的範例中，`friendlyWelcome`的值從`"Hello!"`改為了`"Bonjour!"`:

```swift
var friendlyWelcome = "Hello!"
friendlyWelcome = "Bonjour!"
// friendlyWelcome 現在是 "Bonjour!"
```

與變數不同，常數的值一旦被確定就不能更改了。嘗試這樣做會導致編譯時報錯：

```swift
let languageName = "Swift"
languageName = "Swift++"
// 這會報編譯時錯誤 - languageName 不能被改變
```

### 輸出常數和變數

你可以用`println`函式來輸出目前常數或變數的值:

```swift
println(friendlyWelcome)
// 輸出 "Bonjour!"
```

`println`是一個用來輸出的全域函式，輸出的內容會在最後換行。如果你用 Xcode，`println`將會輸出內容到「console」面板上。(另一種函式叫`print`，唯一區別是在輸出內容最後不會換行。)

`println`函式輸出傳入的`String`值：

```swift
println("This is a string")
// 輸出 "This is a string"
```

與 Cocoa 裡的`NSLog`函式類似的是，`println`函式可以輸出更複雜的訊息。這些訊息可以包含目前常數和變數的值。

Swift 用_字串插值（string interpolation）_的方式把常數名或者變數名當做占位符（placeholder）加入到長字串中，Swift 會用目前常數或變數的值替換這些占位符。將常數或變數名放入圓括號中，並在開括號前使用反斜線將其跳脫：

```swift
println("The current value of friendlyWelcome is \(friendlyWelcome)")
// 輸出 "The current value of friendlyWelcome is Bonjour!
```

> 注意：
字串插值所有可用的選項，請參考[字串插值](03_Strings_and_Characters.html#string_interpolation)。

<a name="comments"></a>
## 註解
請將你的程式碼中的非執行文字註解成提示或者筆記以方便你將來閱讀。Swift 的編譯器將會在編譯程式碼時自動忽略掉註解部分。

Swift 中的註解與 C 語言的註解非常相似。單行註解以雙正斜線（`//`）作為起始標記:

```swift
// 這是一個註解
```

你也可以進行多行註解，其起始標記為單個正斜線後跟隨一個星號（`/*`），終止標記為一個星號後跟隨單個正斜線（`*/`）:

```swift
/* 這是一個,
多行註解 */
```

與 C 語言多行註解不同，Swift 的多行註解可以巢狀地寫在其它的多行註解之中。你可以先寫一個多行註解區塊，然後在這個註解區塊之中再套入第二個多行註解。終止註解時先插入第二個註解區塊的終止標記，然後再插入第一個註解區塊的終止標記：

```swift
/* 這是第一個多行註解的開頭
/* 這是第二個被套入的多行註解 */
這是第一個多行註解的結尾 */
```

透過運用巢狀多行註解，你可以快速方便的註解掉一大段程式碼，即使這段程式碼之中已經含有了多行註解區塊。

<a name="semicolons"></a>
## 分號
與其他大部分程式語言不同，Swift 並不強制要求你在每條語句的結尾處使用分號（`;`），當然，你也可以按照你自己的習慣添加分號。有一種情況下必須要用分號，即你打算在同一行內寫多條獨立的語句：

```swift
let cat = "🐱"; println(cat)
// 輸出 "🐱"
```

<a name="integers"></a>
## 整數

整數就是沒有小數部分的數字，比如`42`和`-23`。整數可以是`有號`（正、負、零）或者`無號`（正、零）。

Swift 提供了 8、16、32 和 64 位元的有號和無號整數型別。這些整數型別和 C 語言的命名方式很像，比如 8 位元無號整數型別是`UInt8`，32 位元有號整數型別是`Int32`。就像 Swift 的其他型別一樣，整數型別采用大寫命名法。

### 整數範圍

你可以存取不同整數型別的`min`和`max`屬性來獲取對應型別的最大值和最小值：

```swift
let minValue = UInt8.min // minValue 為 0，且是Uint8型別
let maxValue = UInt8.max // maxValue 為 255，且是UInt8型別
```
`min`和`max`所傳回值的型別，正是其所對應的整數型別(如上UInt8，所傳回的型別為UInt8)，可用在相同型別的陳述式

### Int

一般來說，你不需要特別指定整數的長度。Swift 提供了一個特殊的整數型別`Int`，長度與目前平台的原生字長相同：

* 在 32 位元平台上，`Int`和`Int32`長度相同。
* 在 64 位元平台上，`Int`和`Int64`長度相同。

除非你需要特定長度的整數，一般來說使用`Int`就夠了。這可以提高程式碼一致性和可複用性。即使是在 32 位元平台上，`Int`可以儲存的整數範圍也可以達到`-2147483648`~`2147483647`，大多數時候這已經足夠大了。

### UInt

Swift 也提供了一個特殊的無號型別`UInt`，長度與目前平台的原生字長相同：

* 在 32 位元平台上，`UInt`和`UInt32`長度相同。
* 在 64 位元平台上，`UInt`和`UInt64`長度相同。

> 注意：
>
盡量不要使用`UInt`，除非你真的需要儲存一個和目前平台原生字長相同的無號整數。除了這種情況，最好使用`Int`，即使你要儲存的值已知是非負的。統一使用`Int`可以提高程式碼的可複用性，避免不同型別數字之間的轉換，並且匹配數字的型別推斷，請參考[型別安全和型別推斷](#type_safety_and_type_inference)。

<a name="floating-point_numbers"></a>
## 浮點數

浮點數是有小數部分的數字，比如`3.14159`，`0.1`和`-273.15`。

浮點型別比整數型別表示的範圍更大，可以儲存比`Int`型別更大或者更小的數字。Swift 提供了兩種有號浮點數型別：

* `Double`表示 64 位元浮點數。當你需要儲存很大或者很高精度的浮點數時請使用此型別。
* `Float`表示 32 位元浮點數。精度要求不高的話可以使用此型別。

> 注意：
`Double`精確度很高，至少有 15 位數字，而`Float`最少只有 6 位數字。選擇哪個型別取決於你的程式碼需要處理的值的範圍。

<a name="type_safety_and_type_inference"></a>
## 型別安全和型別推斷

Swift 是一個_型別安全（type safe）_的語言。型別安全的語言可以讓你清楚地知道程式碼要處理的值的型別。如果你的程式碼需要一個`String`，你絕對不可能不小心傳進去一個`Int`。

由於 Swift 是型別安全的，所以它會在編譯你的程式碼時進行_型別檢查（type checks）_，並把不匹配的型別標記為錯誤。這可以讓你在開發的時候盡早發現並修復錯誤。

當你要處理不同型別的值時，型別檢查可以幫你避免錯誤。然而，這並不是說你每次宣告常數和變數的時候都需要顯式指定型別。如果你沒有顯式指定型別，Swift 會使用_型別推斷（type inference）_來選擇合適的型別。有了型別推斷，編譯器可以在編譯程式碼的時候自動推斷出表達式的型別。原理很簡單，只要檢查你指派的值即可。

因為有型別推斷，和 C 或者 Objective-C 比起來 Swift 很少需要宣告型別。常數和變數雖然需要明確型別，但是大部分工作並不需要你自己來完成。

當你宣告常數或者變數並賦初值的時候型別推斷非常有用。當你在宣告常數或者變數的時候賦給它們一個_字面量（literal value 或 literal）_即可觸發型別推斷。（字面量就是會直接出現在你程式碼中的值，比如`42`和`3.14159`。）

例如，如果你給一個新常數指派值為`42`並且沒有標明型別，Swift 可以推斷出常數型別是`Int`，因為你給它指派的初始值看起來像一個整數：

```swift
let meaningOfLife = 42
// meaningOfLife 會被推測為 Int 型別
```

同理，如果你沒有給浮點字面量標明型別，Swift 會推斷你想要的是`Double`：

```swift
let pi = 3.14159
// pi 會被推測為 Double 型別
```

當推斷浮點數的型別時，Swift 總是會選擇`Double`而不是`Float`。

如果表達式中同時出現了整數和浮點數，會被推斷為`Double`型別：

```swift
let anotherPi = 3 + 0.14159
// anotherPi 會被推測為 Double 型別
```

原始值`3`沒有顯式宣告型別，而表達式中出現了一個浮點字面量，所以表達式會被推斷為`Double`型別。

<a name="numeric_literals"></a>
## 數值型字面量

整數字面量可以被寫作：

* 一個十進制數，沒有前綴
* 一個二進制數，前綴是`0b`
* 一個八進制數，前綴是`0o`
* 一個十六進制數，前綴是`0x`

下面的所有整數字面量的十進制值都是`17`:

```swift
let decimalInteger = 17
let binaryInteger = 0b10001       // 二進制的 17
let octalInteger = 0o21           // 八進制的 17
let hexadecimalInteger = 0x11     // 十六進制的 17
```

浮點字面量可以是十進制（沒有前綴）或者是十六進制（前綴是`0x`）。小數點兩邊必須有至少一個十進制數字（或者是十六進制的數字）。浮點字面量還有一個 optional 的_指數（exponent）_，在十進制浮點數中透過大寫或者小寫的`e`來指定，在十六進制浮點數中透過大寫或者小寫的`p`來指定。

如果一個十進制數的指數為`exp`，那這個數相當於基數和 10^exp 的乘積：
* `1.25e2` 表示 1.25 × 10^2，等於 `125.0`。
* `1.25e-2` 表示 1.25 × 10^-2，等於 `0.0125`。

如果一個十六進制數的指數為`exp`，那這個數相當於基數和 2^exp 的乘積：
* `0xFp2` 表示 15 × 2^2，等於 `60.0`。
* `0xFp-2` 表示 15 × 2^-2，等於 `3.75`。

下面的這些浮點字面量都等於十進制的`12.1875`：

```swift
let decimalDouble = 12.1875
let exponentDouble = 1.21875e1
let hexadecimalDouble = 0xC.3p0
```

數值型字面量可以包括額外的格式來增強可讀性。整數和浮點數都可以添加額外的零並且包含底線，並不會影響字面量：

```swift
let paddedDouble = 000123.456
let oneMillion = 1_000_000
let justOverOneMillion = 1_000_000.000_000_1
```

<a name="numeric_type_conversion"></a>
## 數值型型別轉換

通常來講，即使程式碼中的整數常數和變數已知非負，也請使用`Int`型別。總是使用預設的整數型別可以保證你的整數常數和變數可以直接被複用並且可以匹配整數類別字面量的型別推斷。
只有在必要的時候才使用其他整數型別，比如要處理外部的長度明確的資料或者為了優化性能、記憶體占用等等。使用顯式指定長度的型別可以及時發現溢位並且可以暗示正在處理特殊資料。

### 整數轉換

不同整數型別的變數和常數可以儲存不同範圍的數字。`Int8`型別的常數或者變數可以儲存的數字範圍是`-128`~`127`，而`UInt8`型別的常數或者變數能儲存的數字範圍是`0`~`255`。如果數字超出了常數或者變數可儲存的範圍，編譯的時候會報錯：

```swift
let cannotBeNegative: UInt8 = -1
// UInt8 型別不能儲存負數，所以會報錯
let tooBig: Int8 = Int8.max + 1
// Int8 型別不能儲存超過最大值的數，所以會報錯
```

由於每種整數型別都可以儲存不同範圍的值，所以你必須根據不同情況選擇性使用數值型型別轉換。這種選擇性使用的方式，可以預防隱式轉換的錯誤並讓你的程式碼中的型別轉換意圖變得清晰。

要將一種數字型別轉換成另一種，你要用目前值來初始化一個期望型別的新數字，這個數字的型別就是你的目標型別。在下面的範例中，常數`twoThousand`是`UInt16`型別，然而常數`one`是`UInt8`型別。它們不能直接相加，因為它們型別不同。所以要呼叫`UInt16(one)`來創建一個新的`UInt16`數字並用`one`的值來初始化，然後使用這個新數字來計算：

```swift
let twoThousand: UInt16 = 2_000
let one: UInt8 = 1
let twoThousandAndOne = twoThousand + UInt16(one)
```

現在兩個數字的型別都是`UInt16`，可以進行相加。目標常數`twoThousandAndOne`的型別被推斷為`UInt16`，因為它是兩個`UInt16`值的和。

`SomeType(ofInitialValue)`是呼叫 Swift 初始化函式並傳入一個初始值的預設方法。在語言內部，`UInt16`有一個初始化函式，可以接受一個`UInt8`型別的值，所以這個初始化函式可以用現有的`UInt8`來創建一個新的`UInt16`。注意，你並不能傳入任意型別的值，只能傳入`UInt16`內部有對應初始化函式的值。不過你可以擴展現有的型別來讓它可以接收其他型別的值（包括自定義型別），請參考[擴展](20_Extensions.html)。

### 整數和浮點數轉換

整數和浮點數的轉換必須顯式指定型別：

```swift
let three = 3
let pointOneFourOneFiveNine = 0.14159
let pi = Double(three) + pointOneFourOneFiveNine
// pi 等於 3.14159，所以被推測為 Double 型別
```

這個範例中，常數`three`的值被用來創建一個`Double`型別的值，所以加號兩邊的數型別須相同。如果不進行轉換，兩者無法相加。

浮點數到整數的同樣可以反向轉換，整數型別可以用`Double`或者`Float`型別來初始化：

```swift
let integerPi = Int(pi)
// integerPi 等於 3，所以被推測為 Int 型別
```

當用這種方式來初始化一個新的整數值時，浮點值會被截斷。也就是說`4.75`會變成`4`，`-3.9`會變成`-3`。

> 注意：
結合數值型常數和變數不同於結合數值型字面量。字面量`3`可以直接和字面量`0.14159`相加，因為數值字面量本身沒有明確的型別。它們的型別只在編譯器需要求值的時候被推測。

<a name="type_aliases"></a>
## 型別別名

_型別別名（type aliases）_就是給現有型別定義另一個名字。你可以使用`typealias`關鍵字來定義型別別名。

當你想要給現有型別起一個更有意義的名字時，型別別名非常有用。假設你正在處理特定長度的外部資源的資料：

```swift
typealias AudioSample = UInt16
```

定義了一個型別別名之後，你可以在任何使用原始名的地方使用別名：

```swift
var maxAmplitudeFound = AudioSample.min
// maxAmplitudeFound 現在是 0
```

本例中，`AudioSample`被定義為`UInt16`的一個別名。因為它是別名，`AudioSample.min`實際上是`UInt16.min`，所以會給`maxAmplitudeFound`賦一個初值`0`。

<a name="booleans"></a>
## 布林值

Swift 有一個基本的_布林（Boolean）_型別，叫做`Bool`。布林值指_邏輯上的（logical）_，因為它們只能是真或者假。Swift 有兩個布林常數，`true`和`false`：

```swift
let orangesAreOrange = true
let turnipsAreDelicious = false
```

`orangesAreOrange`和`turnipsAreDelicious`的型別會被推斷為`Bool`，因為它們的初值是布林字面量。就像之前提到的`Int`和`Double`一樣，如果你創建變數的時候給它們指派`true`或者`false`，那你不需要將常數或者變數宣告為`Bool`型別。初始化常數或者變數的時候如果所指派的值的型別已知，就可以觸發型別推斷，這讓 Swift 程式碼更加簡潔並且可讀性更高。

當你使用條件語句比如`if`語句的時候，布林值非常有用：

```swift
if turnipsAreDelicious {
    println("Mmm, tasty turnips!")
} else {
    println("Eww, turnips are horrible.")
}
// 輸出 "Eww, turnips are horrible."
```

條件語句，例如`if`，請參考[控制流程](05_Control_Flow.html)。

如果你在需要使用`Bool`型別的地方使用了非布林值，Swift 的型別安全機制會報錯。下面的範例會報告一個編譯時錯誤：

```swift
let i = 1
if i {
    // 這個範例不會透過編譯，會報錯
}
```

然而，下面的範例是合法的：

```swift
let i = 1
if i == 1 {
    // 這個範例會編譯成功
}
```

`i == 1`的比較結果是`Bool`型別，所以第二個範例可以透過型別檢查。類似`i == 1`這樣的比較，請參考[基本運算子](05_Control_Flow.html)。

和 Swift 中的其他型別安全的範例一樣，這個方法可以避免錯誤並保證這塊程式碼的意圖總是清晰的。

<a name="tuples"></a>
## Tuples

_Tuples_把多個值組合成一個複合值。tuple 內的值可以使任意型別，並不要求是相同型別。

下面這個範例中，`(404, "Not Found")`是一個描述 _HTTP 狀態碼（HTTP status code）_的 tuple。HTTP 狀態碼是當你請求網頁的時候 web 伺服器回傳的一個特殊值。如果你請求的網頁不存在就會回傳一個`404 Not Found`狀態碼。

```swift
let http404Error = (404, "Not Found")
// http404Error 的型別是 (Int, String)，值是 (404, "Not Found")
```

`(404, "Not Found")` tuple 把一個`Int`值和一個`String`值組合起來表示 HTTP 狀態碼的兩個部分：一個數字和一個人類別可讀的描述。這個 tuple 可以被描述為「一個型別為`(Int, String)`的 tuple」。

你可以把任意順序的型別組合成一個 tuple，這個 tuple 可以包含所有型別。只要你想，你可以創建一個型別為`(Int, Int, Int)`或者`(String, Bool)`或者其他任何你想要的組合的 tuple。

你可以將一個 tuple 的內容_分解（decompose）_成單獨的常數和變數，然後你就可以正常使用它們了：

```swift
let (statusCode, statusMessage) = http404Error
println("The status code is \(statusCode)")
// 輸出 "The status code is 404"
println("The status message is \(statusMessage)")
// 輸出 "The status message is Not Found"
```

如果你只需要一部分 tuple 值，分解的時候可以把要忽略的部分用底線（`_`）標記：

```swift
let (justTheStatusCode, _) = http404Error
println("The status code is \(justTheStatusCode)")
// 輸出 "The status code is 404"
```

此外，你還可以透過索引值來存取 tuple 中的單個元素，索引值從零開始：

```swift
println("The status code is \(http404Error.0)")
// 輸出 "The status code is 404"
println("The status message is \(http404Error.1)")
// 輸出 "The status message is Not Found"
```

你可以在定義 tuple 的時候給單個元素命名：

```swift
let http200Status = (statusCode: 200, description: "OK")
```

給 tuple 中的元素命名後，你可以透過名字來獲取這些元素的值：

```swift
println("The status code is \(http200Status.statusCode)")
// 輸出 "The status code is 200"
println("The status message is \(http200Status.description)")
// 輸出 "The status message is OK"
```

作為函式回傳值時，tuple 非常有用。一個用來獲取網頁的函式可能會回傳一個`(Int, String)` tuple 來描述是否獲取成功。和只能回傳一個型別的值比較起來，一個包含兩個不同型別的值的 tuple 可以讓函式的回傳訊息更有用。請參考[函式參數與回傳值](06_Functions.html#Function_Parameters_and_Return_Values)。

> 注意：
tuple 在臨時組織值的時候很有用，但是並不適合創建複雜的資料結構。如果你的資料結構並不是臨時使用，請使用類別別或者結構而不是 tuple。請參考[類別別和結構](09_Classes_and_Structures.html)。

<a name="optionals"></a>
## Optionals

使用_optionals_來處理值可能不存在的情況。optionals 型別表示：

* _有_值，等於 x

或者

* _沒有_值

> 注意：
>
C 和 Objective-C 中並沒有 optionals 型別這個概念。最接近的是 Objective-C 中的一個特性，一個方法要不回傳一個物件，要不回傳`nil`，`nil`表示「缺少一個合法的物件」。然而，這只對物件起作用——對於結構，基本的 C 型別或者列舉型別不起作用。對於這些型別，Objective-C 方法一般會回傳一個特殊值（比如`NSNotFound`）來暗示值不存在。這種方法假設方法的呼叫者知道並記得對特殊值進行判斷。然而，Swift 的 optionals 可以讓你暗示_任意型別_的值不存在，並不需要一個特殊值。

來看一個範例。Swift 的`String`型別有一個叫做`toInt`的方法，作用是將一個`String`值轉換成一個`Int`值。然而，並不是所有的字串都可以轉換成一個整數。字串`"123"`可以被轉換成數字`123`，但是字串`"hello, world"`不行。

下面的範例使用`toInt`方法來嘗試將一個`String`轉換成`Int`：

```swift
let possibleNumber = "123"
let convertedNumber = possibleNumber.toInt()
// convertedNumber 被推測為型別 "Int?"， 也就是 "optional Int"
```

因為`toInt`方法可能會失敗，所以它回傳一個_optional_ `Int`，而不是一個`Int`。一個 optional `Int`被寫作`Int?`而不是`Int`。問號暗示包含的值是 optional 型別，也就是說可能包含`Int`值也可能不包含值。（不能包含其他任何值比如`Bool`值或者`String`值。只能是`Int`或者什麼都沒有。）

### if 語句以及強制解析

你可以使用`if`語句透過對比nil的方式來判斷一個 optional 是否包含值。使用「等於」運算子（`==`）或是「不等於」運算子(`!=`)來執行這樣的比較。

如果一個 optional 有值，就會被認為是「不等於」`nil`。

```swift
if convertedNumber != nil {
    println("convertedNumber contains some integer value.")
}
// 輸出 "convertedNumber contains some integer value."
```

當你確定 optional _確實_包含值之後，你可以在 optional 的名字後面加一個感嘆號（`!`）來獲取值。這個驚嘆號表示「我知道這個 optional 有值，請使用它。」這被稱為 optional 值的_強制解析（forced unwrapping）_：

```swift
if convertedNumber != nil {
    println("convertedNumber has an integer value of \(convertedNumber!).")
}
// 輸出 "convertedNumber has an integer value of 123"
```

更多關於`if`語句的內容，請參考[控制流程程](05_Control_Flow.html)。

> 注意：
使用`!`來獲取一個不存在的 optional 值會導致執行時錯誤。使用`!`來強制解析值之前，一定要確定 optional 包含一個非`nil`的值。

<a name="optional_binding"></a>
### Optional 綁定

使用_optional 綁定（optional binding）_來判斷 optional 是否包含值，如果包含就把值指派給一個臨時常數或者變數。Optional 綁定可以用在`if`和`while`語句中來對 optional 的值進行判斷並把值指派給一個常數或者變數。`if`和`while`語句，請參考[控制流程程](05_Control_Flow.html)。

像下面這樣在`if`語句中寫一個 optional 綁定：

```swift
if let constantName = someOptional {
    statements
}
```

你可以像上面這樣使用 optional 綁定來重寫`possibleNumber`這個範例：

```swift
if let actualNumber = possibleNumber.toInt() {
    println("\(possibleNumber) has an integer value of \(actualNumber)")
} else {
    println("\(possibleNumber) could not be converted to an integer")
}
// 輸出 "123 has an integer value of 123"
```

這段程式碼可以被理解為：

「如果`possibleNumber.toInt`回傳的 optional `Int`包含一個值，創建一個叫做`actualNumber`的新常數並將optional 包含的值指派給它。」

如果轉換成功，`actualNumber`常數可以在`if`語句的第一個分支中使用。它已經被 optional _包含的_值初始化過，所以不需要再使用`!`後綴來獲取它的值。在這個範例中，`actualNumber`只被用來輸出轉換結果。

你可以在 optional 綁定中使用常數和變數。如果你想在`if`語句的第一個分支中操作`actualNumber`的值，你可以改成`if var actualNumber`，這樣 optional 包含的值就會被指派給一個變數而不是常數。

### nil

你可以給 optional 變數指派為`nil`來表示它沒有值：

```swift
var serverResponseCode: Int? = 404
// serverResponseCode 包含一個optionals的 Int 值 404
serverResponseCode = nil
// serverResponseCode 現在不包含值
```

> 注意：
`nil`不能用於非 optional 的常數和變數。如果你的程式碼中有常數或者變數需要處理值不存在的情況，請把它們宣告成對應的 optional 型別。

如果你宣告一個 optional 常數或者變數但是沒有指派，它們會自動被設置為`nil`：

```swift
var surveyAnswer: String?
// surveyAnswer 被自動設置為 nil
```

> 注意：
>
Swift 的`nil`和 Objective-C 中的`nil`並不一樣。在 Objective-C 中，`nil`是一個指向不存在物件的指標。在 Swift 中，`nil`不是指標——它是一個確定的值，用來表示值不存在。_任何_型別的 optional 狀態都可以被設置為`nil`，不只是物件型別。

### 隱式解析 Optionals

如上所述，optionals 暗示了常數或者變數可以「沒有值」。optionals 可以透過`if`語句來判斷是否有值，如果有值的話可以透過 optional 綁定來解析值。

有時候在程式架構中，第一次被指派之後，可以確定一個 optional _總會_有值。在這種情況下，每次都要判斷和解析 optional 值是非常低效的，因為可以確定它總會有值。

這種型別的 optionals 狀態被定義為_隱式解析 optionals（implicitly unwrapped optionals）_。把想要用作 optionals 的型別的後面的問號（`String?`）改成感嘆號（`String!`）來宣告一個隱式解析 optionals。

當 optionals 被第一次指派之後就可以確定之後一直有值的時候，隱式解析 optionals 非常有用。隱式解析 optionals 主要被用在 Swift 中類別別的初始化中，請參考[類別別實例之間的迴圈強參考](16_Automatic_Reference_Counting.html#strong_reference_cycles_between_class_instances)。

一個隱式解析 optionals 其實就是一個普通的 optionals，但是可以被當做非 optionals 來使用，並不需要每次都使用解析來獲取 optionals 值。下面的範例展示了 optionals `String`和隱式解析 optional `String`之間的區別：

```swift
let possibleString: String? = "An optional string."
println(possibleString!) // 需要驚嘆號來獲取值
// 輸出 "An optional string."
```

```swift
let assumedString: String! = "An implicitly unwrapped optional string."
println(assumedString)  // 不需要感嘆號
// 輸出 "An implicitly unwrapped optional string."
```

你可以把隱式解析 optional 當做一個可以自動解析的 optional。你要做的只是宣告的時候把感嘆號放到型別的結尾，而不是每次取值的 optional 名字的結尾。

> 注意：
>
如果你在隱式解析 optional 沒有值的時候嘗試取值，會觸發執行時錯誤。和你在沒有值的普通 optional 後面加一個驚嘆號一樣。

你仍然可以把隱式解析 optional 當做普通 optional 來判斷它是否包含值：

```swift
if assumedString != nil {
    println(assumedString)
}
// 輸出 "An implicitly unwrapped optional string."
```

你也可以在 optional 綁定中使用隱式解析 optional 來檢查並解析它的值：

```swift
if let definiteString = assumedString {
    println(definiteString)
}
// 輸出 "An implicitly unwrapped optional string."
```

> 注意：
>
如果一個變數之後可能變成`nil`的話請不要使用隱式解析 optionals。如果你需要在變數的生命周期中判斷是否是`nil`的話，請使用普通的 optionals。

<a name="assertions"></a>
## Assertions

Optionals 可以讓你判斷值是否存在，你可以在程式碼中優雅地處理值不存在的情況。然而，在某些情況下，如果值不存在或者值並不滿足特定的條件，你的程式碼可能並不需要繼續執行。這時，你可以在你的程式碼中觸發一個_assertion_來結束程式碼的執行並透過除錯來找到值不存在的原因。

### 使用 Assertions 進行除錯

一個 assertion 會在執行時判斷一個邏輯條件是否為`true`。從字面意思來說，一個 assertion「斷言」一個條件是否為真。你可以使用 assertion 來保證在執行其他程式碼之前，某些重要的條件已經被滿足。如果條件判斷為`true`，程式碼執行會繼續進行；如果條件判斷為`false`，程式碼執行停止，你的應用程式被終止。

如果你的程式碼在除錯環境下觸發了一個 assertion，比如你在 Xcode 中構建並執行一個應用程式，你可以清楚地看到不合法的狀態發生在哪裡並檢查 assertion 被觸發時你的應用程式的狀態。此外，assertion 允許你附加一條除錯訊息。

你可以使用全域`assert`函式來寫一個 assertion。向`assert`函式傳入一個結果為`true`或者`false`的表達式以及一條訊息，當表達式為`false`的時候這條訊息會被顯示：

```swift
let age = -3
assert(age >= 0, "A person's age cannot be less than zero")
// 因為 age < 0，所以assertion會觸發
```

在這個範例中，只有`age >= 0`為`true`的時候程式碼執行才會繼續，也就是說，當`age`的值非負的時候。如果`age`的值是負數，就像程式碼中那樣，`age >= 0`為`false`，assertion 被觸發，結束應用程式。

assertion 訊息不能使用字串插值。assertion 訊息可以省略，就像這樣：

```swift
assert(age >= 0)
```

### 何時使用 Assertions

當條件可能為`false`時使用 assertion，但是最終一定要_保證_條件為`true`，這樣你的程式碼才能繼續執行。assertion 的適用情境：

* 整數型別的 subscript 索引值被傳到一個自定義 subscript 實作，但是 subscript 索引值可能太小或者太大。
* 需要給函式傳入一個值，但是非法的值可能導致函式不能正常執行。
* 一個 optional 值現在是`nil`，但是後面的程式碼執行需要一個非`nil`值。

請參考[Subscripts](12_Subscripts.html)和[函式](06_Functions.html)。

> 注意：
Assertion 可能導致你的應用程式終止執行，所以你應當仔細設計你的程式碼來讓非法條件不會出現。然而，在你的應用程式發佈之前，有時候非法條件可能出現，這時使用 assertion 可以快速發現問題。
