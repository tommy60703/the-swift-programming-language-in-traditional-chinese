> 翻譯：[sg552](https://github.com/sg552)
> 校對：[numbbbbb](https://github.com/numbbbbb), [stanzhai](https://github.com/stanzhai)

# 表達式（Expressions）
-----------------

本頁包含內容：

- [前綴表達式（Prefix Expressions）](#prefix_expressions)
- [二元表達式（Binary Expressions）](#binary_expressions)
- [賦值表達式（Assignment Operator）](#assignment_operator)
- [三元條件運算子（Ternary Conditional Operator）](#ternary_conditional_operator)
- [型別轉換運算子（Type-Casting Operators）](#type-casting_operators)
- [主要表達式（Primary Expressions）](#primary_expressions)
- [後綴表達式（Postfix Expressions）](#postfix_expressions)

Swift 中存在四種表達式： 前綴（prefix）表達式，二元（binary）表達式，主要（primary）表達式和後綴（postfix）表達式。表達式可以回傳一個值，以及執行某些邏輯（causes a side effect）。

前綴表達式和二元表達式就是對某些表達式使用各種運算子（operators）。 主要表達式是最短小的表達式，它提供了獲取（變數的）值的一種途徑。 後綴表達式則允許你建立複雜的表達式，例如配合函式呼叫和成員存取。 每種表達式都在下面有詳細論述～

> 表達式語法  
> *表達式* → [*前綴表達式*](..\chapter3\04_Expressions.html#prefix_expression) [*二元表達式列表*](..\chapter3\04_Expressions.html#binary_expressions) _可選_  
> *表達式列表* → [*表達式*](..\chapter3\04_Expressions.html#expression) | [*表達式*](..\chapter3\04_Expressions.html#expression) **,** [*表達式列表*](..\chapter3\04_Expressions.html#expression_list)  

<a name="prefix_expressions"></a>
## 前綴表達式（Prefix Expressions）

前綴表達式由 前綴符號和表達式組成。（這個前綴符號只能接收一個參數）

Swift 標準函式庫支援如下的前綴運算子：

- ++ 累加1 （increment）
- -- 累減1 （decrement）
- ! 邏輯否 （Logical NOT ）
- ~ 按位否 （Bitwise NOT ）
- \+ 加（Unary plus）
- \- 減（Unary minus）

對於這些運算子的使用，請參見： Basic Operators and Advanced Operators

作為對上面標準函式庫運算子的補充，你也可以對 某個函式的參數使用 '&'運算子。 更多資訊，請參見： "In-Out parameters".

> 前綴表達式語法  
> *前綴表達式* → [*前綴運算子*](LexicalStructure.html#prefix_operator) _可選_ [*後綴表達式*](..\chapter3\04_Expressions.html#postfix_expression)  
> *前綴表達式* → [*寫入寫出(in-out)表達式*](..\chapter3\04_Expressions.html#in_out_expression)  
> *寫入寫出(in-out)表達式* → **&** [*識別符號*](LexicalStructure.html#identifier)  

<a name="binary_expressions"></a>
## 二元表達式（Binary Expressions）

二元表達式由 "左邊參數" + "二元運算子" + "右邊參數" 組成, 它有如下的形式：

> `left-hand argument` `operator` `right-hand argument`

Swift 標準函式庫提供了如下的二元運算子：

- 求冪相關（無結合，優先級160）
  - << 按位左移（Bitwise left shift）
  - >> 按位右移（Bitwise right shift）
- 乘除法相關（左結合，優先級150）
  - \* 乘
  - / 除
  - % 取餘
  - &* 乘法，忽略溢出（ Multiply, ignoring overflow）
  - &/ 除法，忽略溢出（Divide, ignoring overflow）
  - &% 取餘, 忽略溢出（ Remainder, ignoring overflow）
  - & 位與（ Bitwise AND）
- 加減法相關（左結合, 優先級140）
  - \+ 加
  - \- 減
  - &+ Add with overflow
  - &- Subtract with overflow
  - | 按位或（Bitwise OR ）
  - ^ 按位異或（Bitwise XOR）
- Range （無結合,優先級 135）
  - .. 半閉值域 Half-closed range
  - ... 全閉值域 Closed range
- 型別轉換 （無結合,優先級 132）
  - is 型別檢查（ type check）
  - as 型別轉換（ type cast）
- Comparative （無結合,優先級 130）
  - < 小於
  - <= 小於等於
  - > 大於
  - >= 大於等於
  - == 等於
  - != 不等
  - === 恆等於
  - !== 不恆等
  - ~= 模式匹配（ Pattern match）
- 合取（ Conjunctive） （左結合,優先級 120）
  - && 邏輯且（Logical AND）
- 析取（Disjunctive） （左結合,優先級 110）
  - || 邏輯或（ Logical OR）
- 三元條件（Ternary Conditional ）（右結合,優先級 100）
  - ?: 三元條件 Ternary conditional
- 賦值 （Assignment） （右結合, 優先級 90）
  - = 賦值（Assign）
  - *=  Multiply and assign
  - /= Divide and assign
  - %= Remainder and assign
  - += Add and assign
  - -= Subtract and assign
  - <<= Left bit shift and assign
  - >>= Right bit shift and assign
  - &= Bitwise AND and assign
  - ^= Bitwise XOR and assign
  - |= Bitwise OR and assign
  - &&= Logical AND and assign
  - ||= Logical OR and assign

關於這些運算子（operators）的更多資訊，請參見：Basic Operators and Advanced Operators.

> 注意  
> 在解析時,  一個二元表達式表示為一個一級陣列（a flat list）, 這個陣列（List）根據運算子的先後順序，被轉換成了一個tree. 例如： 2 + 3 * 5 首先被認為是：  2, + , `` 3``, *, 5. 隨後它被轉換成 tree （2 + （3 * 5））

<p></p>

> 二元表達式語法  
> *二元表達式* → [*二元運算子*](LexicalStructure.html#binary_operator) [*前綴表達式*](..\chapter3\04_Expressions.html#prefix_expression)  
> *二元表達式* → [*賦值運算子*](..\chapter3\04_Expressions.html#assignment_operator) [*前綴表達式*](..\chapter3\04_Expressions.html#prefix_expression)  
> *二元表達式* → [*條件運算子*](..\chapter3\04_Expressions.html#conditional_operator) [*前綴表達式*](..\chapter3\04_Expressions.html#prefix_expression)  
> *二元表達式* → [*型別轉換運算子*](..\chapter3\04_Expressions.html#type_casting_operator)  
> *二元表達式列表* → [*二元表達式*](..\chapter3\04_Expressions.html#binary_expression) [*二元表達式列表*](..\chapter3\04_Expressions.html#binary_expressions) _可選_  


<a name="assignment_operator"></a>
## 賦值表達式（Assignment Operator）

The assigment operator sets a new value for a given expression. It has the following form:
賦值表達式會對某個給定的表達式賦值。 它有如下的形式；

> `expression` = `value`

就是把右邊的 *value* 賦值給左邊的 *expression*. 如果左邊的*expression* 需要接收多個參數（是一個tuple ），那麼右邊必須也是一個具有同樣數量參數的tuple. （允許嵌套的tuple）

```swift
(a, _, (b, c)) = ("test", 9.45, (12, 3))
// a is "test", b is 12, c is 3, and 9.45 is ignored
```

賦值運算子不回傳任何值。

> 賦值運算子語法  
> *賦值運算子* → **=**  

<a name="ternary_conditional_operator"></a>
## 三元條件運算子（Ternary Conditional Operator）

三元條件運算子 是根據條件來獲取值。 形式如下：

> `condition` ? `expression used if true` : `expression used if false`

如果 `condition` 是true, 那麼回傳 第一個表達式的值（此時不會呼叫第二個表達式）， 否則回傳第二個表達式的值（此時不會呼叫第一個表達式）。

想看三元條件運算子的範例，請參見： Ternary Conditional Operator.

> 三元條件運算子語法  
> *三元條件運算子* → **?** [*表達式*](..\chapter3\04_Expressions.html#expression) **:**  

<a name="type-casting_operators"></a>
## 型別轉換運算子（Type-Casting Operators）

有兩種型別轉換運算子： as 和 is.  它們有如下的形式：

> `expression` as `type`  
> `expression` as? `type`  
> `expression` is `type`  

as 運算子會把`目標表達式`轉換成指定的`型別`（specified type），過程如下：

- 如果型別轉換成功， 那麼目標表達式就會回傳指定型別的實例（instance）. 例如：把子類別（subclass）變成父類別（superclass）時.
- 如果轉換失敗，則會拋出編譯錯誤（ compile-time error）。
- 如果上述兩個情況都不是（也就是說，編譯器在編譯時期無法確定轉換能否成功，） 那麼目標表達式就會變成指定的型別的optional. （is an optional of the specified type ） 然後在執行時，如果轉換成功， 目標表達式就會作為 optional的一部分來回傳， 否則，目標表達式回傳nil. 對應的範例是： 把一個 superclass 轉換成一個 subclass.

```swift
class SomeSuperType {}
class SomeType: SomeSuperType {}
class SomeChildType: SomeType {}
let s = SomeType()

let x = s as SomeSuperType  // known to succeed; type is SomeSuperType
let y = s as Int            // known to fail; compile-time error
let z = s as SomeChildType  // might fail at runtime; type is SomeChildType?
```

使用'as'做型別轉換跟正常的型別宣告，對於編譯器來說是一樣的。例如：

```swift
let y1 = x as SomeType  // Type information from 'as'
let y2: SomeType = x    // Type information from an annotation
```

'is' 運算子在“執行時（runtime）”會做檢查。 成功會回傳true, 否則 false

The check must not be known to be true or false at compile time. The following are invalid:
上述檢查在“編譯時（compile time）”不能使用。 例如下面的使用是錯誤的：

```swift
"hello" is String
"hello" is Int
```

關於型別轉換的更多內容和範例，請參見： Type Casting.

> 型別轉換運算子(type-casting-operator)語法  
> *型別轉換運算子* → **is** [*型別*](..\chapter3\03_Types.html#type) | **as** **?** _可選_ [*型別*](..\chapter3\03_Types.html#type)  

<a name="primary_expressions"></a>
## 主表達式（Primary Expressions）

`主表達式`是最基本的表達式。 它們可以跟 前綴表達式，二元表達式，後綴表達式以及其他主要表達式組合使用。

> 主表達式語法  
> *主表達式* → [*識別符號*](LexicalStructure.html#identifier) [*泛型參數子句*](GenericParametersAndArguments.html#generic_argument_clause) _可選_  
> *主表達式* → [*字面量表達式*](..\chapter3\04_Expressions.html#literal_expression)  
> *主表達式* → [*self表達式*](..\chapter3\04_Expressions.html#self_expression)  
> *主表達式* → [*超類別表達式*](..\chapter3\04_Expressions.html#superclass_expression)  
> *主表達式* → [*閉包表達式*](..\chapter3\04_Expressions.html#closure_expression)  
> *主表達式* → [*圓括號表達式*](..\chapter3\04_Expressions.html#parenthesized_expression)  
> *主表達式* → [*隱式成員表達式*](..\chapter3\04_Expressions.html#implicit_member_expression)  
> *主表達式* → [*通配符表達式*](..\chapter3\04_Expressions.html#wildcard_expression)  

### 字元型表達式（Literal Expression）

由這些內容組成：普通的字元（string, number） , 一個字元的字典或者陣列，或者下面列表中的特殊字元。

字元（Literal） | 型別（Type） | 值（Value）
------------- | ---------- | ----------
\__FILE__ | String | 所在的文件名
\__LINE__ | Int | 所在的行數
\__COLUMN__ | Int | 所在的列數
\__FUNCTION__ | String | 所在的function 的名字

在某個函式（function）中，`__FUNCTION__` 會回傳當前函式的名字。 在某個方法（method）中，它會回傳當前方法的名字。 在某個property 的getter/setter中會回傳這個屬性的名字。 在init/subscript中 只有的特殊成員（member）中會回傳這個keyword的名字，在某個文件的頂端（the top level of a file），它回傳的是當前module的名字。

一個array literal，是一個有序的值的集合。 它的形式是：

> [`value 1`, `value 2`, `...`]

陣列中的最後一個表達式可以緊跟一個逗號（','）. []表示空陣列 。 array literal的type是 T[], 這個T就是陣列中元素的type. 如果該陣列中有多種type, T則是跟這些type的公共supertype最接近的type.（closest common supertype）

一個`dictionary literal` 是一個包含無序的鍵值對（key-value pairs）的集合，它的形式是:

> [`key 1`: `value 1`, `key 2`: `value 2`, `...`]

dictionary 的最後一個表達式可以是一個逗號（','）. [:] 表示一個空的dictionary. 它的type是 Dictionary<KeyType, ValueType> （這裡KeyType表示 key的type, ValueType表示 value的type） 如果這個dictionary 中包含多種 types, 那麼KeyType, Value 則對應著它們的公共supertype最接近的type（ closest common supertype）.

> 字面量表達式語法  
> *字面量表達式* → [*字面量*](LexicalStructure.html#literal)  
> *字面量表達式* → [*陣列字面量*](..\chapter3\04_Expressions.html#array_literal) | [*字典字面量*](..\chapter3\04_Expressions.html#dictionary_literal)  
> *字面量表達式* → **&#95;&#95;FILE&#95;&#95;** | **&#95;&#95;LINE&#95;&#95;** | **&#95;&#95;COLUMN&#95;&#95;** | **&#95;&#95;FUNCTION&#95;&#95;**  
> *陣列字面量* → **[** [*陣列字面量項列表*](..\chapter3\04_Expressions.html#array_literal_items) _可選_ **]**  
> *陣列字面量項列表* → [*陣列字面量項*](..\chapter3\04_Expressions.html#array_literal_item) **,** _可選_ | [*陣列字面量項*](..\chapter3\04_Expressions.html#array_literal_item) **,** [*陣列字面量項列表*](..\chapter3\04_Expressions.html#array_literal_items)  
> *陣列字面量項* → [*表達式*](..\chapter3\04_Expressions.html#expression)  
> *字典字面量* → **[** [*字典字面量項列表*](..\chapter3\04_Expressions.html#dictionary_literal_items) **]** | **[** **:** **]**  
> *字典字面量項列表* → [*字典字面量項*](..\chapter3\04_Expressions.html#dictionary_literal_item) **,** _可選_ | [*字典字面量項*](..\chapter3\04_Expressions.html#dictionary_literal_item) **,** [*字典字面量項列表*](..\chapter3\04_Expressions.html#dictionary_literal_items)  
> *字典字面量項* → [*表達式*](..\chapter3\04_Expressions.html#expression) **:** [*表達式*](..\chapter3\04_Expressions.html#expression)  

### self表達式（Self Expression）

self表達式是對 當前type 或者當前instance的參考。它的形式如下：

> self  
> self.`member name`  
> self[`subscript index`]  
> self（`initializer arguments`）  
> self.init（`initializer arguments`）  

如果在 initializer, subscript, instance method中，self等同於當前type的instance. 在一個靜態方法（static method）, 類別方法（class method）中， self等同於當前的type.

當存取 member（成員變數時）， self 用來區分重名變數（例如函式的參數）.  例如，
（下面的 self.greeting 指的是 var greeting: String, 而不是 init（greeting: String） ）

```swift
class SomeClass {
    var greeting: String
    init（greeting: String） {
        self.greeting = greeting
    }
}
```

在mutating 方法中， 你可以使用self 對 該instance進行賦值。

```swift
struct Point {
    var x = 0.0, y = 0.0
    mutating func moveByX（deltaX: Double, y deltaY: Double） {
        self = Point（x: x + deltaX, y: y + deltaY）
    }
}
```

> Self 表達式語法  
> *self表達式* → **self**  
> *self表達式* → **self** **.** [*識別符號*](LexicalStructure.html#identifier)  
> *self表達式* → **self** **[** [*表達式*](..\chapter3\04_Expressions.html#expression) **]**  
> *self表達式* → **self** **.** **init**  

### 超類別表達式（Superclass Expression）

超類別表達式可以使我們在某個class中存取它的超類別. 它有如下形式：

> super.`member name`  
> super[`subscript index`]  
> super.init（`initializer arguments`）  

形式1 用來存取超類別的某個成員（member）. 形式2 用來存取該超類別的 subscript 實作。 形式3 用來存取該超類別的 initializer.

子類別（subclass）可以通過超類別（superclass）表達式在它們的 member, subscripting 和 initializers 中來利用它們超類別中的某些實作（既有的方法或者邏輯）。

> 超類別(superclass)表達式語法  
> *超類別表達式* → [*超類別方法表達式*](..\chapter3\04_Expressions.html#superclass_method_expression) | [*超類別下標表達式*](..\chapter3\04_Expressions.html#超類別下標表達式) | [*超類別建構器表達式*](..\chapter3\04_Expressions.html#superclass_initializer_expression)  
> *超類別方法表達式* → **super** **.** [*識別符號*](LexicalStructure.html#identifier)  
> *超類別下標表達式* → **super** **[** [*表達式*](..\chapter3\04_Expressions.html#expression) **]**  
> *超類別建構器表達式* → **super** **.** **init**  

### 閉包表達式（Closure Expression）

閉包（closure） 表達式可以建立一個閉包（在其他語言中也叫 lambda, 或者 匿名函式（anonymous function））. 跟函式（function）的宣告一樣， 閉包（closure）包含了可執行的程式碼（跟方法主體（statement）類似） 以及接收（capture）的參數。 它的形式如下：

```swift
{ （parameters） -> return type in
    statements
}
```

閉包的參數宣告形式跟方法中的宣告一樣, 請參見：Function Declaration.

閉包還有幾種特殊的形式, 讓使用更加簡潔：

- 閉包可以省略 它的參數的type 和回傳值的type. 如果省略了參數和參數型別，就也要省略 'in'關鍵字。 如果被省略的type 無法被編譯器獲知（inferred） ，那麼就會拋出編譯錯誤。
- 閉包可以省略參數，轉而在方法體（statement）中使用 $0, $1, $2 來參考出現的第一個，第二個，第三個參數。
- 如果閉包中只包含了一個表達式，那麼該表達式就會自動成為該閉包的回傳值。 在執行 'type inference '時，該表達式也會回傳。

下面幾個 閉包表達式是 等價的：

```swift
myFunction {
    （x: Int, y: Int） -> Int in
    return x + y
}

myFunction {
    （x, y） in
    return x + y
}

myFunction { return $0 + $1 }

myFunction { $0 + $1 }
```

關於 向閉包中傳遞參數的內容，參見： Function Call Expression.

閉包表達式可以通過一個參數列表（capture list） 來顯式指定它需要的參數。 參數列表 由中括號 [] 括起來，裡面的參數由逗號','分隔。一旦使用了參數列表，就必須使用'in'關鍵字（在任何情況下都得這樣做，包括忽略參數的名字，type, 回傳值時等等）。

在閉包的參數列表（ capture list）中， 參數可以宣告為 'weak' 或者 'unowned' .

```swift
myFunction { print（self.title） }                    // strong capture
myFunction { [weak self] in print（self!.title） }    // weak capture
myFunction { [unowned self] in print（self.title） }  // unowned capture
```

在參數列表中，也可以使用任意表達式來賦值. 該表達式會在 閉包被執行時賦值，然後按照不同的力度來獲取（這句話請慎重理解）。（captured with the specified strength. ） 例如：

```swift
// Weak capture of "self.parent" as "parent"
myFunction { [weak parent = self.parent] in print（parent!.title） }
```

關於閉包表達式的更多資訊和範例，請參見： Closure Expressions.

> 閉包表達式語法  
> *閉包表達式* → **{** [*閉包簽名(Signational)*](..\chapter3\04_Expressions.html#closure_signature) _可選_ [*多條語句(Statements)*](..\chapter3\10_Statements.html#statements) **}**  
> *閉包簽名(Signational)* → [*參數子句*](..\chapter3\05_Declarations.html#parameter_clause) [*函式結果*](..\chapter3\05_Declarations.html#function_result) _可選_ **in**  
> *閉包簽名(Signational)* → [*識別符號列表*](LexicalStructure.html#identifier_list) [*函式結果*](..\chapter3\05_Declarations.html#function_result) _可選_ **in**  
> *閉包簽名(Signational)* → [*捕獲(Capature)列表*](..\chapter3\04_Expressions.html#capture_list) [*參數子句*](..\chapter3\05_Declarations.html#parameter_clause) [*函式結果*](..\chapter3\05_Declarations.html#function_result) _可選_ **in**  
> *閉包簽名(Signational)* → [*捕獲(Capature)列表*](..\chapter3\04_Expressions.html#capture_list) [*識別符號列表*](LexicalStructure.html#identifier_list) [*函式結果*](..\chapter3\05_Declarations.html#function_result) _可選_ **in**  
> *閉包簽名(Signational)* → [*捕獲(Capature)列表*](..\chapter3\04_Expressions.html#capture_list) **in**  
> *捕獲(Capature)列表* → **[** [*捕獲(Capature)說明符*](..\chapter3\04_Expressions.html#capture_specifier) [*表達式*](..\chapter3\04_Expressions.html#expression) **]**  
> *捕獲(Capature)說明符* → **weak** | **unowned** | **unowned(safe)** | **unowned(unsafe)**  

### 隱式成員表達式（Implicit Member Expression）

在可以判斷出型別（type）的上下文（context）中，隱式成員表達式是存取某個type的member（ 例如 class method, enumeration case） 的簡潔方法。 它的形式是：

> .`member name`

範例：

```swift
var x = MyEnumeration.SomeValue
x = .AnotherValue
```

> 隱式成員表達式語法  
> *隱式成員表達式* → **.** [*識別符號*](..\chapter3\02_Lexical_Structure.html#identifier)  

### 圓括號表達式（Parenthesized Expression）

圓括號表達式由多個子表達式和逗號','組成。 每個子表達式前面可以有 identifier x: 這樣的可選前綴。形式如下：

>（`identifier 1`: `expression 1`, `identifier 2`: `expression 2`, `...`）

圓括號表達式用來建立tuples ， 然後把它做為參數傳遞給 function. 如果某個圓括號表達式中只有一個 子表達式，那麼它的type就是 子表達式的type。例如： （1）的 type是Int, 而不是（Int）

> 圓括號表達式(Parenthesized Expression)語法  
> *圓括號表達式* → **(** [*表達式元素列表*](..\chapter3\04_Expressions.html#expression_element_list) _可選_ **)**  
> *表達式元素列表* → [*表達式元素*](..\chapter3\04_Expressions.html#expression_element) | [*表達式元素*](..\chapter3\04_Expressions.html#expression_element) **,** [*表達式元素列表*](..\chapter3\04_Expressions.html#expression_element_list)  
> *表達式元素* → [*表達式*](..\chapter3\04_Expressions.html#expression) | [*識別符號*](..\chapter3\02_Lexical_Structure.html#identifier) **:** [*表達式*](..\chapter3\04_Expressions.html#expression)  

### 通配符表達式（Wildcard Expression）

通配符表達式用來忽略傳遞進來的某個參數。例如：下面的程式碼中，10被傳遞給x, 20被忽略（譯注：好奇葩的語法。。。）

```swift
（x, _） = （10, 20）
// x is 10, 20 is ignored
```

> 通配符表達式語法  
> *通配符表達式* → **_**  

<a name="postfix_expressions"></a>
## 後綴表達式（Postfix Expressions）

後綴表達式就是在某個表達式的後面加上 運算子。 嚴格的講，每個主要表達式（primary expression）都是一個後綴表達式

Swift 標準函式庫提供了下列後綴表達式：

- ++ Increment
- -- Decrement

對於這些運算子的使用，請參見： Basic Operators and Advanced Operators

> 後綴表達式語法  
> *後綴表達式* → [*主表達式*](..\chapter3\04_Expressions.html#primary_expression)  
> *後綴表達式* → [*後綴表達式*](..\chapter3\04_Expressions.html#postfix_expression) [*後綴運算子*](..\chapter3\02_Lexical_Structure.html#postfix_operator)  
> *後綴表達式* → [*函式呼叫表達式*](..\chapter3\04_Expressions.html#function_call_expression)  
> *後綴表達式* → [*建構器表達式*](..\chapter3\04_Expressions.html#initializer_expression)  
> *後綴表達式* → [*顯示成員表達式*](..\chapter3\04_Expressions.html#explicit_member_expression)  
> *後綴表達式* → [*後綴self表達式*](..\chapter3\04_Expressions.html#postfix_self_expression)  
> *後綴表達式* → [*動態型別表達式*](..\chapter3\04_Expressions.html#dynamic_type_expression)  
> *後綴表達式* → [*下標表達式*](..\chapter3\04_Expressions.html#subscript_expression)  
> *後綴表達式* → [*強制取值(Forced Value)表達式*](..\chapter3\04_Expressions.html#forced_value_expression)  
> *後綴表達式* → [*可選鏈(Optional Chaining)表達式*](..\chapter3\04_Expressions.html#optional_chaining_expression)  

### 函式呼叫表達式（Function Call Expression）

函式呼叫表達式由函式名和參數列表組成。它的形式如下：

> `function name`（`argument value 1`, `argument value 2`）

The function name can be any expression whose value is of a function type.
（不用翻譯了, 太羅嗦）

如果該function 的宣告中指定了參數的名字，那麼在呼叫的時候也必須得寫出來. 例如：

> `function name`（`argument name 1`: `argument value 1`, `argument name 2`: `argument value 2`）

可以在 函式呼叫表達式的尾部（最後一個參數之後）加上 一個閉包（closure） ， 該閉包會被目標函式理解並執行。它具有如下兩種寫法：

```swift
// someFunction takes an integer and a closure as its arguments
someFunction（x, {$0 == 13}）
someFunction（x） {$0 == 13}
```

如果閉包是該函式的唯一參數，那麼圓括號可以省略。

```swift
// someFunction takes a closure as its only argument
myData.someMethod（） {$0 == 13}
myData.someMethod {$0 == 13}
```

> 函式呼叫表達式語法  
> *函式呼叫表達式* → [*後綴表達式*](..\chapter3\04_Expressions.html#postfix_expression) [*圓括號表達式*](..\chapter3\04_Expressions.html#parenthesized_expression)  
> *函式呼叫表達式* → [*後綴表達式*](..\chapter3\04_Expressions.html#postfix_expression) [*圓括號表達式*](..\chapter3\04_Expressions.html#parenthesized_expression) _可選_ [*後綴閉包(Trailing Closure)*](..\chapter3\04_Expressions.html#trailing_closure)  
> *後綴閉包(Trailing Closure)* → [*閉包表達式*](..\chapter3\04_Expressions.html#closure_expression)  

### 初始化函式表達式（Initializer Expression）

Initializer表達式用來給某個Type初始化。 它的形式如下：

> `expression`.init（`initializer arguments`）

（Initializer表達式用來給某個Type初始化。） 跟函式（function）不同， initializer 不能回傳值。

```swift
var x = SomeClass.someClassFunction // ok
var y = SomeClass.init              // error
```

可以通過 initializer 表達式來委托呼叫（delegate to ）到superclass的initializers.

```swift
class SomeSubClass: SomeSuperClass {
    init（） {
        // subclass initialization goes here
        super.init（）
    }
}
```

> 建構器表達式語法  
> *建構器表達式* → [*後綴表達式*](..\chapter3\04_Expressions.html#postfix_expression) **.** **init**  

### 顯式成員表達式（Explicit Member Expression）

顯示成員表達式允許我們存取type, tuple, module的成員變數。它的形式如下：

> `expression`.`member name`

該member 就是某個type在宣告時候所定義（declaration or extension） 的變數, 例如：

```swift
class SomeClass {
    var someProperty = 42
}
let c = SomeClass（）
let y = c.someProperty  // Member access
```

對於tuple, 要根據它們出現的順序（0, 1, 2...）來使用:

```swift
var t = （10, 20, 30）
t.0 = t.1
// Now t is （20, 20, 30）
```

The members of a module access the top-level declarations of that module.
（不確定：對於某個module的member的呼叫，只能呼叫在top-level宣告中的member.）

> 顯式成員表達式語法  
> *顯示成員表達式* → [*後綴表達式*](..\chapter3\04_Expressions.html#postfix_expression) **.** [*十進制數字*](..\chapter3\02_Lexical_Structure.html#decimal_digit)  
> *顯示成員表達式* → [*後綴表達式*](..\chapter3\04_Expressions.html#postfix_expression) **.** [*識別符號*](..\chapter3\02_Lexical_Structure.html#identifier) [*泛型參數子句*](GenericParametersAndArguments.html#generic_argument_clause) _可選_  

### 後綴self表達式（Postfix Self Expression）

後綴表達式由 某個表達式 + '.self' 組成. 形式如下：

> `expression`.self  
> `type`.self  

形式1 表示會回傳 expression 的值。例如： x.self 回傳 x

形式2：回傳對應的type。我們可以用它來動態的獲取某個instance的type。

> 後綴Self 表達式語法  
> *後綴self表達式* → [*後綴表達式*](..\chapter3\04_Expressions.html#postfix_expression) **.** **self**  

### dynamic表達式（Dynamic Type Expression）

（因為dynamicType是一個獨有的方法，所以這裡保留了英文單詞，未作翻譯, --- 類似與self expression）

dynamicType 表達式由 某個表達式 + '.dynamicType' 組成。

> `expression`.dynamicType

上面的形式中， expression 不能是某type的名字（當然了，如果我都知道它的名字了還需要動態來獲取它嗎）。動態型別表達式會回傳"執行時"某個instance的type, 具體請看下面的列子：

```swift
class SomeBaseClass {
    class func printClassName（） {
        println（"SomeBaseClass"）
    }
}
class SomeSubClass: SomeBaseClass {
    override class func printClassName（） {
        println（"SomeSubClass"）
    }
}
let someInstance: SomeBaseClass = SomeSubClass（）

// someInstance is of type SomeBaseClass at compile time, but
// someInstance is of type SomeSubClass at runtime
someInstance.dynamicType.printClassName（）
// prints "SomeSubClass"
```

> 動態型別表達式語法  
> *動態型別表達式* → [*後綴表達式*](..\chapter3\04_Expressions.html#postfix_expression) **.** **dynamicType**  

### 下標腳本表達式（Subscript Expression）

下標腳本表達式提供了通過下標腳本存取getter/setter 的方法。它的形式是：

> `expression`[`index expressions`]

可以通過下標腳本表達式通過getter獲取某個值，或者通過setter賦予某個值.

關於subscript的宣告，請參見： Protocol Subscript Declaration.

> 附屬腳本表達式語法  
> *附屬腳本表達式* → [*後綴表達式*](..\chapter3\04_Expressions.html#postfix_expression) **[** [*表達式列表*](..\chapter3\04_Expressions.html#expression_list) **]**  

### 強制取值表達式（Forced-Value Expression）

強制取值表達式用來獲取某個目標表達式的值（該目標表達式的值必須不是nil ）。它的形式如下：

> `expression`!

如果該表達式的值不是nil, 則回傳對應的值。 否則，拋出執行時錯誤（runtime error）。

> 強制取值(Forced Value)語法  
> *強制取值(Forced Value)表達式* → [*後綴表達式*](..\chapter3\04_Expressions.html#postfix_expression) **!**  

### 可選鏈表達式（Optional-Chaining Expression）

可選鏈表達式由目標表達式 + '?' 組成，形式如下：

> `expression`?

後綴'?' 回傳目標表達式的值，把它做為可選的參數傳遞給後續的表達式

如果某個後綴表達式包含了可選鏈表達式，那麼它的執行過程就比較特殊： 首先先判斷該可選鏈表達式的值，如果是 nil, 整個後綴表達式都回傳 nil, 如果該可選鏈的值不是nil, 則正常回傳該後綴表達式的值（依次執行它的各個子表達式）。在這兩種情況下，該後綴表達式仍然是一個optional type（In either case, the value of the postfix expression is still of an optional type）

如果某個"後綴表達式"的"子表達式"中包含了"可選鏈表達式"，那麼只有最外層的表達式回傳的才是一個optional type. 例如，在下面的範例中， 如果c 不是nil, 那麼 c?.property.performAction（） 這句程式碼在執行時，就會先獲得c 的property方法，然後呼叫 performAction（）方法。 然後對於 "c?.property.performAction（）" 這個整體，它的回傳值是一個optional type.

```swift
var c: SomeClass?
var result: Bool? = c?.property.performAction（）
```

如果不使用可選鏈表達式，那麼 上面範例的程式碼跟下面範例等價：

```swift
if let unwrappedC = c {
    result = unwrappedC.property.performAction（）
}
```

> 可選鏈表達式語法  
> *可選鏈表達式* → [*後綴表達式*](..\chapter3\04_Expressions.html#postfix_expression) **?**  
