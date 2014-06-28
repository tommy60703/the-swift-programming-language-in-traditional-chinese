> 翻譯：[lyuka](https://github.com/lyuka)
> 校對：[numbbbbb](https://github.com/numbbbbb), [stanzhai](https://github.com/stanzhai)

# 型別（Types）
-----------------

本頁包含內容：

- [型別注解（Type Annotation）](#type_annotation)
- [型別識別符號（Type Identifier）](#type_identifier)
- [元組型別（Tuple Type）](#tuple_type)
- [函式型別（Function Type）](#function_type)
- [陣列型別（Array Type）](#array_type)
- [可選型別（Optional Type）](#optional_type)
- [隱式解析可選型別（Implicitly Unwrapped Optional Type）](#implicitly_unwrapped_optional_type)
- [協定合成型別（Protocol Composition Type）](#protocol_composition_type)
- [元型別（Metatype Type）](#metatype_type)
- [型別繼承子句（Type Inheritance Clause）](#type_inheritance_clause)
- [型別推斷（Type Inference）](#type_inference)

Swift 語言存在兩種型別：命名型型別和複合型型別。*命名型型別*是指定義時可以給定名字的型別。命名型型別包括類別、結構、列舉和協定。比如，一個使用者定義的類別`MyClass`的實例擁有型別`MyClass`。除了使用者定義的命名型型別，Swift 標準函式庫也定義了很多常用的命名型型別，包括那些表示陣列、字典和可選值的型別。

那些通常被其它語言認為是基本或初級的資料型型別（Data types）——比如表示數字、字元和字串——實際上就是命名型型別，Swift 標準函式庫是使用結構定義和實作它們的。因為它們是命名型型別，因此你可以按照“擴展和擴展宣告”章節裡討論的那樣，宣告一個擴展來增加它們的行為以適應你程式的需求。

*複合型型別*是沒有名字的型別，它由 Swift 本身定義。Swift 存在兩種複合型型別：函式型別和元組型別。一個複合型型別可以包含命名型型別和其它複合型型別。例如，元組型別`(Int, (Int, Int))`包含兩個元素：第一個是命名型型別`Int`，第二個是另一個複合型型別`(Int, Int)`.

本節討論 Swift 語言本身定義的型別，並描述 Swift 中的型別推斷行為。

> 型別語法  
> *型別* → [*陣列型別*](..\chapter3\03_Types.html#array_type) | [*函式型別*](..\chapter3\03_Types.html#function_type) | [*型別標識*](..\chapter3\03_Types.html#type_identifier) | [*元組型別*](..\chapter3\03_Types.html#tuple_type) | [*可選型別*](..\chapter3\03_Types.html#optional_type) | [*隱式解析可選型別*](..\chapter3\03_Types.html#implicitly_unwrapped_optional_type) | [*協定合成型別*](..\chapter3\03_Types.html#protocol_composition_type) | [*元型型別*](..\chapter3\03_Types.html#metatype_type)  

<a name="type_annotation"></a>
##型別注解

型別注解顯式地指定一個變數或表達式的值。型別注解始於冒號`:`終於型別，比如下面兩個範例：

```swift
let someTuple：(Double, Double) = (3.14159, 2.71828)
func someFunction(a: Int){ /* ... */ }
```
在第一個範例中，表達式`someTuple`的型別被指定為`(Double, Double)`。在第二個範例中，函式`someFunction`的參數`a`的型別被指定為`Int`。

型別注解可以在型別之前包含一個型別特性（type attributes）的可選列表。

> 型別注解語法  
> *型別注解* → **:** [*特性(Attributes)列表*](..\chapter3\06_Attributes.html#attributes) _可選_ [*型別*](..\chapter3\03_Types.html#type)  

<a name="type_identifier"></a>
##型別識別符號

型別識別符號參考命名型型別或者是命名型/複合型型別的別名。

大多數情況下，型別識別符號參考的是同名的命名型型別。例如型別識別符號`Int`參考命名型型別`Int`，同樣，型別識別符號`Dictionary<String, Int>`參考命名型型別`Dictionary<String, Int>`。

在兩種情況下型別識別符號參考的不是同名的型別。情況一，型別識別符號參考的是命名型/複合型型別的型別別名。比如，在下面的範例中，型別識別符號使用`Point`來參考元組`(Int, Int)`：

```swift
typealias Point = (Int, Int)
let origin: Point = (0, 0)
```

情況二，型別識別符號使用dot(`.`)語法來表示在其它模塊（modules）或其它型別嵌套內宣告的命名型型別。例如，下面範例中的型別識別符號參考在`ExampleModule`模塊中宣告的命名型型別`MyType`：

```swift
var someValue: ExampleModule.MyType
```

> 型別標識語法  
> *型別標識* → [*型別名稱*](..\chapter3\03_Types.html#type_name) [*泛型參數子句*](GenericParametersAndArguments.html#generic_argument_clause) _可選_ | [*型別名稱*](..\chapter3\03_Types.html#type_name) [*泛型參數子句*](GenericParametersAndArguments.html#generic_argument_clause) _可選_ **.** [*型別標識*](..\chapter3\03_Types.html#type_identifier)  
> *類別名* → [*識別符號*](LexicalStructure.html#identifier)  

<a name="tuple_type"></a>
##元組型別

元組型別使用逗號隔開並使用括號括起來的0個或多個型別組成的列表。

你可以使用元組型別作為一個函式的回傳型別，這樣就可以使函式回傳多個值。你也可以命名元組型別中的元素，然後用這些名字來參考每個元素的值。元素的名字由一個識別符號和`:`組成。“函式和多回傳值”章節裡有一個展示上述特性的範例。

`void`是空元組型別`()`的別名。如果括號內只有一個元素，那麼該型別就是括號內元素的型別。比如，`(Int)`的型別是`Int`而不是`(Int)`。所以，只有當元組型別包含兩個元素以上時才可以標記元組元素。

> 元組型別語法  
> *元組型別* → **(** [*元組型別主體*](..\chapter3\03_Types.html#tuple_type_body) _可選_ **)**  
> *元組型別主體* → [*元組型別的元素列表*](..\chapter3\03_Types.html#tuple_type_element_list) **...** _可選_  
> *元組型別的元素列表* → [*元組型別的元素*](..\chapter3\03_Types.html#tuple_type_element) | [*元組型別的元素*](..\chapter3\03_Types.html#tuple_type_element) **,** [*元組型別的元素列表*](..\chapter3\03_Types.html#tuple_type_element_list)  
> *元組型別的元素* → [*特性(Attributes)列表*](..\chapter3\06_Attributes.html#attributes) _可選_ **inout** _可選_ [*型別*](..\chapter3\03_Types.html#type) | **inout** _可選_ [*元素名*](..\chapter3\03_Types.html#element_name) [*型別注解*](..\chapter3\03_Types.html#type_annotation)  
> *元素名* → [*識別符號*](LexicalStructure.html#identifier)  

<a name="function_type"></a>
##函式型別

函式型別表示一個函式、方法或閉包的型別，它由一個參數型別和回傳值型別組成，中間用箭頭`->`隔開：

- `parameter type` -> `return type`

由於 *參數型別* 和 *回傳值型別* 可以是元組型別，所以函式型別可以讓函式與方法支援多參數與多回傳值。

你可以對函式型別應用帶有參數型別`()`並回傳表達式型別的`auto_closure`屬性（見型別屬性章節）。一個自動閉包函式捕獲特定表達式上的隱式閉包而非表達式本身。下面的範例使用`auto_closure`屬性來定義一個很簡單的assert函式：

```swift
func simpleAssert(condition: @auto_closure () -> Bool, message: String){
    if !condition(){
        println(message)
    }
}
let testNumber = 5
simpleAssert(testNumber % 2 == 0, "testNumber isn't an even number.")
// prints "testNumber isn't an even number."
```
函式型別可以擁有一個可變長參數作為參數型別中的最後一個參數。從語法角度上講，可變長參數由一個基礎型別名字和`...`組成，如`Int...`。可變長參數被認為是一個包含了基礎型別元素的陣列。即`Int...`就是`Int[]`。關於使用可變長參數的範例，見章節“可變長參數”。

為了指定一個`in-out`參數，可以在參數型別前加`inout`前綴。但是你不可以對可變長參數或回傳值型別使用`inout`。關於In-Out參數的討論見章節In-Out參數部分。

柯裡化函式（curried function）的型別相當於一個嵌套函式型別。例如，下面的柯裡化函式`addTwoNumber()()`的型別是`Int -> Int -> Int`：

```swift
func addTwoNumbers(a: Int)(b: Int) -> Int{
    return a + b
}
addTwoNumbers(4)(5)      // returns 9
```

柯裡化函式的函式型別從右向左組成一組。例如，函式型別`Int -> Int -> Int`可以被理解為`Int -> (Int -> Int)`——也就是說，一個函式傳入一個`Int`然後輸出作為另一個函式的輸入，然後又回傳一個`Int`。例如，你可以使用如下嵌套函式來重寫柯裡化函式`addTwoNumbers()()`：

```swift
func addTwoNumbers(a: Int) -> (Int -> Int){
    func addTheSecondNumber(b: Int) -> Int{
        return a + b
    }
    return addTheSecondNumber
}
addTwoNumbers(4)(5)     // Returns 9
```

> 函式型別語法  
> *函式型別* → [*型別*](..\chapter3\03_Types.html#type) **->** [*型別*](..\chapter3\03_Types.html#type)  

<a name="array_type"></a>
##陣列型別

Swift語言使用型別名緊接中括號`[]`來簡化標準函式庫中定義的命名型型別`Array<T>`。換句話說，下面兩個宣告是等價的：

```swift
let someArray: String[] = ["Alex", "Brian", "Dave"]
let someArray: Array<String> = ["Alex", "Brian", "Dave"]
```
上面兩種情況下，常數`someArray`都被宣告為字串陣列。陣列的元素也可以通過`[]`獲取存取：`someArray[0]`是指第0個元素`“Alex”`。

上面的範例同時顯示，你可以使用`[]`作為初始值建構陣列，空的`[]`則用來來建構指定型別的空陣列。

```swift
var emptyArray: Double[] = []
```
你也可以使用鏈接起來的多個`[]`集合來建構多維陣列。例如，下例使用三個`[]`集合來建構三維整型陣列：

```swift
var array3D: Int[][][] = [[[1, 2], [3, 4]], [[5, 6], [7, 8]]]
```
存取一個多維陣列的元素時，最左邊的下標指向最外層陣列的相應位置元素。接下來往右的下標指向第一層嵌入的相應位置元素，依次類別推。這就意味著，在上面的範例中，`array3D[0]`是指`[[1, 2], [3, 4]]`，`array3D[0][1]`是指`[3, 4]`，`array3D[0][1][1]`則是指值`4`。

關於Swift標準函式庫中`Array`型別的細節討論，見章節Arrays。

> 陣列型別語法  
> *陣列型別* → [*型別*](..\chapter3\03_Types.html#type) **[** **]** | [*陣列型別*](..\chapter3\03_Types.html#array_type) **[** **]**  

<a name="optional_type"></a>
##可選型別

Swift定義後綴`?`來作為標準函式庫中的定義的命名型型別`Optional<T>`的簡寫。換句話說，下面兩個宣告是等價的：

```swift
var optionalInteger: Int?
var optionalInteger: Optional<Int>
```
在上述兩種情況下，變數`optionalInteger`都被宣告為可選整型型別。注意在型別和`?`之間沒有空格。

型別`Optional<T>`是一個列舉，有兩種形式，`None`和`Some(T)`，又來代表可能出現或可能不出現的值。任意型別都可以被顯式的宣告（或隱式的轉換）為可選型別。當宣告一個可選型別時，確保使用括號給`?`提供合適的作用範圍。比如說，宣告一個整型的可選陣列，應寫作`(Int[])?`，寫成`Int[]?`的話則會出錯。

如果你在宣告或定義可選變數或特性的時候沒有提供初始值，它的值則會自動賦成缺省值`nil`。

可選符合`LogicValue`協定，因此可以出現在布林值環境下。此時，如果一個可選型別`T?`實例包含有型別為`T`的值（也就是說值為`Optional.Some(T)`），那麼此可選型別就為`true`，否則為`false`。

如果一個可選型別的實例包含一個值，那麼你就可以使用後綴運算子`!`來獲取該值，正如下面描述的：

```swift
optionalInteger = 42
optionalInteger!      // 42
```
使用`!`運算子獲取值為`nil`的可選項會導致執行錯誤（runtime error）。

你也可以使用可選鏈和可選綁定來選擇性的執行可選表達式上的操作。如果值為`nil`，不會執行任何操作因此也就沒有執行錯誤產生。

更多細節以及更多如何使用可選型別的範例，見章節“可選”。

> 可選型別語法  
> *可選型別* → [*型別*](..\chapter3\03_Types.html#type) **?**  

<a name="implicitly_unwrapped_optional_type"></a>
##隱式解析可選型別

Swift語言定義後綴`!`作為標準函式庫中命名型別`ImplicitlyUnwrappedOptional<T>`的簡寫。換句話說，下面兩個宣告等價：

```swift
var implicitlyUnwrappedString: String!
var implicitlyUnwrappedString: ImplicitlyUnwrappedOptional<String>
```
上述兩種情況下，變數`implicitlyUnwrappedString`被宣告為一個隱式解析可選型別的字串。注意型別與`!`之間沒有空格。

你可以在使用可選的地方同樣使用隱式解析可選。比如，你可以將隱式解析可選的值賦給變數、常數和可選特性，反之亦然。

有了可選，你在宣告隱式解析可選變數或特性的時候就不用指定初始值，因為它有缺省值`nil`。

由於隱式解析可選的值會在使用時自動解析，所以沒必要使用運算子`!`來解析它。也就是說，如果你使用值為`nil`的隱式解析可選，就會導致執行錯誤。

使用可選鏈會選擇性的執行隱式解析可選表達式上的某一個操作。如果值為`nil`，就不會執行任何操作，因此也不會產生執行錯誤。

關於隱式解析可選的更多細節，見章節“隱式解析可選”。

> 隱式解析可選型別(Implicitly Unwrapped Optional Type)語法  
> *隱式解析可選型別* → [*型別*](..\chapter3\03_Types.html#type) **!**  

<a name="protocol_composition_type"></a>
##協定合成型別

協定合成型別是一種符合每個協定的指定協定列表型別。協定合成型別可能會用在型別注解和泛型參數中。

協定合成型別的形式如下：

```swift
protocol<Protocol 1, Procotol 2>
```

協定合成型別允許你指定一個值，其型別可以適配多個協定的條件，而且不需要定義一個新的命名型協定來繼承其它想要適配的各個協定。比如，協定合成型別`protocol<Protocol A, Protocol B, Protocol C>`等效於一個從`Protocol A`，`Protocol B`， `Protocol C`繼承而來的新協定`Protocol D`，很顯然這樣做有效率的多，甚至不需引入一個新名字。

協定合成列表中的每項必須是協定名或協定合成型別的型別別名。如果列表為空，它就會指定一個空協定合成列表，這樣每個型別都能適配。

> 協定合成型別語法  
> *協定合成型別* → **protocol** **<** [*協定識別符號列表*](..\chapter3\03_Types.html#protocol_identifier_list) _可選_ **>**  
> *協定識別符號列表* → [*協定識別符號*](..\chapter3\03_Types.html#protocol_identifier) | [*協定識別符號*](..\chapter3\03_Types.html#protocol_identifier) **,** [*協定識別符號列表*](..\chapter3\03_Types.html#protocol_identifier_list)  
> *協定識別符號* → [*型別標識*](..\chapter3\03_Types.html#type_identifier)  

<a name="metatype_type"></a>
##元型別

元型別是指所有型別的型別，包括類別、結構、列舉和協定。

類別、結構或列舉型別的元型別是相應的型別名緊跟`.Type`。協定型別的元型別——並不是執行時適配該協定的具體型別——是該協定名字緊跟`.Protocol`。比如，類別`SomeClass`的元型別就是`SomeClass.Type`，協定`SomeProtocol`的元型別就是`SomeProtocal.Protocol`。

你可以使用後綴`self`表達式來獲取型別。比如，`SomeClass.self`回傳`SomeClass`本身，而不是`SomeClass`的一個實例。同樣，`SomeProtocol.self`回傳`SomeProtocol`本身，而不是執行時適配`SomeProtocol`的某個型別的實例。還可以對型別的實例使用`dynamicType`表達式來獲取該實例在執行階段的型別，如下所示：

```swift
class SomeBaseClass {
    class func printClassName() {
        println("SomeBaseClass")
    }
}
class SomeSubClass: SomeBaseClass {
    override class func printClassName() {
        println("SomeSubClass")
    }
}
let someInstance: SomeBaseClass = SomeSubClass()
// someInstance is of type SomeBaseClass at compile time, but
// someInstance is of type SomeSubClass at runtime
someInstance.dynamicType.printClassName()
// prints "SomeSubClass
```

> 元(Metatype)型別語法  
> *元型別* → [*型別*](..\chapter3\03_Types.html#type) **.** **Type** | [*型別*](..\chapter3\03_Types.html#type) **.** **Protocol**  x

<a name="type_inheritance_clause"></a>
##型別繼承子句

型別繼承子句被用來指定一個命名型型別繼承哪個類別且適配哪些協定。型別繼承子句開始於冒號`:`，緊跟由`,`隔開的型別識別符號列表。

類別可以繼承單個超類別，適配任意數量的協定。當定義一個類別時，超類別的名字必須出現在型別識別符號列表首位，然後跟上該類別需要適配的任意數量的協定。如果一個類別不是從其它類別繼承而來，那麼列表可以以協定開頭。關於類別繼承更多的討論和範例，見章節“繼承”。

其它命名型型別可能只繼承或適配一個協定列表。協定型別可能繼承於其它任意數量的協定。當一個協定型別繼承於其它協定時，其它協定的條件集合會被集成在一起，然後其它從當前協定繼承的任意型別必須適配所有這些條件。

列舉定義中的型別繼承子句可以是一個協定列表，或是指定原始值的列舉，一個單獨的指定原始值型別的命名型型別。使用型別繼承子句來指定原始值型別的列舉定義的範例，見章節“原始值”。

> 型別繼承子句語法  
> *型別繼承子句* → **:** [*型別繼承列表*](..\chapter3\03_Types.html#type_inheritance_list)  
> *型別繼承列表* → [*型別標識*](..\chapter3\03_Types.html#type_identifier) | [*型別標識*](..\chapter3\03_Types.html#type_identifier) **,** [*型別繼承列表*](..\chapter3\03_Types.html#type_inheritance_list)

<a name="type_inference"></a>
##型別推斷

Swift廣泛的使用型別推斷，從而允許你可以忽略很多變數和表達式的型別或部分型別。比如，對於`var x: Int = 0`，你可以完全忽略型別而簡寫成`var x = 0`——編譯器會正確的推斷出`x`的型別`Int`。類似的，當完整的型別可以從上下文推斷出來時，你也可以忽略型別的一部分。比如，如果你寫了`let dict: Dictionary = ["A": 1]`，編譯提也能推斷出`dict`的型別是`Dictionary<String, Int>`。

在上面的兩個範例中，型別資訊從表達式樹（expression tree）的葉子節點傳向根節點。也就是說，`var x: Int = 0`中`x`的型別首先根據`0`的型別進行推斷，然後將該型別資訊傳遞到根節點（變數`x`）。

在Swift中，型別資訊也可以反方向流動——從根節點傳向葉子節點。在下面的範例中，常數`eFloat`上的顯式型別注解（`:Float`）導致數字字面量`2.71828`的型別是`Float`而非`Double`。

```swift
let e = 2.71828 // The type of e is inferred to be Double.
let eFloat: Float = 2.71828 // The type of eFloat is Float.
```

Swift中的型別推斷在單獨的表達式或語句水平上進行。這意味著所有用於推斷型別的資訊必須可以從表達式或其某個子表達式的型別檢查中獲取。
