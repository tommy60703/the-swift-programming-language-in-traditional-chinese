> 翻譯：[JaySurplus](https://github.com/JaySurplus)
> 校對：[sg552](https://github.com/sg552)

# 類別和結構

本頁包含內容：

- [類別和結構對比](#comparing_classes_and_structures)
- [結構和列舉是值型別](#structures_and_enumerations_are_value_types)
- [類別是參考型別](#classes_are_reference_types)
- [類別和結構的選擇](#choosing_between_classes_and_structures)
- [集合（collection）型別的賦值與複製行為](#assignment_and_copy_behavior_for_collection_types)

類別和結構是人們構建程式碼所用的一種通用且靈活的建構體。為了在類別和結構中實作各種功能，我們必須要嚴格按照對於常數，變數以及函式所規定的語法規則來定義屬性和添加方法。

與其他程式語言所不同的是，Swift 並不要求你為自定義類別和結構去創建獨立的接口和實作文件。你所要做的是在一個單一文件中定義一個類別或者結構，系統將會自動生成面向其它程式碼的外部接口。

>  注意：  
通常一個`類別`的實例被稱為`物件`。然而在Swift 中，類別和結構的關系要比在其他語言中更加的密切，本章中所討論的大部分功能都可以用在類別和結構上。因此，我們會主要使用`實例`而不是`物件`。

<a name="comparing_classes_and_structures"></a>
###類別和結構對比

Swift 中類別和結構有很多共同點。共同處在於：

* 定義屬性用於儲存值
* 定義方法用於提供功能
* 定義附屬腳本用於存取值
* 定義建構器用於生成初始化值
* 通過擴展以增加預設實作的功能
* 符合協定以對某類別提供標準功能

更多資訊請參見 [屬性](10_Properties.html)，[方法](11_Methods.html)，[下標腳本](12_Subscripts.html)，[初始過程](14_Initialization.html)，[擴展](20_Extensions.html)，和[協定](21_Protocols.html)。

與結構相比，類別還有如下的附加功能：

* 繼承允許一個類別繼承另一個類別的特征
* 型別轉換允許在執行時檢查和解釋一個類別實例的型別
* 解構器允許一個類別實例釋放任何其所被分配的資源
* 參考計數允許對一個類別的多次參考

更多資訊請參見[繼承](http://)，[型別轉換](http://)，[初始化](http://)，和[自動引用計數](http://)。

> 注意：  
結構總是通過被複製的方式在程式碼中傳遞，因此請不要使用參考計數。

### 定義

類別和結構有著類似的定義方式。我們通過關鍵字`class`和`struct`來分別表示類別和結構，並在一對大括號中定義它們的具體內容：

```swift
class SomeClass {
	// class definition goes here
}
struct SomeStructure {
	// structure definition goes here
}
```

>  注意：  
在你每次定義一個新類別或者結構的時候，實際上你是有效地定義了一個新的 Swift 型別。因此請使用 `UpperCamelCase` 這種方式來命名（如 `SomeClass` 和`SomeStructure`等），以便符合標準Swift 型別的大寫命名風格（如`String`，`Int`和`Bool`）。相反的，請使用`lowerCamelCase`這種方式為屬性和方法命名（如`framerate`和`incrementCount`），以便和類別區分。

以下是定義結構和定義類別的示例：

```swift
struct Resolution {
	var width = 0
	var heigth = 0
}
class VideoMode {
	var resolution = Resolution()
	var interlaced = false
	var frameRate = 0.0
	var name: String?
}
```

在上面的示例中我們定義了一個名為`Resolution`的結構，用來描述一個顯示器的像素分辨率。這個結構包含了兩個名為`width`和`height`的儲存屬性。儲存屬性是捆綁和儲存在類別或結構中的常數或變數。當這兩個屬性被初始化為整數`0`的時候，它們會被推斷為`Int`型別。

在上面的示例中我們還定義了一個名為`VideoMode`的類別，用來描述一個視頻顯示器的特定模式。這個類別包含了四個儲存屬性變數。第一個是`分辨率`，它被初始化為一個新的`Resolution`結構的實例，具有`Resolution`的屬性型別。新`VideoMode`實例同時還會初始化其它三個屬性，它們分別是，初始值為`false`(意為「non-interlaced video」)的`inteflaced`，回放幀率初始值為`0.0`的`frameRate`和值為可選`String`的`name`。`name`屬性會被自動賦予一個預設值`nil`，意為「沒有`name`值」，因它是一個可選型別。

### 類別和結構實例

`Resolution`結構和`VideoMode`類別的定義僅描述了什麼是`Resolution`和`VideoMode`。它們並沒有描述一個特定的分辨率（resolution）或者視頻模式（video mode）。為了描述一個特定的分辨率或者視頻模式，我們需要生成一個它們的實例。

生成結構和類別實例的語法非常相似：

```swift
let someResolution = Resolution()
let someVideoMode = VideoMode()
```

結構和類別都使用建構器語法來生成新的實例。建構器語法的最簡單形式是在結構或者類別的型別名稱後跟隨一個空括弧，如`Resolution()`或`VideoMode()`。通過這種方式所創建的類別或者結構實例，其屬均會被初始化為預設值。[建構過程](14_Initialization.html)章節會對類別和結構的初始化進行更詳細的討論。

### 屬性存取

通過使用*點語法*（*dot syntax*）,你可以存取實例中所含有的屬性。其語法規則是，實例名後面緊跟屬性名，兩者通過點號(.)連接：

```swift
println("The width of someResolution is \(someResolution.width)")
// 輸出 "The width of someResolution is 0"
```

在上面的範例中，`someResolution.width`參考`someResolution`的`width`屬性，回傳`width`的初始值`0`。

你也可以存取子屬性，如何`VideoMode`中`Resolution`屬性的`width`屬性：

```swift
println("The width of someVideoMode is \(someVideoMode.resolution.width)")
// 輸出 "The width of someVideoMode is 0"
```

你也可以使用點語法為屬性變數賦值：

```swift
someVideoMode.resolution.width = 12880
println("The width of someVideoMode is now \(someVideoMode.resolution.width)")
// 輸出 "The width of someVideoMode is now 1280"
```

>  注意：  
與 Objective-C 語言不同的是，Swift 允許直接設置結構屬性的子屬性。上面的最後一個範例，就是直接設置了`someVideoMode`中`resolution`屬性的`width`這個子屬性，以上操作並不需要從新設置`resolution`屬性。

### 結構型別的成員逐一建構器(Memberwise Initializers for structure Types)

所有結構都有一個自動生成的成員逐一建構器，用於初始化新結構實例中成員的屬性。新實例中各個屬性的初始值可以通過屬性的名稱傳遞到成員逐一建構器之中：

```swift
let vga = resolution(width:640, heigth: 480)
```

與結構不同，類別實例沒有預設的成員逐一建構器。[建構過程](14_Initialization.html)章節會對建構器進行更詳細的討論。

<a name="structures_and_enumerations_are_value_types"></a>
## 結構和列舉是值型別

值型別被賦予給一個變數，常數或者本身被傳遞給一個函式的時候，實際上操作的是其的拷貝。

在之前的章節中，我們已經大量使用了值型別。實際上，在 Swift 中，所有的基本型別：整數（Integer）、浮點數（floating-point）、布林值（Booleans）、字串（string)、陣列（array）和字典（dictionaries），都是值型別，並且都是以結構的形式在後台所實作。

在 Swift 中，所有的結構和列舉都是值型別。這意味著它們的實例，以及實例中所包含的任何值型別屬性，在程式碼中傳遞的時候都會被複製。

請看下面這個示例，其使用了前一個示例中`Resolution`結構：

```swift
let hd = Resolution(width: 1920, height: 1080)
var cinema = hd
```

在以上示例中，宣告了一個名為`hd`的常數，其值為一個初始化為全高清視頻分辨率（1920 像素寬，1080 像素高）的`Resolution`實例。

然後示例中又宣告了一個名為`cinema`的變數，其值為之前宣告的`hd`。因為`Resolution`是一個結構，所以`cinema`的值其實是`hd`的一個拷貝副本，而不是`hd`本身。儘管`hd`和`cinema`有著相同的寬（width）和高（height）屬性，但是在後台中，它們是兩個完全不同的實例。

下面，為了符合數碼影院放映的需求（2048 像素寬，1080 像素高），`cinema`的`width`屬性需要作如下修改：

```swift
cinema.width = 2048
```

這裡，將會顯示`cinema`的`width`屬性確已改為了`2048`：

```swift
println("cinema is now  \(cinema.width) pixels wide")
// 輸出 "cinema is now 2048 pixels wide"
```

然而，初始的`hd`實例中`width`屬性還是`1920`：

```swift
println("hd is still \(hd.width	) pixels wide")
// 輸出 "hd is still 1920 pixels wide"
```

在將`hd`賦予給`cinema`的時候，實際上是將`hd`中所儲存的`值（values）`進行拷貝，然後將拷貝的資料儲存到新的`cinema`實例中。結果就是兩個完全獨立的實例碰巧包含有相同的數值。由於兩者相互獨立，因此將`cinema`的`width`修改為`2048`並不會影響`hd`中的寬（width）。

列舉也遵循相同的行為准則：

```swift
enum CompassPoint {
	case North, South, East, West
}
var currentDirection = CompassPoint.West
let rememberedDirection = currentDirection
currentDirection = .East
if rememberDirection == .West {
	println("The remembered direction is still .West")
}
// 輸出 "The remembered direction is still .West"
```

上例中`rememberedDirection`被賦予了`currentDirection`的值（value），實際上它被賦予的是值（value）的一個拷貝。賦值過程結束後再修改`currentDirection`的值並不影響`rememberedDirection`所儲存的原始值（value）的拷貝。

<a name="classes_are_reference_types"></a>
## 類別是參考型別

與值型別不同，參考型別在被賦予到一個變數，常數或者被傳遞到一個函式時，操作的並不是其拷貝。因此，參考的是已存在的實例本身而不是其拷貝。

請看下面這個示例，其使用了之前定義的`VideoMode`類別：

```swift
let tenEighty = VideoMode()
tenEighty.resolution = hd
tenEighty.interlaced = true
tenEighty.name = "1080i"
tenEighty.frameRate = 25.0
```

以上示例中，宣告了一個名為`tenEighty`的常數，其參考了一個`VideoMode`類別的新實例。在之前的示例中，這個視頻模式（video mode）被賦予了HD分辨率（1920*1080）的一個拷貝（`hd`）。同時設置為交錯（interlaced）,命名為`「1080i」`。最後，其幀率是`25.0`幀每秒。

然後，`tenEighty` 被賦予名為`alsoTenEighty`的新常數，同時對`alsoTenEighty`的幀率進行修改：

```swift
let alsoTenEighty = tenEighty
alsoTenEighty.frameRate = 30.0
```

因為類別是參考型別，所以`tenEight`和`alsoTenEight`實際上參考的是相同的`VideoMode`實例。換句話說，它們只是同一個實例的兩種叫法。

下面，通過查看`tenEighty`的`frameRate`屬性，我們會發現它正確的顯示了基本`VideoMode`實例的新幀率，其值為`30.0`：

```swift
println("The frameRate property of tenEighty is now \(tenEighty.frameRate)")
// 輸出 "The frameRate property of theEighty is now 30.0"
```

需要注意的是`tenEighty`和`alsoTenEighty`被宣告為*常數（（constants）*而不是變數。然而你依然可以改變`tenEighty.frameRate`和`alsoTenEighty.frameRate`,因為這兩個常數本身不會改變。它們並不`儲存`這個`VideoMode`實例，在後台僅僅是對`VideoMode`實例的參考。所以，改變的是被參考的基礎`VideoMode`的`frameRate`參數，而不改變常數的值。

### 恆等運算子

因為類別是參考型別，有可能有多個常數和變數在後台同時參考某一個類別實例。（對於結構和列舉來說，這並不成立。因為它們作值型別，在被賦予到常數，變數或者傳遞到函式時，總是會被拷貝。）

如果能夠判定兩個常數或者變數是否參考同一個類別實例將會很有幫助。為了達到這個目的，Swift 內建了兩個恆等運算子：

* 等價於 （ === ）
* 不等價於 （ !== ）

以下是運用這兩個運算子檢測兩個常數或者變數是否參考同一個實例：

```swift
if tenEighty === alsoTenTighty {
	println("tenTighty and alsoTenEighty refer to the same Resolution instance.")
}
//輸出 "tenEighty and alsoTenEighty refer to the same Resolution instance."
```

請注意「等價於」（用三個等號表示，===） 與「等於」（用兩個等號表示，==）的不同：

* 「等價於」表示兩個類型別（class type）的常數或者變數參考同一個類別實例。
* 「等於」表示兩個實例的值「相等」或「相同」，判定時要遵照類別設計者定義定義的評判標準，因此相比於「相等」，這是一種更加合適的叫法。

當你在定義你的自定義類別和結構的時候，你有義務來決定判定兩個實例「相等」的標準。在章節[運算子函式(Operator Functions)](23_Advanced_Operators.html#operator_functions)中將會詳細介紹實作自定義「等於」和「不等於」運算子的流程。

### 指針

如果你有 C，C++ 或者 Objective-C 語言的經驗，那麼你也許會知道這些語言使用指針來參考內存中的地址。一個 Swift 常數或者變數參考一個參考型別的實例與 C 語言中的指針類似，不同的是並不直接指向內存中的某個地址，而且也不要求你使用星號（*）來表明你在創建一個參考。Swift 中這些參考與其它的常數或變數的定義方式相同。

<a name="choosing_between_classes_and_structures"></a>
## 類別和結構的選擇

在你的程式碼中，你可以使用類別和結構來定義你的自定義資料型別。

然而，結構實例總是通過值傳遞，類別實例總是通過參考傳遞。這意味兩者適用不同的任務。當你的在考慮一個工程項目的資料建構和功能的時候，你需要決定每個資料建構是定義成類別還是結構。

按照通用的准則，當符合一條或多條以下條件時，請考慮構建結構：

* 結構的主要目的是用來封裝少量相關簡單資料值。
* 有理由預計一個結構實例在賦值或傳遞時，封裝的資料將會被拷貝而不是被參考。
* 任何在結構中儲存的值型別屬性，也將會被拷貝，而不是被參考。
* 結構不需要去繼承另一個已存在型別的屬性或者行為。

合適的結構候選者包括：

* 幾何形狀的大小，封裝一個`width`屬性和`height`屬性，兩者均為`Double`型別。
* 一定範圍內的路徑，封裝一個`start`屬性和`length`屬性，兩者均為`Int`型別。
* 三維坐標系內一點，封裝`x`，`y`和`z`屬性，三者均為`Double`型別。

在所有其它案例中，定義一個類別，生成一個它的實例，並通過參考來管理和傳遞。實際中，這意味著絕大部分的自定義資料建構都應該是類別，而非結構。

<a name="assignment_and_copy_behavior_for_collection_types"></a>
## 集合（Collection）型別的賦值和拷貝行為

Swift 中`陣列（Array）`和`字典（Dictionary）`型別均以結構的形式實作。然而當陣列被賦予一個常數或變數，或被傳遞給一個函式或方法時，其拷貝行為與字典和其它結構有些許不同。

以下對`陣列`和`結構`的行為描述與對`NSArray`和`NSDictionary`的行為描述在本質上不同，後者是以類別的形式實作，前者是以結構的形式實作。`NSArray`和`NSDictionary`實例總是以對已有實例參考,而不是拷貝的方式被賦值和傳遞。

> 注意：  
以下是對於陣列，字典，字串和其它值的`拷貝`的描述。
在你的程式碼中，拷貝好像是確實是在有拷貝行為的地方產生過。然而，在 Swift 的後台中，只有確有必要，`實際（actual）`拷貝才會被執行。Swift 管理所有的值拷貝以確保性能最優化的性能，所以你也沒有必要去避免賦值以保證最優性能。（實際賦值由系統管理優化）

### 字典型別的賦值和拷貝行為

無論何時將一個`字典`實例賦給一個常數或變數，或者傳遞給一個函式或方法，這個字典會即會在賦值或呼叫發生時被拷貝。在章節[結構和列舉是值型別](#structures_and_enumerations_are_value_types)中將會對此過程進行詳細介紹。

如果`字典`實例中所儲存的鍵（keys）和/或值（values）是值型別（結構或列舉），當賦值或呼叫發生時，它們都會被拷貝。相反，如果鍵（keys）和/或值（values）是參考型別，被拷貝的將會是參考，而不是被它們參考的類別實例或函式。`字典`的鍵和值的拷貝行為與結構所儲存的屬性的拷貝行為相同。

下面的示例定義了一個名為`ages`的字典，其中儲存了四個人的名字和年齡。`ages`字典被賦予了一個名為`copiedAges`的新變數，同時`ages`在賦值的過程中被拷貝。賦值結束後，`ages`和`copiedAges`成為兩個相互獨立的字典。

```swift
var ages = ["Peter": 23, "Wei": 35, "Anish": 65, "Katya": 19]
var copiedAges = ages
```

這個字典的鍵（keys）是`字串（String）`型別，值（values）是`整（Int）`型別。這兩種型別在Swift 中都是值型別（value types），所以當字典被拷貝時，兩者都會被拷貝。

我們可以通過改變一個字典中的年齡值（age value），檢查另一個字典中所對應的值，來證明`ages`字典確實是被拷貝了。如果在`copiedAges`字典中將`Peter`的值設為`24`，那麼`ages`字典仍然會回傳修改前的值`23`：

```swift
copiedAges["Peter"] = 24
println(ages["Peter"])
// 輸出 "23"
```

### 陣列的賦值和拷貝行為

在Swift 中，`陣列（Arrays）`型別的賦值和拷貝行為要比`字典（Dictionary）`型別的複雜的多。當操作陣列內容時，`陣列（Array）`能提供接近C語言的的性能，並且拷貝行為只有在必要時才會發生。

如果你將一個`陣列（Array）`實例賦給一個變數或常數，或者將其作為參數傳遞給函式或方法呼叫，在事件發生時陣列的內容`不`會被拷貝。相反，陣列公用相同的元素序列。當你在一個陣列內修改某一元素，修改結果也會在另一陣列顯示。

對陣列來說，拷貝行為僅僅當操作有可能修改陣列`長度`時才會發生。這種行為包括了附加（appending）,插入（inserting）,刪除（removing）或者使用範圍下標（ranged subscript）去替換這一範圍內的元素。只有當陣列拷貝確要發生時，陣列內容的行為規則與字典中鍵值的相同，參見章節[集合（collection）型別的賦值與複製行為](#assignment_and_copy_behavior_for_collection_types。

下面的示例將一個`整數（Int）`陣列賦給了一個名為`a`的變數，繼而又被賦給了變數`b`和`c`：

```swift
var a = [1, 2, 3]
var b = a
var c = a
```

我們可以在`a`,`b`,`c`上使用下標語法以得到陣列的第一個元素：

```swift
println(a[0])
// 1
println(b[0])
// 1
println(c[0])
// 1
```

如果通過下標語法修改陣列中某一元素的值，那麼`a`,`b`,`c`中的相應值都會發生改變。請注意當你用下標語法修改某一值時，並沒有拷貝行為伴隨發生，因為下表語法修改值時沒有改變陣列長度的可能：

```swift
a[0] = 42
println(a[0])
// 42
println(b[0])
// 42
println(c[0])
// 42
```

然而，當你給`a`附加新元素時，陣列的長度`會`改變。
當附加元素這一事件發生時，Swift 會創建這個陣列的一個拷貝。從此以後，`a`將會是原陣列的一個獨立拷貝。

拷貝發生後，如果再修改`a`中元素值的話，`a`將會回傳與`b`，`c`不同的結果，因為後兩者參考的是原來的陣列：

```swift
a.append(4)
a[0] = 777
println(a[0])
// 777
println(b[0])
// 42
println(c[0])
// 42
```

### 確保陣列的唯一性

在操作一個陣列，或將其傳遞給函式以及方法呼叫之前是很有必要先確定這個陣列是有一個唯一拷貝的。通過在陣列變數上呼叫`unshare`方法來確定陣列參考的唯一性。（當陣列賦給常數時，不能呼叫`unshare`方法）

如果一個陣列被多個變數參考，在其中的一個變數上呼叫`unshare`方法，則會拷貝此陣列，此時這個變數將會有屬於它自己的獨立陣列拷貝。當陣列僅被一個變數參考時，則不會有拷貝發生。

在上一個示例的最後，`b`和`c`都參考了同一個陣列。此時在`b`上呼叫`unshare`方法則會將`b`變成一個唯一個拷貝：

```swift
b.unshare()
```

在`unshare`方法呼叫後再修改`b`中第一個元素的值，這三個陣列（`a`,`b`,`c`）會回傳不同的三個值：

```swift
b[0] = -105
println(a[0])
// 77
println(b[0])
// -105
println(c[0])
// 42
```


### 判定兩個陣列是否共用相同元素

我們通過使用恆等運算子（identity operators）（ === 和 !==）來判定兩個陣列或子陣列共用相同的儲存空間或元素。

下面這個示例使用了「等同（identical to）」 運算子（===） 來判定`b`和`c`是否共用相同的陣列元素：

```swift
if b === c {
	println("b and c still share the same array elements.")
} else {
	println("b and c now refer to two independent sets of array elements.")
}
```

```swift
// 輸出 "b and c now refer totwo independent sets of array elements."
```

此外，我們還可以使用恆等運算子來判定兩個子陣列是否共用相同的元素。下面這個示例中，比較了`b`的兩個相等的子陣列，並且確定了這兩個子陣列都參考相同的元素：

```swift
if b[0...1] === b[0...1] {
	println("These two subarrays share the same elements.")
} else {
	println("These two subarrays do not share the same elements.")
}
// 輸出 "These two subarrays share the same elements."
```

### 強制複製陣列

我們通過呼叫陣列的`copy`方法進行強制顯性複製。這個方法對陣列進行了淺拷貝（shallow copy）,並且回傳一個包含此拷貝的新陣列。

下面這個示例中定義了一個`names`陣列，其包含了七個人名。還定義了一個`copiedNames`變數，用以儲存在`names`上呼叫`copy`方法所回傳的結果：

```swift
var names = ["Mohsen", "Hilary", "Justyn", "Amy", "Rich", "Graham", "Vic"]
var copiedNames = names.copy()
```

我們可以通過修改一個陣列中某元素，並且檢查另一個陣列中對應元素的方法來判定`names`陣列確已被複製。如果你將`copiedNames`中第一個元素從"`Mohsen`"修改為"`Mo`",則`names`陣列回傳的仍是拷貝發生前的"`Mohsen`"：

```swift
copiedName[0] = "Mo"
println(name[0])
// 輸出 "Mohsen"
```

> 注意：  
如果你僅需要確保你對陣列的參考是唯一參考，請呼叫`unshare`方法，而不是`copy`方法。`unshare`方法僅會在確有必要時才會創建陣列拷貝。`copy`方法會在任何時候都創建一個新的拷貝，即使參考已經是唯一參考。

