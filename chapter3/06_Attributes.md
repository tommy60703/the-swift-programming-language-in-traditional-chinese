> 翻譯：[Hawstein](https://github.com/Hawstein)
> 校對：[numbbbbb](https://github.com/numbbbbb), [stanzhai](https://github.com/stanzhai)

# 特性
-----------------

本頁內容包括：

- [宣告特性](#declaration_attributes)
- [型別特性](#type_attributes)

特性提供了關於宣告和型別的更多資訊。在Swift中有兩類別特性，用於修飾宣告的以及用於修飾型別的。例如，`required`特性，當應用於一個類別的指定或便利初始化器宣告時，表明它的每個子類別都必須實作那個初始化器。再比如`noreturn`特性，當應用於函式或方法型別時，表明該函式或方法不會回傳到它的呼叫者。

通過以下方式指定一個特性：符號`@`後面跟特性名，如果包含參數，則把參數帶上：

> @`attribute name`  
> @`attribute name`(`attribute arguments`)  

有些宣告特性通過接收參數來指定特性的更多資訊以及它是如何修飾一個特定的宣告的。這些特性的參數寫在小括號內，它們的格式由它們所屬的特性來定義。

<a name="declaration_attributes"></a>
## 宣告特性

宣告特性只能應用於宣告。然而，你也可以將`noreturn`特性應用於函式或方法型別。

`assignment`

該特性用於修飾重載了複合賦值運算子的函式。重載了複合賦值運算子的函式必需將它們的初始輸入參數標記為`inout`。如何使用`assignment`特性的一個範例，請見：[複合賦值運算子]()。

`class_protocol`

該特性用於修飾一個協定表明該協定只能被類型別采用[待改：adopted]。

如果你用`objc`特性修飾一個協定，`class_protocol`特性就會隱式地應用到該協定，因此無需顯式地用`class_protocol`特性標記該協定。

`exported`

該特性用於修飾導入宣告，以此來導出已導入的模塊，子模塊，或當前模塊的宣告。如果另一個模塊導入了當前模塊，那麼那個模塊可以存取當前模塊的導出項。

`final`

該特性用於修飾一個類別或類別中的屬性，方法，以及下標成員。如果用它修飾一個類別，那麼這個類別則不能被繼承。如果用它修飾類別中的屬性，方法或下標，則表示在子類別中，它們不能被重寫。

`lazy`

該特性用於修飾類別或結構中的儲存型變數屬性，表示該屬性的初始值最多只被計算和儲存一次，且發生在第一次存取它時。如何使用`lazy`特性的一個範例，請見：[惰性儲存型屬性]()。

`noreturn`

該特性用於修飾函式或方法宣告，表明該函式或方法的對應型別，`T`，是`@noreturn T`。你可以用這個特性修飾函式或方法的型別，這樣一來，函式或方法就不會回傳到它的呼叫者中去。

對於一個沒有用`noreturn`特性標記的函式或方法，你可以將它重寫(override)為用該特性標記的。相反，對於一個已經用`noreturn`特性標記的函式或方法，你則不可以將它重寫為沒使用該特性標記的。相同的規則試用於當你在一個comforming型別中實作一個協定方法時。

`NSCopying`

該特性用於修飾一個類別的儲存型變數屬性。該特性將使屬性的setter與屬性值的一個副本合成，由`copyWithZone`方法回傳，而不是屬性本身的值。該屬性的型別必需遵循`NSCopying`協定。

`NSCopying`特性的行為與Objective-C中的`copy`特性相似。

`NSManaged`

該特性用於修飾`NSManagedObject`子類別中的儲存型變數屬性，表明屬性的儲存和實作由Core Data在執行時基於相關實體描述動態提供。

`objc`

該特性用於修飾任意可以在Objective-C中表示的宣告，比如，非嵌套類別，協定，類別和協定中的屬性和方法（包含getter和setter），初始化器，析構器，以下下標。`objc`特性告訴編譯器該宣告可以在Objective-C程式碼中使用。

如果你將`objc`特性應用於一個類別或協定，它也會隱式地應用於那個類別或協定的成員。對於標記了`objc`特性的類別，編譯器會隱式地為它的子類別添加`objc`特性。標記了`objc`特性的協定不能繼承自沒有標記`objc`的協定。

`objc`特性有一個可選的參數，由標記符組成。當你想把`objc`所修飾的實體以一個不同的名字暴露給Objective-C，你就可以使用這個特性參數。你可以使用這個參數來命名類別，協定，方法，getters，setters，以及初始化器。下面的範例把`ExampleClass`中`enabled`屬性的getter暴露給Objective-C，名字是`isEnabled`，而不是它原來的屬性名。

```swift
@objc
class ExampleClass {
    var enabled: Bool {
    @objc(isEnabled) get {
        // Return the appropriate value
    }
    }
}
```

`optional`

用該特性修飾協定的屬性，方法或下標成員，表示實作這些成員並不需要一致性型別（conforming type）。

你只能用`optional`特性修飾那些標記了`objc`特性的協定。因此，只有類型別可以adopt和comform to那些包含可選成員需求的協定。更多關於如何使用`optional`特性以及如何存取可選協定成員的指導，例如，當你不確定一個conforming型別是否實作了它們，請見：[可選協定需求]()。

`required`

用該特性修飾一個類別的指定或便利初始化器，表示該類別的所有子類別都必需實作該初始化器。

加了該特性的指定初始化器必需顯式地實作，而便利初始化器既可顯式地實作，也可以在子類別實作了超類別所有指定初始化器後繼承而來（或者當子類別使用便利初始化器重寫了指定初始化器）。

### Interface Builder使用的宣告特性

Interface Builder特性是Interface Builder用來與Xcode同步的宣告特性。Swift提供了以下的Interface Builder特性：`IBAction`，`IBDesignable`，`IBInspectable`，以及`IBOutlet`。這些特性與Objective-C中對應的特性在概念上是相同的。

`IBOutlet`和`IBInspectable`用於修飾一個類別的屬性宣告；`IBAction`特性用於修飾一個類別的方法宣告；`IBDesignable`用於修飾類別的宣告。

<a name="type_attributes"></a>
## 型別特性

型別特性只能用於修飾型別。然而，你也可以用`noreturn`特性去修飾函式或方法宣告。

`auto_closure`

這個特性通過自動地將表達式封閉到一個無參數閉包中來延遲表達式的求值。使用該特性修飾無參的函式或方法型別，回傳表達式的型別。一個如何使用`auto_closure`特性的範例，見[函式型別]()

`noreturn`

該特性用於修飾函式或方法的型別，表明該函式或方法不會回傳到它的呼叫者中去。你也可以用它標記函式或方法的宣告，表示函式或方法的相應型別，`T`，是`@noreturn T`。

> 特性語法  
> *特性* → **@** [*特性名*](..\chapter3\06_Attributes.html#attribute_name) [*特性參數子句*](..\chapter3\06_Attributes.html#attribute_argument_clause) _可選_  
> *特性名* → [*識別符號*](LexicalStructure.html#identifier)  
> *特性參數子句* → **(** [*平衡語彙單元列表*](..\chapter3\06_Attributes.html#balanced_tokens) _可選_ **)**  
> *特性(Attributes)列表* → [*特色*](..\chapter3\06_Attributes.html#attribute) [*特性(Attributes)列表*](..\chapter3\06_Attributes.html#attributes) _可選_  
> *平衡語彙單元列表* → [*平衡語彙單元*](..\chapter3\06_Attributes.html#balanced_token) [*平衡語彙單元列表*](..\chapter3\06_Attributes.html#balanced_tokens) _可選_  
> *平衡語彙單元* → **(** [*平衡語彙單元列表*](..\chapter3\06_Attributes.html#balanced_tokens) _可選_ **)**  
> *平衡語彙單元* → **[** [*平衡語彙單元列表*](..\chapter3\06_Attributes.html#balanced_tokens) _可選_ **]**  
> *平衡語彙單元* → **{** [*平衡語彙單元列表*](..\chapter3\06_Attributes.html#balanced_tokens) _可選_ **}**  
> *平衡語彙單元* → **任意識別符號, 關鍵字, 字面量或運算子**  
> *平衡語彙單元* → **任意標點除了(, ), [, ], {, 或 }**
