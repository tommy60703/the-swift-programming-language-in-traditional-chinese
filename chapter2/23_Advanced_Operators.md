> 翻譯：[xielingwang](https://github.com/xielingwang)
> 校對：[numbbbbb](https://github.com/numbbbbb)

# 高級運算子
-----------------

本頁內容包括：

- [位運算子](#bitwise_operators)
- [溢出運算子](#overflow_operators)
- [優先級和結合性(Precedence and Associativity)](#precedence_and_associativity)
- [運算子函式(Operator Functions)](#operator_functions)
- [自定義運算子](#custom_operators)

除了[基本運算子](02_Basic_Operators.html)中所講的運算子，Swift還有許多複雜的高級運算子，包括了C語和Objective-C中的位運算子和移位運算。

不同於C語言中的數值計算，Swift的數值計算預設是不可溢出的。溢出行為會被捕獲並報告為錯誤。你是故意的？好吧，你可以使用Swift為你準備的另一套預設允許溢出的數值運算子，如可溢出加`&+`。所有允許溢出的運算子都是以`&`開始的。

自定義的結構，類別和列舉，是否可以使用標準的運算子來定義操作？當然可以！在Swift中，你可以為你創建的所有型別定制運算子的操作。

可定制的運算子並不限於那些預設的運算子，自定義有個性的中綴，前綴，後綴及賦值運算子，當然還有優先級和結合性。這些運算子的實作可以運用預設的運算子，也可以運用之前定制的運算子。

<a name="bitwise_operators"></a>
## 位運算子

位運算子通常在諸如圖像處理和創建設備驅動等底層開發中使用，使用它可以單獨運算元據結構中原始資料的比特位。在使用一個自定義的協定進行通信的時候，運用位運算子來對原始資料進行編碼和解碼也是非常有效的。

Swift支援如下所有C語言的位運算子：

### 按位取反運算子

按位取反運算子`~`對一個運算元的每一位都取反。

![Art/bitwiseNOT_2x.png](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Art/bitwiseNOT_2x.png "Art/bitwiseNOT_2x.png")

這個運算子是前綴的，所以請不加任何空格地寫著運算元之前。

```swift
let initialBits: UInt8 = 0b00001111
let invertedBits = ~initialBits  // 等於 0b11110000
```

`UInt8`是8位無符整型，可以儲存0~255之間的任意數。這個範例初始化一個整型為二進制值`00001111`(前4位為`0`，後4位為`1`)，它的十進制值為`15`。

使用按位取反運算`~`對`initialBits`操作，然後賦值給`invertedBits`這個新常數。這個新常數的值等於所有位都取反的`initialBits`，即`1`變成`0`，`0`變成`1`，變成了`11110000`，十進制值為`240`。

### 按位與運算子

按位與運算子對兩個數進行操作，然後回傳一個新的數，這個數的每個位都需要兩個輸入數的同一位都為1時才為1。

![Art/bitwiseAND_2x.png](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Art/bitwiseAND_2x.png "Art/bitwiseAND_2x.png")

以下程式碼，`firstSixBits`和`lastSixBits`中間4個位都為1。對它倆進行按位與運算後，就得到了`00111100`，即十進制的`60`。

```swift
let firstSixBits: UInt8 = 0b11111100
let lastSixBits: UInt8  = 0b00111111
let middleFourBits = firstSixBits & lastSixBits  // 等於 00111100
```

### 按位或運算

按位或運算子`|`比較兩個數，然後回傳一個新的數，這個數的每一位設置1的條件是兩個輸入數的同一位都不為0(即任意一個為1，或都為1)。

![Art/bitwiseOR_2x.png](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Art/bitwiseOR_2x.png "Art/bitwiseOR_2x.png")

如下程式碼，`someBits`和`moreBits`在不同位上有`1`。按位或執行的結果是`11111110`，即十進制的`254`。

```swift
let someBits: UInt8 = 0b10110010
let moreBits: UInt8 = 0b01011110
let combinedbits = someBits | moreBits  // 等於 11111110
```

### 按位異或運算子

按位異或運算子`^`比較兩個數，然後回傳一個數，這個數的每個位設為`1`的條件是兩個輸入數的同一位不同，如果相同就設為`0`。

![Art/bitwiseXOR_2x.png](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Art/bitwiseXOR_2x.png "Art/bitwiseXOR_2x.png")

以下程式碼，`firstBits`和`otherBits`都有一個`1`跟另一個數不同的。所以按位異或的結果是把它這些位置為`1`，其他都置為`0`。

```swift
let firstBits: UInt8 = 0b00010100
let otherBits: UInt8 = 0b00000101
let outputBits = firstBits ^ otherBits  // 等於 00010001
```

### 按位左移/右移運算子

左移運算子`<<`和右移運算子`>>`會把一個數的所有比特位按以下定義的規則向左或向右移動指定位數。

按位左移和按位右移的效果相當把一個整數乘於或除於一個因子為`2`的整數。向左移動一個整型的比特位相當於把這個數乘於`2`，向右移一位就是除於`2`。

#### 無符整型的移位操作

對無符整型的移位的效果如下：

已經存在的比特位向左或向右移動指定的位數。被移出整型儲存邊界的的位數直接拋棄，移動留下的空白位用零`0`來填充。這種方法稱為邏輯移位。

以下這張把展示了 `11111111 << 1`(`11111111`向左移1位)，和 `11111111 >> 1`(`11111111`向右移1位)。藍色的是被移位的，灰色是被拋棄的，橙色的`0`是被填充進來的。

![Art/bitshiftUnsigned_2x.png](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Art/bitshiftUnsigned_2x.png "Art/bitshiftUnsigned_2x.png")

```swift
let shiftBits: UInt8 = 4   // 即二進制的00000100
shiftBits << 1             // 00001000
shiftBits << 2             // 00010000
shiftBits << 5             // 10000000
shiftBits << 6             // 00000000
shiftBits >> 2             // 00000001
```

你可以使用移位操作進行其他資料型別的編碼和解碼。

```swift
let pink: UInt32 = 0xCC6699
let redComponent = (pink & 0xFF0000) >> 16    // redComponent 是 0xCC, 即 204
let greenComponent = (pink & 0x00FF00) >> 8   // greenComponent 是 0x66, 即 102
let blueComponent = pink & 0x0000FF           // blueComponent 是 0x99, 即 153
```

這個範例使用了一個`UInt32`的命名為`pink`的常數來儲存層疊樣式表`CSS`中粉色的顏色值，`CSS`顏色`#CC6699`在Swift用十六進制`0xCC6699`來表示。然後使用按位與(&)和按位右移就可以從這個顏色值中解析出紅(CC)，綠(66)，藍(99)三個部分。

對`0xCC6699`和`0xFF0000`進行按位與`&`操作就可以得到紅色部分。`0xFF0000`中的`0`了遮蓋了`OxCC6699`的第二和第三個位元組，這樣`6699`被忽略了，只留下`0xCC0000`。

然後，按向右移動16位，即 `>> 16`。十六進制中每兩個字元是8比特位，所以移動16位的結果是把`0xCC0000`變成`0x0000CC`。這和`0xCC`是相等的，都是十進制的`204`。

同樣的，綠色部分來自於`0xCC6699`和`0x00FF00`的按位操作得到`0x006600`。然後向右移動8們，得到`0x66`，即十進制的`102`。

最後，藍色部分對`0xCC6699`和`0x0000FF`進行按位與運算，得到`0x000099`，無需向右移位了，所以結果就是`0x99`，即十進制的`153`。

#### 有符整型的移位操作

有符整型的移位操作相對複雜得多，因為正負號也是用二進制位表示的。(這裡舉的範例雖然都是8位的，但它的原理是通用的。)

有符整型通過第1個比特位(稱為符號位)來表達這個整數是正數還是負數。`0`代表正數，`1`代表負數。

其余的比特位(稱為數值位)儲存其實值。有符正整數和無符正整數在計算機裡的儲存結果是一樣的，下來我們來看`+4`內部的二進制結構。

![Art/bitshiftSignedFour_2x.png](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Art/bitshiftSignedFour_2x.png "Art/bitshiftSignedFour_2x.png")

符號位為`0`，代表正數，另外7比特位二進制表示的實際值就剛好是`4`。

負數呢，跟正數不同。負數儲存的是2的n次方減去它的絕對值，n為數值位的位數。一個8比特的數有7個數值位，所以是2的7次方，即128。

我們來看`-4`儲存的二進制結構。

![Art/bitshiftSignedMinusFour_2x.png](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Art/bitshiftSignedMinusFour_2x.png "Art/bitshiftSignedMinusFour_2x.png")

現在符號位為`1`，代表負數，7個數值位要表達的二進制值是124，即128 - 4。

![Art/bitshiftSignedMinusFourValue_2x.png](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Art/bitshiftSignedMinusFourValue_2x.png "Art/bitshiftSignedMinusFourValue_2x.png")

負數的編碼方式稱為二進制補碼表示。這種表示方式看起來很奇怪，但它有幾個優點。

首先，只需要對全部8個比特位(包括符號)做標準的二進制加法就可以完成 `-1 + -4` 的操作，忽略加法過程產生的超過8個比特位表達的任何資訊。

![Art/bitshiftSignedAddition_2x.png](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Art/bitshiftSignedAddition_2x.png "Art/bitshiftSignedAddition_2x.png")

第二，由於使用二進制補碼表示，我們可以和正數一樣對負數進行按位左移右移的，同樣也是左移1位時乘於`2`，右移1位時除於`2`。要達到此目的，對有符整型的右移有一個特別的要求：

對有符整型按位右移時，使用符號位(正數為`0`，負數為`1`)填充空白位。

![Art/bitshiftSigned_2x.png](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Art/bitshiftSigned_2x.png "Art/bitshiftSigned_2x.png")

這就確保了在右移的過程中，有符整型的符號不會發生變化。這稱為算術移位。

正因為正數和負數特殊的儲存方式，向右移位使它接近於`0`。移位過程中保持符號會不變，負數在接近`0`的過程中一直是負數。

<a name="overflow_operators"></a>
## 溢出運算子

預設情況下，當你往一個整型常數或變數賦於一個它不能承載的大數時，Swift不會讓你這麼干的，它會報錯。這樣，在操作過大或過小的數的時候就很安全了。

例如，`Int16`整型能承載的整數範圍是`-32768`到`32767`，如果給它賦上超過這個範圍的數，就會報錯：

```swift
var potentialOverflow = Int16.max
// potentialOverflow 等於 32767, 這是 Int16 能承載的最大整數
potentialOverflow += 1
// 噢, 出錯了
```

對過大或過小的數值進行錯誤處理讓你的數值邊界條件更靈活。

當然，你有意在溢出時對有效位進行截斷，你可采用溢出運算，而非錯誤處理。Swfit為整型計算提供了5個`&`符號開頭的溢出運算子。

- 溢出加法 `&+`
- 溢出減法 `&-`
- 溢出乘法 `&*`
- 溢出除法 `&/`
- 溢出取餘 `&%`

### 值的上溢出

下面範例使用了溢出加法`&+`來解剖的無符整數的上溢出

```swift
var willOverflow = UInt8.max
// willOverflow 等於UInt8的最大整數 255
willOverflow = willOverflow &+ 1
// 這時候 willOverflow 等於 0
```

`willOverflow`用`Int8`所能承載的最大值`255`(二進制`11111111`)，然後用`&+`加1。然後`UInt8`就無法表達這個新值的二進制了，也就導致了這個新值上溢出了，大家可以看下圖。溢出後，新值在`UInt8`的承載範圍內的那部分是`00000000`，也就是`0`。

![Art/overflowAddition_2x.png](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Art/overflowAddition_2x.png "Art/overflowAddition_2x.png")

### 值的下溢出

數值也有可能因為太小而越界。舉個範例：

`UInt8`的最小值是`0`(二進制為`00000000`)。使用`&-`進行溢出減1，就會得到二進制的`11111111`即十進制的`255`。

![Art/overflowUnsignedSubtraction_2x.png](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Art/overflowUnsignedSubtraction_2x.png "Art/overflowUnsignedSubtraction_2x.png")

Swift程式碼是這樣的:

```swift
var willUnderflow = UInt8.min
// willUnderflow 等於UInt8的最小值0
willUnderflow = willUnderflow &- 1
// 此時 willUnderflow 等於 255
```

有符整型也有類似的下溢出，有符整型所有的減法也都是對包括在符號位在內的二進制數進行二進制減法的，這在 "按位左移/右移運算子" 一節提到過。最小的有符整數是`-128`，即二進制的`10000000`。用溢出減法減去去1後，變成了`01111111`，即UInt8所能承載的最大整數`127`。

![Art/overflowSignedSubtraction_2x.png](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Art/overflowSignedSubtraction_2x.png "Art/overflowSignedSubtraction_2x.png")

來看看Swift程式碼：

```swift
var signedUnderflow = Int8.min
// signedUnderflow 等於最小的有符整數 -128
signedUnderflow = signedUnderflow &- 1
// 如今 signedUnderflow 等於 127
```

### 除零溢出

一個數除於0 `i / 0`，或者對0取餘數 `i % 0`，就會產生一個錯誤。

```swift
let x = 1
let y = x / 0
```

使用它們對應的可溢出的版本的運算子`&/`和`&%`進行除0操作時就會得到`0`值。

```swift
let x = 1
let y = x &/ 0
// y 等於 0
```

<a name="precedence_and_associativity"></a>
## 優先級和結合性

運算子的優先級使得一些運算子優先於其他運算子，高優先級的運算子會先被計算。

結合性定義相同優先級的運算子在一起時是怎麼組合或關聯的，是和左邊的一組呢，還是和右邊的一組。意思就是，到底是和左邊的表達式結合呢，還是和右邊的表達式結合？

在混合表達式中，運算子的優先級和結合性是非常重要的。舉個範例，為什麼下列表達式的結果為`4`？

```swift
2 + 3 * 4 % 5
// 結果是 4
```

如果嚴格地從左計算到右，計算過程會是這樣：

- 2 plus 3 equals 5;
- 2 + 3 = 5
- 5 times 4 equals 20;
- 5 * 4 = 20
- 20 remainder 5 equals 0
- 20 / 5 = 4 余 0

但是正確答案是`4`而不是`0`。優先級高的運算子要先計算，在Swift和C語言中，都是先乘除後加減的。所以，執行完乘法和取餘運算才能執行加減運算。

乘法和取餘擁有相同的優先級，在運算過程中，我們還需要結合性，乘法和取餘運算都是左結合的。這相當於在表達式中有隱藏的括號讓運算從左開始。

```swift
2 + ((3 * 4) % 5)
```

(3 * 4) is 12, so this is equivalent to:
3 * 4 = 12，所以這相當於：


```swift
2 + (12 % 5)
```

(12 % 5) is 2, so this is equivalent to:
12 % 5 = 2，所這又相當於

```swift
2 + 2
```

計算結果為 4。

查閱Swift運算子的優先級和結合性的完整列表，請看[表達式](../chapter3/04_Expressions.html)。

> 注意：  
Swift的運算子較C語言和Objective-C來得更簡單和保守，這意味著跟基於C的語言可能不一樣。所以，在移植已有程式碼到Swift時，注意去確保程式碼按你想的那樣去執行。

<a name="operator_functions"></a>
## 運算子函式

讓已有的運算子也可以對自定義的類別和結構進行運算，這稱為運算子重載。

這個範例展示了如何用`+`讓一個自定義的結構做加法。算術運算子`+`是一個兩目運算子，因為它有兩個運算元，而且它必須出現在兩個運算元之間。

範例中定義了一個名為`Vector2D`的二維坐標向量 `(x，y)` 的結構，然後定義了讓兩個`Vector2D`的物件相加的運算子函式。

```swift
struct Vector2D {
    var x = 0.0, y = 0.0
}
@infix func + (left: Vector2D, right: Vector2D) -> Vector2D {
    return Vector2D(x: left.x + right.x, y: left.y + right.y)
}
```

該運算子函式定義了一個全域的`+`函式，這個函式需要兩個`Vector2D`型別的參數，回傳值也是`Vector2D`型別。需要定義和實作一個中綴運算的時候，在關鍵字`func`之前寫上屬性 `@infix` 就可以了。

在這個程式碼實作中，參數被命名為了`left`和`right`，代表`+`左邊和右邊的兩個`Vector2D`物件。函式回傳了一個新的`Vector2D`的物件，這個物件的`x`和`y`分別等於兩個參數物件的`x`和`y`的和。

這個函式是全域的，而不是`Vector2D`結構的成員方法，所以任意兩個`Vector2D`物件都可以使用這個中綴運算子。

```swift
let vector = Vector2D(x: 3.0, y: 1.0)
let anotherVector = Vector2D(x: 2.0, y: 4.0)
let combinedVector = vector + anotherVector
// combinedVector 是一個新的Vector2D, 值為 (5.0, 5.0)
```

這個範例實作兩個向量 `(3.0，1.0)` 和 `(2.0，4.0)` 相加，得到向量 `(5.0，5.0)` 的過程。如下圖示：

![Art/vectorAddition_2x.png](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Art/vectorAddition_2x.png "Art/vectorAddition_2x.png")

### 前綴和後綴運算子

上個範例演示了一個雙目中綴運算子的自定義實作，同樣我們也可以玩標準單目運算子的實作。單目運算子只有一個運算元，在運算元之前就是前綴的，如`-a`; 在運算元之後就是後綴的，如`i++`。

實作一個前綴或後綴運算子時，在定義該運算子的時候於關鍵字`func`之前標注 `@prefix` 或 `@postfix` 屬性。

```swift
@prefix func - (vector: Vector2D) -> Vector2D {
    return Vector2D(x: -vector.x, y: -vector.y)
}
```

這段程式碼為`Vector2D`型別提供了單目減運算`-a`，`@prefix`屬性表明這是個前綴運算子。

對於數值，單目減運算子可以把正數變負數，把負數變正數。對於`Vector2D`，單目減運算將其`x`和`y`都進進行單目減運算。

```swift
let positive = Vector2D(x: 3.0, y: 4.0)
let negative = -positive
// negative 為 (-3.0, -4.0)
let alsoPositive = -negative
// alsoPositive 為 (3.0, 4.0)
```

### 組合賦值運算子

組合賦值是其他運算子和賦值運算子一起執行的運算。如`+=`把加運算和賦值運算組合成一個操作。實作一個組合賦值符號需要使用`@assignment`屬性，還需要把運算子的左參數設置成`inout`，因為這個參數會在運算子函式內直接修改它的值。

```swift
@assignment func += (inout left: Vector2D, right: Vector2D) {
    left = left + right
}
```

因為加法運算在之前定義過了，這裡無需重新定義。所以，加賦運算子函式使用已經存在的高級加法運算子函式來執行左值加右值的運算。

```swift
var original = Vector2D(x: 1.0, y: 2.0)
let vectorToAdd = Vector2D(x: 3.0, y: 4.0)
original += vectorToAdd
// original 現在為 (4.0, 6.0)
```

你可以將 `@assignment` 屬性和 `@prefix` 或 `@postfix` 屬性起來組合，實作一個`Vector2D`的前綴運算子。

```swift
@prefix @assignment func ++ (inout vector: Vector2D) -> Vector2D {
    vector += Vector2D(x: 1.0, y: 1.0)
    return vector
}
```

這個前綴使用了已經定義好的高級加賦運算，將自己加上一個值為 `(1.0，1.0)` 的物件然後賦給自己，然後再將自己回傳。

```swift
var toIncrement = Vector2D(x: 3.0, y: 4.0)
let afterIncrement = ++toIncrement
// toIncrement 現在是 (4.0, 5.0)
// afterIncrement 現在也是 (4.0, 5.0)
```

>注意：  
預設的賦值符是不可重載的。只有組合賦值符可以重載。三目條件運算子 `a？b：c` 也是不可重載。

### 比較運算子

Swift無所知道自定義型別是否相等或不等，因為等於或者不等於由你的程式碼說了算了。所以自定義的類別和結構要使用比較符`==`或`!=`就需要重載。

定義相等運算子函式跟定義其他中綴運算子雷同：

```swift
@infix func == (left: Vector2D, right: Vector2D) -> Bool {
    return (left.x == right.x) && (left.y == right.y)
}

@infix func != (left: Vector2D, right: Vector2D) -> Bool {
    return !(left == right)
}
```

上述程式碼實作了相等運算子`==`來判斷兩個`Vector2D`物件是否有相等的值，相等的概念就是它們有相同的`x`值和相同的`y`值，我們就用這個邏輯來實作。接著使用`==`的結果實作了不相等運算子`!=`。

現在我們可以使用這兩個運算子來判斷兩個`Vector2D`物件是否相等。

```swift
let twoThree = Vector2D(x: 2.0, y: 3.0)
let anotherTwoThree = Vector2D(x: 2.0, y: 3.0)
if twoThree == anotherTwoThree {
    println("這兩個向量是相等的.")
}
// prints "這兩個向量是相等的."
```

### 自定義運算子

標準的運算子不夠玩，那你可以宣告一些個性的運算子，但個性的運算子只能使用這些字元 `/ = - + * % < >！& | ^。~`。

新的運算子宣告需在全域域使用`operator`關鍵字宣告，可以宣告為前綴，中綴或後綴的。

```swift
operator prefix +++ {}
```


這段程式碼定義了一個新的前綴運算子叫`+++`，此前Swift並不存在這個運算子。此處為了演示，我們讓`+++`對`Vector2D`物件的操作定義為 `雙累加` 這樣一個獨有的操作，這個操作使用了之前定義的加賦運算實作了自已加上自己然後回傳的運算。

```swift
@prefix @assignment func +++ (inout vector: Vector2D) -> Vector2D {
    vector += vector
    return vector
}
```

`Vector2D` 的 `+++` 的實作和 `++` 的實作很接近, 唯一不同的前者是加自己, 後者是加值為 `(1.0, 1.0)` 的向量.

```swift
var toBeDoubled = Vector2D(x: 1.0, y: 4.0)
let afterDoubling = +++toBeDoubled
// toBeDoubled 現在是 (2.0, 8.0)
// afterDoubling 現在也是 (2.0, 8.0)
```

### 自定義中綴運算子的優先級和結合性

可以為自定義的中綴運算子指定優先級和結合性。可以回頭看看[優先級和結合性](#PrecedenceandAssociativity)解釋這兩個因素是如何影響多種中綴運算子混合的表達式的計算的。

結合性(associativity)的值可取的值有`left`，`right`和`none`。左結合運算子跟其他優先級相同的左結合運算子寫在一起時，會跟左邊的運算元結合。同理，右結合運算子會跟右邊的運算元結合。而非結合運算子不能跟其他相同優先級的運算子寫在一起。

結合性(associativity)的值預設為`none`，優先級(precedence)預設為`100`。

以下範例定義了一個新的中綴符`+-`，是左結合的`left`，優先級為`140`。

```swift
operator infix +- { associativity left precedence 140 }
func +- (left: Vector2D, right: Vector2D) -> Vector2D {
    return Vector2D(x: left.x + right.x, y: left.y - right.y)
}
let firstVector = Vector2D(x: 1.0, y: 2.0)
let secondVector = Vector2D(x: 3.0, y: 4.0)
let plusMinusVector = firstVector +- secondVector
// plusMinusVector 此時的值為 (4.0, -2.0)
```

這個運算子把兩個向量的`x`相加，把向量的`y`相減。因為他實際是屬於加減運算，所以讓它保持了和加法一樣的結合性和優先級(`left`和`140`)。查閱完整的Swift預設結合性和優先級的設置，請移步[表達式](../chapter3/04_Expressions.html);
