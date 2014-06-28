> 翻譯：[marsprince](https://github.com/marsprince)
> 校對：[numbbbbb](https://github.com/numbbbbb), [stanzhai](https://github.com/stanzhai)

# 宣告
-----------------

本頁包含內容：

- [模塊範圍](#module_scope)
- [程式碼區塊](#code_blocks)
- [引入宣告](#import_declaration)
- [常數宣告](#constant_declaration)
- [變數宣告](#variable_declaration)
- [型別的別名宣告](#type_alias_declaration)
- [函式宣告](#function_declaration)
- [列舉宣告](#enumeration_declaration)
- [結構宣告](#structure_declaration)
- [類別宣告](#class_declaration)
- [協定宣告](#protocol_declaration)
- [建構器宣告](#initializer_declaration)
- [析構宣告](#deinitializer_declaration)
- [擴展宣告](#extension_declaration)
- [下標腳本宣告](#subscript_declaration)
- [運算子宣告](#operator_declaration)

一條宣告可以在你的程式裡引入新的名字和建構。舉例來說，你可以使用宣告來引入函式和方法，變數和常數，或者來定義
新的命名好的列舉，結構，類別和協定型別。你也可以使用一條宣告來延長一個已經存在的命名好的型別的行為。或者在你的
程式裡引入在其他地方宣告的符號。

在swift中，大多數宣告在某種意義上講也是執行或同事宣告它們的初始化定義。這意味著，因為協定和它們的成員不匹配，
大多數協定成員需要單獨的宣告。為了方便起見，也因為這些區別在swift裡不是很重要，宣告語句同時包含了宣告和定義。

> 宣告語法  
> *宣告* → [*導入宣告*](..\chapter3\05_Declarations.html#import_declaration)  
> *宣告* → [*常數宣告*](..\chapter3\05_Declarations.html#constant_declaration)  
> *宣告* → [*變數宣告*](..\chapter3\05_Declarations.html#variable_declaration)  
> *宣告* → [*型別別名宣告*](..\chapter3\05_Declarations.html#typealias_declaration)  
> *宣告* → [*函式宣告*](..\chapter3\05_Declarations.html#function_declaration)  
> *宣告* → [*列舉宣告*](..\chapter3\05_Declarations.html#enum_declaration)  
> *宣告* → [*結構宣告*](..\chapter3\05_Declarations.html#struct_declaration)  
> *宣告* → [*類別宣告*](..\chapter3\05_Declarations.html#class_declaration)  
> *宣告* → [*協定宣告*](..\chapter3\05_Declarations.html#protocol_declaration)  
> *宣告* → [*建構器宣告*](..\chapter3\05_Declarations.html#initializer_declaration)  
> *宣告* → [*析構器宣告*](..\chapter3\05_Declarations.html#deinitializer_declaration)  
> *宣告* → [*擴展宣告*](..\chapter3\05_Declarations.html#extension_declaration)  
> *宣告* → [*附屬腳本宣告*](..\chapter3\05_Declarations.html#subscript_declaration)  
> *宣告* → [*運算子宣告*](..\chapter3\05_Declarations.html#operator_declaration)  
> *宣告(Declarations)列表* → [*宣告*](..\chapter3\05_Declarations.html#declaration) [*宣告(Declarations)列表*](..\chapter3\05_Declarations.html#declarations) _可選_  
> *宣告描述符(Specifiers)列表* → [*宣告描述符(Specifier)*](..\chapter3\05_Declarations.html#declaration_specifier) [*宣告描述符(Specifiers)列表*](..\chapter3\05_Declarations.html#declaration_specifiers) _可選_  
> *宣告描述符(Specifier)* → **class** | **mutating** | **nonmutating** | **override** | **static** | **unowned** | **unowned(safe)** | **unowned(unsafe)** | **weak**  

<a name="module_scope"></a>
##模塊範圍

模塊範圍定義了對模塊中其他原始碼可見的程式碼。（注：待改進）在swift的原始碼中，最高級別的程式碼由零個或多個語句，
宣告和表達組成。變數，常數和其他的宣告語句在一個原始碼的最頂級被宣告，使得它們對同一模塊中的每個原始碼都是可見的。

> 頂級(Top Level) 宣告語法  
> *頂級宣告* → [*多條語句(Statements)*](..\chapter3\10_Statements.html#statements) _可選_  

<a name="code_blocks"></a>
##程式碼區塊

程式碼區塊用來將一些宣告和控制結構的語句組織在一起。它有如下的形式：

> {  
>     `statements`  
> }  

程式碼區塊中的語句包括宣告，表達式和各種其他型別的語句，它們按照在源碼中的出現順序被依次執行。

> 程式碼區塊語法  
> *程式碼區塊* → **{** [*多條語句(Statements)*](..\chapter3\10_Statements.html#statements) _可選_ **}**  

<a name="import_declaration"></a>
##引入宣告

引入宣告使你可以使用在其他文件中宣告的內容。引入語句的基本形式是引入整個程式碼模塊；它由import關鍵字開始，後面
緊跟一個模塊名：

> import `module`

你可以提供更多的細節來限制引入的符號，如宣告一個特殊的子模塊或者在一個模塊或子模塊中做特殊的宣告。（待改進）
當你使用了這些細節後，在當前的程式彙總只有引入的符號是可用的（並不是宣告的整個模塊）。

> import `import kind` `module`.`symbol name`  
> import `module`.`submodule`  

<p></p>

> 導入(Import)宣告語法  
> *導入宣告* → [*特性(Attributes)列表*](..\chapter3\06_Attributes.html#attributes) _可選_ **import** [*導入型別*](..\chapter3\05_Declarations.html#import_kind) _可選_ [*導入路徑*](..\chapter3\05_Declarations.html#import_path)  
> *導入型別* → **typealias** | **struct** | **class** | **enum** | **protocol** | **var** | **func**  
> *導入路徑* → [*導入路徑識別符號*](..\chapter3\05_Declarations.html#import_path_identifier) | [*導入路徑識別符號*](..\chapter3\05_Declarations.html#import_path_identifier) **.** [*導入路徑*](..\chapter3\05_Declarations.html#import_path)  
> *導入路徑識別符號* → [*識別符號*](LexicalStructure.html#identifier) | [*運算子*](LexicalStructure.html#operator)  

<a name="constant_declaration"></a>
##常數宣告

常數宣告可以在你的程式裡命名一個常數。常數以關鍵詞let來宣告，遵循如下的格式:

> let `constant name`: `type` = `expression`

當常數的值被給定後，常數就將常數名稱和表達式初始值不變的結合在了一起，而且不能更改。
這意味著如果常數以類別的形式被初始化，類別本身的內容是可以改變的，但是常數和類別之間的結合關系是不能改變的。
當一個常數被宣告為全域變數，它必須被給定一個初始值。當一個常數在類別或者結構中被宣告時，它被認為是一個常數
屬性。常數並不是可計算的屬性，因此不包含getters和setters。（譯者注：getters和setters不知道怎麼翻譯，待改進）

如果常數名是一個元祖形式，元祖中的每一項初始化表達式中都要有對應的值

```swift
let (firstNumber, secondNumber) = (10, 42)
```

在上例中，firstNumber是一個值為10的常數，secnodeName是一個值為42的常數。所有常數都可以獨立的使用：

```swift
println("The first number is \(firstNumber).")
// prints "The first number is 10."
println("The second number is \(secondNumber).")
// prints "The second number is 42."
```

型別註解（:type）在常數宣告中是一個可選項，它可以用來描述在型別推斷（type inference）中找到的型別。

宣告一個靜態常數要使用關鍵字static。靜態屬性在型別屬性（type propetries）中有介紹。

如果還想獲得更多關於常數的資訊或者想在使用中獲得幫助，請查看常數和變數（constants and variables）,
儲存屬性（stored properties）等節。

> 常數宣告語法  
> *常數宣告* → [*特性(Attributes)列表*](..\chapter3\06_Attributes.html#attributes) _可選_ [*宣告描述符(Specifiers)列表*](..\chapter3\05_Declarations.html#declaration_specifiers) _可選_ **let** [*模式建構器列表*](..\chapter3\05_Declarations.html#pattern_initializer_list)  
> *模式建構器列表* → [*模式建構器*](..\chapter3\05_Declarations.html#pattern_initializer) | [*模式建構器*](..\chapter3\05_Declarations.html#pattern_initializer) **,** [*模式建構器列表*](..\chapter3\05_Declarations.html#pattern_initializer_list)  
> *模式建構器* → [*模式*](..\chapter3\07_Patterns.html#pattern) [*建構器*](..\chapter3\05_Declarations.html#initializer) _可選_  
> *建構器* → **=** [*表達式*](..\chapter3\04_Expressions.html#expression)  

<a name="variable_declaration"></a>
##變數宣告

變數宣告可以在你的程式裡宣告一個變數，它以關鍵字var來宣告。根據宣告變數型別和值的不同，如儲存和計算
變數和屬性，儲存變數和屬性監視，和靜態變數屬性，有著不同的宣告形式。（待改進）
所使用的宣告形式取決於變數所宣告的範圍和你打算宣告的變數型別。

>注意：  
你也可以在協定宣告的上下文宣告屬性，詳情參見型別屬性宣告。

###儲存型變數和儲存型屬性

下面的形式宣告了一個儲存型變數或儲存型變數屬性

> var `variable name`: `type` = `expression`

你可以在全域，函式內，或者在類別和結構的宣告(context)中使用這種形式來宣告一個變數。當變數以這種形式
在全域或者一個函式內被宣告時，它代表一個儲存型變數。當它在類別或者結構中被宣告時，它代表一個儲存型變數屬性。

建構器表達式可以被

和常數宣告相比，如果變數名是一個元祖型別，元祖的每一項的名字都要和初始化表達式一致。

正如名字一樣，儲存型變數的值或儲存型變數屬性儲存在內存中。

###計算型變數和計算型屬性

如下形式宣告一個一個儲存型變數或儲存型屬性：

> var `variable name`: `type` {  
> get {  
>     `statements`  
> }  
> set(`setter name`) {  
>     `statements`  
> }  
> }  

你可以在全域，函式體內或者類別，結構，列舉，擴展宣告的上下文中使用這種形式的宣告。
當變數以這種形式在全域或者一個函式內被宣告時，它代表一個計算型變數。當它在類別，結構，列舉，擴展宣告的上下文
中中被宣告時，它代表一個計算型變數屬性。

getter用來讀取變數值，setter用來寫入變數值。setter子句是可選擇的，只有getter是必需的，你可以將這些語句
都省略，只是簡單的直接回傳請求值，正如在唯讀計算屬性(read-only computed properites)中描述的那樣。
但是如果你提供了一個setter語句，你也必需提供一個getter語句。

setter的名字和圓括號內的語句是可選的。如果你寫了一個setter名，它就會作為setter的參數被使用。如果你不寫setter名，
setter的初始名為newValue，正如在seter宣告速記(shorthand setter declaration)中提到的那樣。

不像儲存型變數和儲存型屬性那樣，計算型屬性和計算型變數的值不儲存在內存中。

獲得更多資訊，查看更多關於計算型屬性的範例，請查看計算型屬性(computed properties)一節。

###儲存型變數監視器和屬性監視器

你可以用willset和didset監視器來宣告一個儲存型變數或屬性。一個包含監視器的儲存型變數或屬性按如下的形式宣告：

> var `variable name`: `type` = expression {  
> willSet(setter name) {  
>     `statements`  
> }  
> didSet(`setter name`) {  
>     `statements`  
> }  
> }  

你可以在全域，函式體內或者類別，結構，列舉，擴展宣告的上下文中使用這種形式的宣告。
當變數以這種形式在全域或者一個函式內被宣告時，監視器代表一個儲存型變數監視器；
當它在類別，結構，列舉，擴展宣告的上下文中被宣告時，監視器代表屬性監視器。

你可以為適合的監視器添加任何儲存型屬性。你也可以通過重寫子類別屬性的方式為適合的監視器添加任何繼承的屬性
(無論是儲存型還是計算型的)，參見重寫屬性監視器(overriding properyt observers)。

初始化表達式在類別或者結構的宣告中是可選的，但是在其他地方是必需的。無論在什麼地方宣告，
所有包含監視器的變數宣告都必須有型別註解(type annotation)。

當變數或屬性的值被改變時，willset和didset監視器提供了一個監視方法（適當的回應）。
監視器不會在變數或屬性第一次初始化時不會被執行，它們只有在值被外部初始化語句改變時才會被執行。

willset監視器只有在變數或屬性值被改變之前執行。新的值作為一個常數經過過willset監視器，因此不可以在
willset語句中改變它。didset監視器在變數或屬性值被改變後立即執行。和willset監視器相反，為了以防止你仍然
需要獲得舊的資料，舊變數值或者屬性會經過didset監視器。這意味著，如果你在變數或屬性自身的didiset監視器語句
中設置了一個值，你設置的新值會取代剛剛在willset監視器中經過的那個值。

在willset和didset語句中，setter名和圓括號的語句是可選的。如果你寫了一個setter名，它就會作為willset和didset的參數被使用。如果你不寫setter名，
willset監視器初始名為newvalue，didset監視器初始名為oldvalue。

當你提供一個willset語句時，didset語句是可選的。同樣的，在你提供了一個didset語句時，willset語句是可選的。

獲得更多資訊，查看如何使用屬性監視器的範例，請查看屬性監視器(prpperty observers)一節。

###類別和靜態變數屬性

class關鍵字用來宣告類別的計算型屬性。static關鍵字用來宣告類別的靜態變數屬性。類別和靜態變數在型別屬性(type properties)中有詳細討論。

> 變數宣告語法  
> *變數宣告* → [*變數宣告頭(Head)*](..\chapter3\05_Declarations.html#variable_declaration_head) [*模式建構器列表*](..\chapter3\05_Declarations.html#pattern_initializer_list)  
> *變數宣告* → [*變數宣告頭(Head)*](..\chapter3\05_Declarations.html#variable_declaration_head) [*變數名*](..\chapter3\05_Declarations.html#variable_name) [*型別注解*](..\chapter3\03_Types.html#type_annotation) [*程式碼區塊*](..\chapter3\05_Declarations.html#code_block)  
> *變數宣告* → [*變數宣告頭(Head)*](..\chapter3\05_Declarations.html#variable_declaration_head) [*變數名*](..\chapter3\05_Declarations.html#variable_name) [*型別注解*](..\chapter3\03_Types.html#type_annotation) [*getter-setter塊*](..\chapter3\05_Declarations.html#getter_setter_block)  
> *變數宣告* → [*變數宣告頭(Head)*](..\chapter3\05_Declarations.html#variable_declaration_head) [*變數名*](..\chapter3\05_Declarations.html#variable_name) [*型別注解*](..\chapter3\03_Types.html#type_annotation) [*getter-setter關鍵字(Keyword)塊*](..\chapter3\05_Declarations.html#getter_setter_keyword_block)  
> *變數宣告* → [*變數宣告頭(Head)*](..\chapter3\05_Declarations.html#variable_declaration_head) [*變數名*](..\chapter3\05_Declarations.html#variable_name) [*型別注解*](..\chapter3\03_Types.html#type_annotation) [*建構器*](..\chapter3\05_Declarations.html#initializer) _可選_ [*willSet-didSet程式碼區塊*](..\chapter3\05_Declarations.html#willSet_didSet_block)  
> *變數宣告頭(Head)* → [*特性(Attributes)列表*](..\chapter3\06_Attributes.html#attributes) _可選_ [*宣告描述符(Specifiers)列表*](..\chapter3\05_Declarations.html#declaration_specifiers) _可選_ **var**  
> *變數名稱* → [*識別符號*](LexicalStructure.html#identifier)  
> *getter-setter塊* → **{** [*getter子句*](..\chapter3\05_Declarations.html#getter_clause) [*setter子句*](..\chapter3\05_Declarations.html#setter_clause) _可選_ **}**  
> *getter-setter塊* → **{** [*setter子句*](..\chapter3\05_Declarations.html#setter_clause) [*getter子句*](..\chapter3\05_Declarations.html#getter_clause) **}**  
> *getter子句* → [*特性(Attributes)列表*](..\chapter3\06_Attributes.html#attributes) _可選_ **get** [*程式碼區塊*](..\chapter3\05_Declarations.html#code_block)  
> *setter子句* → [*特性(Attributes)列表*](..\chapter3\06_Attributes.html#attributes) _可選_ **set** [*setter名稱*](..\chapter3\05_Declarations.html#setter_name) _可選_ [*程式碼區塊*](..\chapter3\05_Declarations.html#code_block)  
> *setter名稱* → **(** [*識別符號*](LexicalStructure.html#identifier) **)**  
> *getter-setter關鍵字(Keyword)塊* → **{** [*getter關鍵字(Keyword)子句*](..\chapter3\05_Declarations.html#getter_keyword_clause) [*setter關鍵字(Keyword)子句*](..\chapter3\05_Declarations.html#setter_keyword_clause) _可選_ **}**  
> *getter-setter關鍵字(Keyword)塊* → **{** [*setter關鍵字(Keyword)子句*](..\chapter3\05_Declarations.html#setter_keyword_clause) [*getter關鍵字(Keyword)子句*](..\chapter3\05_Declarations.html#getter_keyword_clause) **}**  
> *getter關鍵字(Keyword)子句* → [*特性(Attributes)列表*](..\chapter3\06_Attributes.html#attributes) _可選_ **get**  
> *setter關鍵字(Keyword)子句* → [*特性(Attributes)列表*](..\chapter3\06_Attributes.html#attributes) _可選_ **set**  
> *willSet-didSet程式碼區塊* → **{** [*willSet子句*](..\chapter3\05_Declarations.html#willSet_clause) [*didSet子句*](..\chapter3\05_Declarations.html#didSet_clause) _可選_ **}**  
> *willSet-didSet程式碼區塊* → **{** [*didSet子句*](..\chapter3\05_Declarations.html#didSet_clause) [*willSet子句*](..\chapter3\05_Declarations.html#willSet_clause) **}**  
> *willSet子句* → [*特性(Attributes)列表*](..\chapter3\06_Attributes.html#attributes) _可選_ **willSet** [*setter名稱*](..\chapter3\05_Declarations.html#setter_name) _可選_ [*程式碼區塊*](..\chapter3\05_Declarations.html#code_block)  
> *didSet子句* → [*特性(Attributes)列表*](..\chapter3\06_Attributes.html#attributes) _可選_ **didSet** [*setter名稱*](..\chapter3\05_Declarations.html#setter_name) _可選_ [*程式碼區塊*](..\chapter3\05_Declarations.html#code_block)  

<a name="type_alias_declaration"></a>
##型別的別名宣告

型別別名的宣告可以在你的程式裡為一個已存在的型別宣告一個別名。型別的別名宣告以關鍵字typealias開始，遵循如下的
形式：

> `typealias name` = `existing type`

當一個型別被別名被宣告後，你可以在你程式的任何地方使用別名來代替已存在的型別。已存在的型別可以是已經被命名的
型別或者是混合型別。型別的別名不產生新的型別，它只是簡單的和已存在的型別做名稱替換。

查看更多Protocol Associated Type Declaration.

> 型別別名宣告語法  
> *型別別名宣告* → [*型別別名頭(Head)*](..\chapter3\05_Declarations.html#typealias_head) [*型別別名賦值*](..\chapter3\05_Declarations.html#typealias_assignment)  
> *型別別名頭(Head)* → **typealias** [*型別別名名稱*](..\chapter3\05_Declarations.html#typealias_name)  
> *型別別名名稱* → [*識別符號*](LexicalStructure.html#identifier)  
> *型別別名賦值* → **=** [*型別*](..\chapter3\03_Types.html#type)  

<a name="function_declaration"></a>
##函式宣告

你可以使用函式宣告在你的程式裡引入新的函式。函式可以在類別的上下文，結構，列舉，或者作為方法的協定中被宣告。
函式宣告使用關鍵字func，遵循如下的形式：

> func `function name`(`parameters`) -> `return type` {  
>     `statements`  
> }  

如果函式不回傳任何值，回傳型別可以被忽略，如下所示：

> func `function name`(`parameters`) {  
>     `statements`  
> }  

每個參數的型別都要標明，它們不能被推斷出來。初始時函式的參數是常值。在這些參數前面添加var使它們成為變數，
作用域內任何對變數的改變只在函式體內有效，或者用inout使的這些改變可以在呼叫域內生效。
更多關於in-out參數的討論，參見in-out參數(in-out parameters)

函式可以使用元組型別作為回傳值來回傳多個變數。

函式定義可以出現在另一個函式宣告內。這種函式被稱作nested函式。更多關於nested函式的討論，參見nestde functions。

###參數名

函式的參數是一個以逗號分隔的列表 。函式呼叫是的變數順序必須和函式宣告時的參數順序一致。
最簡單的參數列表有著如下的形式：

> `parameter name`: `parameter type`

對於函式參數來講，參數名在函式體內被使用，而不是在函式呼叫時使用。對於方法參數，參數名在函式體內被使用，
同時也在方法被呼叫時作為標簽被使用。該方法的第一個參數名僅僅在函式體內被使用，就像函式的參數一樣，舉例來講：

```swift
func f(x: Int, y: String) -> String {
    return y + String(x)
}
f(7, "hello")  // x and y have no name
```

```swift
class C {
    func f(x: Int, y: String) -> String {
       return y + String(x)
    }
}
let c = C()
c.f(7, y: "hello")  // x沒有名稱，y有名稱
```

你可以按如下的形式，重寫參數名被使用的過程：

> `external parameter name` `local parameter name`: `parameter type`  
> &#35;`parameter name`: `parameter type`  
> _ `local parameter name`: `parameter type`  

在本地參數前命名的第二名稱(second name)使得參數有一個擴展名。且不同於本地的參數名。
擴展參數名在函式被呼叫時必須被使用。對應的參數在方法或函式被呼叫時必須有擴展名 。

在參數名前所寫的雜湊符號(#)代表著這個參數名可以同時作為外部或本體參數名來使用。等同於書寫兩次本地參數名。
在函式或方法呼叫時，與其對應的語句必須包含這個名字。

本地參數名前的強調字元(_)使參數在函式被呼叫時沒有名稱。在函式或方法呼叫時，與其對應的語句必須沒有名字。

###特殊型別的參數

參數可以被忽略，值可以是變化的，並且提供一個初始值，這種方法有著如下的形式：

> _ : <#parameter type#.  
> `parameter name`: `parameter type`...  
> `parameter name`: `parameter type` = `default argument value`  

以強調符(_)命名的參數明確的在函式體內不能被存取。

一個以基礎型別名的參數，如果緊跟著三個點(...)，被理解為是可變參數。一個函式至多可以擁有一個可變參數，
且必須是最後一個參數。可變參數被作為該基本型別名的陣列來看待。舉例來講，可變參數int...被看做是int[]。
查看可變參數的使用範例，詳見可變參數(variadic parameters)一節。

在參數的型別後面有一個以等號(=)連接的表達式，這樣的參數被看做有著給定表達式的初試值。如果參數在函式
呼叫時被省略了，就會使用初始值。如果參數沒有勝率，那麼它在函式呼叫是必須有自己的名字.舉例來講，
f()和f(x:7)都是只有一個變數x的函式的有效呼叫，但是f(7)是非法的，因為它提供了一個值而不是名稱。

###特殊方法

以self修飾的列舉或結構方法必須以mutating關鍵字作為函式宣告頭。

子類別重寫的方法必須以override關鍵字作為函式宣告頭。不用override關鍵字重寫的方法，使用了override關鍵字
卻並沒有重寫父類別方法都會報錯。

和型別相關而不是和型別實例相關的方法必須在static宣告的結構以或列舉內，亦或是以class關鍵字定義的類別內。

###柯裡化函式和方法

柯裡化函式或方法有著如下的形式：

> func `function name`(`parameters`)(`parameters`) -> `return type` {  
>     `statements`  
> }  

以這種形式定義的函式的回傳值是另一個函式。舉例來說，下面的兩個宣告時等價的:

```swift
func addTwoNumbers(a: Int)(b: Int) -> Int {
    return a + b
}
func addTwoNumbers(a: Int) -> (Int -> Int) {
    func addTheSecondNumber(b: Int) -> Int {
        return a + b
    }
    return addTheSecondNumber
}
```

```swift
addTwoNumbers(4)(5) // Returns 9
```

多級柯裡化應用如下

> 函式宣告語法  
> *函式宣告* → [*函式頭*](..\chapter3\05_Declarations.html#function_head) [*函式名*](..\chapter3\05_Declarations.html#function_name) [*泛型參數子句*](GenericParametersAndArguments.html#generic_parameter_clause) _可選_ [*函式簽名(Signature)*](..\chapter3\05_Declarations.html#function_signature) [*函式體*](..\chapter3\05_Declarations.html#function_body)  
> *函式頭* → [*特性(Attributes)列表*](..\chapter3\06_Attributes.html#attributes) _可選_ [*宣告描述符(Specifiers)列表*](..\chapter3\05_Declarations.html#declaration_specifiers) _可選_ **func**  
> *函式名* → [*識別符號*](LexicalStructure.html#identifier) | [*運算子*](LexicalStructure.html#operator)  
> *函式簽名(Signature)* → [*parameter-clauses*](..\chapter3\05_Declarations.html#parameter_clauses) [*函式結果*](..\chapter3\05_Declarations.html#function_result) _可選_  
> *函式結果* → **->** [*特性(Attributes)列表*](..\chapter3\06_Attributes.html#attributes) _可選_ [*型別*](..\chapter3\03_Types.html#type)  
> *函式體* → [*程式碼區塊*](..\chapter3\05_Declarations.html#code_block)  
> *parameter-clauses* → [*參數子句*](..\chapter3\05_Declarations.html#parameter_clause) [*parameter-clauses*](..\chapter3\05_Declarations.html#parameter_clauses) _可選_  
> *參數子句* → **(** **)** | **(** [*參數列表*](..\chapter3\05_Declarations.html#parameter_list) **...** _可選_ **)**  
> *參數列表* → [*參數*](..\chapter3\05_Declarations.html#parameter) | [*參數*](..\chapter3\05_Declarations.html#parameter) **,** [*參數列表*](..\chapter3\05_Declarations.html#parameter_list)  
> *參數* → **inout** _可選_ **let** _可選_ **#** _可選_ [*參數名*](..\chapter3\05_Declarations.html#parameter_name) [*本地參數名*](..\chapter3\05_Declarations.html#local_parameter_name) _可選_ [*型別注解*](..\chapter3\03_Types.html#type_annotation) [*預設參數子句*](..\chapter3\05_Declarations.html#default_argument_clause) _可選_  
> *參數* → **inout** _可選_ **var** **#** _可選_ [*參數名*](..\chapter3\05_Declarations.html#parameter_name) [*本地參數名*](..\chapter3\05_Declarations.html#local_parameter_name) _可選_ [*型別注解*](..\chapter3\03_Types.html#type_annotation) [*預設參數子句*](..\chapter3\05_Declarations.html#default_argument_clause) _可選_  
> *參數* → [*特性(Attributes)列表*](..\chapter3\06_Attributes.html#attributes) _可選_ [*型別*](..\chapter3\03_Types.html#type)  
> *參數名* → [*識別符號*](LexicalStructure.html#identifier) | **_**  
> *本地參數名* → [*識別符號*](LexicalStructure.html#identifier) | **_**  
> *預設參數子句* → **=** [*表達式*](..\chapter3\04_Expressions.html#expression)  

<a name="enumeration_declaration"></a>
##列舉宣告

在你的程式裡使用列舉宣告來引入一個列舉型別。

列舉宣告有兩種基本的形式，使用關鍵字enum來宣告。列舉宣告體使用從零開始的變數——叫做列舉事件，和任意數量的
宣告，包括計算型屬性，實例方法，靜態方法，建構器，型別別名，甚至其他列舉，結構，和類別。列舉宣告不能
包含析構器或者協定宣告。

不像類別或者結構。列舉型別並不提供隱式的初始建構器，所有建構器必須顯式的宣告。建構器可以委托列舉中的其他
建構器，但是建構過程僅當建構器將一個列舉時間完成後才全部完成。

和結構類似但是和類別不同，列舉是值型別：列舉實例在賦予變數或常數時，或者被函式呼叫時被複製。
更多關於值型別的資訊，參見結構和列舉都是值型別(Structures and Enumerations Are Value Types)一節。

你可以擴展列舉型別，正如在擴展名宣告(Extension Declaration)中討論的一樣。

###任意事件型別的列舉

如下的形式宣告了一個包含任意型別列舉時間的列舉變數

> enum `enumeration name` {  
>     case `enumeration case 1`  
>     case `enumeration case 2`(`associated value types`)  
> }  

這種形式的列舉宣告在其他語言中有時被叫做可識別聯合(discrinminated)。

這種形式中，每一個事件塊由關鍵字case開始，後面緊接著一個或多個以逗號分隔的列舉事件。每一個事件名必須是
獨一無二的。每一個事件也可以指定它所儲存的指定型別的值，這些型別在關聯值型別的元祖裡被指定，立即書寫在事件
名後。獲得更多關於關聯值型別的資訊和範例，請查看關聯值(associated values)一節。

###使用原始事件值的列舉

以下的形式宣告了一個包含相同基礎型別的列舉事件的列舉：

> enum `enumeration name`: `raw value type` {  
>     case `enumeration case 1` = `raw value 1`  
>     case `enumeration case 2` = `raw value 2`  
> }  

在這種形式中，每一個事件塊由case關鍵字開始，後面緊接著一個或多個以逗號分隔的列舉事件。和第一種形式的列舉
事件不同，這種形式的列舉事件包含一個同型別的基礎值，叫做原始值(raw value)。這些值的型別在原始值型別(raw value type)
中被指定，必須是字面上的整數，浮點數，字元或者字串。

每一個事件必須有唯一的名字，必須有一個唯一的初始值。如果初始值型別被指定為int，則不必為事件顯式的指定值，
它們會隱式的被標為值0,1,2等。每一個沒有被賦值的Int型別時間會隱式的賦予一個初始值，它們是自動遞增的。

```swift
num ExampleEnum: Int {
    case A, B, C = 5, D
}
```

在上面的範例中，ExampleEnum.A的值是0，ExampleEnum.B的值是。因為ExampleEnum.C的值被顯式的設定為5，因此
ExampleEnum.D的值會自動增長為6.

列舉事件的初始值可以呼叫方法roRaw獲得，如ExampleEnum.B.toRaw()。你也可以通過呼叫fromRaw方法來使用初始值找到
其對應的事件，並回傳一個可選的事件。查看更多資訊和獲取初始值型別事件的資訊，參閱初始值(raw values)。

###獲得列舉事件

使用點(.)來參考列舉型別的事件，如 EnumerationType.EnumerationCase。當列舉型別可以上下文推斷出時，你可以
省略它(.仍然需要)，參照列舉語法(Enumeration Syntax)和顯式成員表達(Implicit Member Expression).

使用switch語句來檢驗列舉事件的值，正如使用switch語句匹配列舉值（Matching Enumeration Values with a Switch Statement)一節描述的那樣。

列舉型別是模式匹配(pattern-matched)的，和其相反的是switch語句case塊中列舉事件匹配，在列舉事件型別(Enumeration Case Pattern)中有描述。

> 列舉宣告語法  
> *列舉宣告* → [*特性(Attributes)列表*](..\chapter3\06_Attributes.html#attributes) _可選_ [*聯合式列舉*](..\chapter3\05_Declarations.html#union_style_enum) | [*特性(Attributes)列表*](..\chapter3\06_Attributes.html#attributes) _可選_ [*原始值式列舉*](..\chapter3\05_Declarations.html#raw_value_style_enum)  
> *聯合式列舉* → [*列舉名*](..\chapter3\05_Declarations.html#enum_name) [*泛型參數子句*](GenericParametersAndArguments.html#generic_parameter_clause) _可選_ **{** [*union-style-enum-members*](..\chapter3\05_Declarations.html#union_style_enum_members) _可選_ **}**  
> *union-style-enum-members* → [*union-style-enum-member*](..\chapter3\05_Declarations.html#union_style_enum_member) [*union-style-enum-members*](..\chapter3\05_Declarations.html#union_style_enum_members) _可選_  
> *union-style-enum-member* → [*宣告*](..\chapter3\05_Declarations.html#declaration) | [*聯合式(Union Style)的列舉case子句*](..\chapter3\05_Declarations.html#union_style_enum_case_clause)  
> *聯合式(Union Style)的列舉case子句* → [*特性(Attributes)列表*](..\chapter3\06_Attributes.html#attributes) _可選_ **case** [*聯合式(Union Style)的列舉case列表*](..\chapter3\05_Declarations.html#union_style_enum_case_list)  
> *聯合式(Union Style)的列舉case列表* → [*聯合式(Union Style)的case*](..\chapter3\05_Declarations.html#union_style_enum_case) | [*聯合式(Union Style)的case*](..\chapter3\05_Declarations.html#union_style_enum_case) **,** [*聯合式(Union Style)的列舉case列表*](..\chapter3\05_Declarations.html#union_style_enum_case_list)  
> *聯合式(Union Style)的case* → [*列舉的case名*](..\chapter3\05_Declarations.html#enum_case_name) [*元組型別*](..\chapter3\03_Types.html#tuple_type) _可選_  
> *列舉名* → [*識別符號*](LexicalStructure.html#identifier)  
> *列舉的case名* → [*識別符號*](LexicalStructure.html#identifier)  
> *原始值式列舉* → [*列舉名*](..\chapter3\05_Declarations.html#enum_name) [*泛型參數子句*](GenericParametersAndArguments.html#generic_parameter_clause) _可選_ **:** [*型別標識*](..\chapter3\03_Types.html#type_identifier) **{** [*原始值式列舉成員列表*](..\chapter3\05_Declarations.html#raw_value_style_enum_members) _可選_ **}**  
> *原始值式列舉成員列表* → [*原始值式列舉成員*](..\chapter3\05_Declarations.html#raw_value_style_enum_member) [*原始值式列舉成員列表*](..\chapter3\05_Declarations.html#raw_value_style_enum_members) _可選_  
> *原始值式列舉成員* → [*宣告*](..\chapter3\05_Declarations.html#declaration) | [*原始值式列舉case子句*](..\chapter3\05_Declarations.html#raw_value_style_enum_case_clause)  
> *原始值式列舉case子句* → [*特性(Attributes)列表*](..\chapter3\06_Attributes.html#attributes) _可選_ **case** [*原始值式列舉case列表*](..\chapter3\05_Declarations.html#raw_value_style_enum_case_list)  
> *原始值式列舉case列表* → [*原始值式列舉case*](..\chapter3\05_Declarations.html#raw_value_style_enum_case) | [*原始值式列舉case*](..\chapter3\05_Declarations.html#raw_value_style_enum_case) **,** [*原始值式列舉case列表*](..\chapter3\05_Declarations.html#raw_value_style_enum_case_list)  
> *原始值式列舉case* → [*列舉的case名*](..\chapter3\05_Declarations.html#enum_case_name) [*原始值賦值*](..\chapter3\05_Declarations.html#raw_value_assignment) _可選_  
> *原始值賦值* → **=** [*字面量*](LexicalStructure.html#literal)  

<a name="structure_declaration"></a>
##結構宣告

使用結構宣告可以在你的程式裡引入一個結構型別。結構宣告使用struct關鍵字，遵循如下的形式：

> struct `structure name`: `adopted protocols` {  
>     `declarations`  
> }  

結構內包含零或多個宣告。這些宣告可以包括儲存型和計算型屬性，靜態屬性，實例方法，靜態方法，建構器，
型別別名，甚至其他結構，類別，和列舉宣告。結構宣告不能包含析構器或者協定宣告。詳細討論和包含多種結構
宣告的實例，參見類別和結構一節。

結構可以包含任意數量的協定，但是不能繼承自類別，列舉或者其他結構。

有三種方法可以創建一個宣告過的結構實例：

-呼叫結構內宣告的建構器，參照建構器(initializers)一節。

—如果沒有宣告建構器，呼叫結構的逐個建構器，詳情參見Memberwise Initializers for Structure Types.

—如果沒有宣告析構器，結構的所有屬性都有初始值，呼叫結構的預設建構器，詳情參見預設建構器(Default Initializers).

結構的建構過程參見初始化(initiaization)一節。

結構實例屬性可以用點(.)來獲得，詳情參見獲得屬性(Accessing Properties)一節。

結構是值型別；結構的實例在被賦予變數或常數，被函式呼叫時被複製。獲得關於值型別更多資訊，參見
結構和列舉都是值型別(Structures and Enumerations Are Value Types)一節。

你可以使用擴展宣告來擴展結構型別的行為，參見擴展宣告(Extension Declaration).

> 結構宣告語法  
> *結構宣告* → [*特性(Attributes)列表*](..\chapter3\06_Attributes.html#attributes) _可選_ **struct** [*結構名稱*](..\chapter3\05_Declarations.html#struct_name) [*泛型參數子句*](GenericParametersAndArguments.html#generic_parameter_clause) _可選_ [*型別繼承子句*](..\chapter3\03_Types.html#type_inheritance_clause) _可選_ [*結構主體*](..\chapter3\05_Declarations.html#struct_body)  
> *結構名稱* → [*識別符號*](LexicalStructure.html#identifier)  
> *結構主體* → **{** [*宣告(Declarations)列表*](..\chapter3\05_Declarations.html#declarations) _可選_ **}**  

<a name="class_declaration"></a>
##類別宣告

你可以在你的程式中使用類別宣告來引入一個類別。類別宣告使用關鍵字class，遵循如下的形式：

> class `class name`: `superclass`, `adopted protocols` {  
>     `declarations`  
> }  

一個類別內包含零或多個宣告。這些宣告可以包括儲存型和計算型屬性，實例方法，類別方法，建構器，單獨的析構器方法，
型別別名，甚至其他結構，類別，和列舉宣告。類別宣告不能包含協定宣告。詳細討論和包含多種類別宣告的實例，參見類別和
結構一節。

一個類別只能繼承一個父類別，超類別，但是可以包含任意數量的協定。這些超類別第一次在type-inheritance-clause出現，遵循任意協定。

正如在初始化宣告(Initializer Declaration)談及的那樣，類別可以有指定和方便的建構器。當你宣告任一中建構器時，
你可以使用requierd變數來標記建構器，要求任意子類別來重寫它。指定類別的建構器必須初始化類別所有的已宣告的屬性，
它必須在子類別建構器呼叫前被執行。

類別可以重寫屬性，方法和它的超類別的建構器。重寫的方法和屬性必須以override標注。

雖然超類別的屬性和方法宣告可以被當前類別繼承，但是超類別宣告的指定建構器卻不能。這意味著，如果當前類別重寫了超類別
的所有指定建構器，它就繼承了超類別的方便建構器。Swift的類別並不是繼承自一個全域基礎類別。

有兩種方法來創建已宣告的類別的實例：

- 呼叫類別的一個建構器，參見建構器(initializers)。
- 如果沒有宣告建構器，而且類別的所有屬性都被賦予了初始值，呼叫類別的預設建構器，參見預設建構器(default initializers).

類別實例屬性可以用點(.)來獲得，詳情參見獲得屬性(Accessing Properties)一節。

類別是參考型別；當被賦予常數或變數，函式呼叫時，類別的實例是被參考，而不是複製。獲得更多關於參考型別的資訊，
結構和列舉都是值型別(Structures and Enumerations Are Value Types)一節。

你可以使用擴展宣告來擴展類別的行為，參見擴展宣告(Extension Declaration).

> 類別宣告語法  
> *類別宣告* → [*特性(Attributes)列表*](..\chapter3\06_Attributes.html#attributes) _可選_ **class** [*類別名*](..\chapter3\05_Declarations.html#class_name) [*泛型參數子句*](GenericParametersAndArguments.html#generic_parameter_clause) _可選_ [*型別繼承子句*](..\chapter3\03_Types.html#type_inheritance_clause) _可選_ [*類別主體*](..\chapter3\05_Declarations.html#class_body)  
> *類別名* → [*識別符號*](LexicalStructure.html#identifier)  
> *類別主體* → **{** [*宣告(Declarations)列表*](..\chapter3\05_Declarations.html#declarations) _可選_ **}**  

<a name="protocol_declaration"></a>
##協定宣告(translated by 小一)

一個協定宣告為你的程式引入一個命名了的協定型別。協定宣告使用 `protocol` 關鍵詞來進行宣告並有下面這樣的形式：

> protocol `protocol name`: `inherited protocols` {  
>     `protocol member declarations`  
> }  

協定的主體包含零或多個協定成員宣告，這些成員描述了任何采用該協定必須滿足的一致性要求。特別的，一個協定可以宣告必須實作某些屬性、方法、初始化程式及下標腳本的一致性型別。協定也可以宣告專用種類別的型別別名，叫做關聯型別，它可以指定協定的不同宣告之間的關系。協定成員宣告會在下面的詳情裡進行討論。

協定型別可以從很多其它協定那繼承。當一個協定型別從其它協定那繼承的時候，來自其它協定的所有要求就集合了，而且從當前協定繼承的任何型別必須符合所有的這些要求。對於如何使用協定繼承的範例，查看[協定繼承](../chapter2/21_Protocols.html#protocol_inheritance)

> 注意：  
你也可以使用協定合成型別集合多個協定的一致性要求，詳情參見[協定合成型別](../chapter3/03_Types.html#protocol_composition_type)和[協定合成](../chapter2/21_Protocols.html#protocol_composition)

你可以通過采用在型別的擴展宣告中的協定來為之前宣告的型別添加協定一致性。在擴展中你必須實作所有采用協定的要求。如果該型別已經實作了所有的要求，你可以讓這個擴展宣告的主題留空。

預設地，符合某一個協定的型別必須實作所有宣告在協定中的屬性、方法和下標腳本。也就是說，你可以用`optional`屬性標注這些協定成員宣告以指定它們的一致性型別實作是可選的。`optional`屬性僅僅可以用於使用`objc`屬性標記過的協定。這樣的結果就是僅僅類型別可以采用並符合包含可選成員要求的協定。更多關於如何使用`optional`屬性的資訊及如何存取可選協定成員的指導——比如當你不能肯定是否一致性的型別實作了它們——參見[可選協定要求](../chapter2/21_Protocols.html#optional_protocol_requirements)

為了限制協定的采用僅僅針對類型別，需要使用`class_protocol`屬性標記整個協定宣告。任意繼承自標記有`class_protocol`屬性協定的協定都可以智能地僅能被類型別采用。

>注意：  
如果協定已經用`object`屬性標記了，`class_protocol`屬性就隱性地應用於該協定；沒有必要再明確地使用`class_protocol`屬性來標記該協定了。

協定是命名的型別，因此它們可以以另一個命名型別出現在你程式碼的所有地方，就像[協定型別](../chapter2/21_Protocols.html#protocols_as_types)裡討論的那樣。然而你不能建構一個協定的實例，因為協定實際上不提供它們指定的要求的實作。

你可以使用協定來宣告一個類別的代理的方法或者應該實作的結構，就像[委托(代理)模式](../chapter2/21_Protocols.html#delegation)描述的那樣。

> 協定(Protocol)宣告語法  
> *協定宣告* → [*特性(Attributes)列表*](..\chapter3\06_Attributes.html#attributes) _可選_ **protocol** [*協定名*](..\chapter3\05_Declarations.html#protocol_name) [*型別繼承子句*](..\chapter3\03_Types.html#type_inheritance_clause) _可選_ [*協定主體*](..\chapter3\05_Declarations.html#protocol_body)  
> *協定名* → [*識別符號*](LexicalStructure.html#identifier)  
> *協定主體* → **{** [*協定成員宣告(Declarations)列表*](..\chapter3\05_Declarations.html#protocol_member_declarations) _可選_ **}**  
> *協定成員宣告* → [*協定屬性宣告*](..\chapter3\05_Declarations.html#protocol_property_declaration)  
> *協定成員宣告* → [*協定方法宣告*](..\chapter3\05_Declarations.html#protocol_method_declaration)  
> *協定成員宣告* → [*協定建構器宣告*](..\chapter3\05_Declarations.html#protocol_initializer_declaration)  
> *協定成員宣告* → [*協定附屬腳本宣告*](..\chapter3\05_Declarations.html#protocol_subscript_declaration)  
> *協定成員宣告* → [*協定關聯型別宣告*](..\chapter3\05_Declarations.html#protocol_associated_type_declaration)  
> *協定成員宣告(Declarations)列表* → [*協定成員宣告*](..\chapter3\05_Declarations.html#protocol_member_declaration) [*協定成員宣告(Declarations)列表*](..\chapter3\05_Declarations.html#protocol_member_declarations) _可選_  

<a name="protocol_property_declaration"></a>
###協定屬性宣告

協定宣告了一致性型別必須在協定宣告的主體裡通過引入一個協定屬性宣告來實作一個屬性。協定屬性宣告有一種特殊的型別宣告形式：

> var `property name`: `type` { get set }

同其它協定成員宣告一樣，這些屬性宣告僅僅針對符合該協定的型別宣告了`getter`和`setter`要求。結果就是你不需要在協定裡它被宣告的地方實作`getter`和`setter`。

`getter`和`setter`要求可以通過一致性型別以各種方式滿足。如果屬性宣告包含`get`和`set`關鍵詞，一致性型別就可以用可讀寫（實作了`getter`和`setter`）的儲存型變數屬性或計算型屬性，但是屬性不能以常數屬性或唯讀計算型屬性實作。如果屬性宣告僅僅包含`get`關鍵詞的話，它可以作為任意型別的屬性被實作。比如說實作了協定的屬性要求的一致性型別，參見[屬性要求](../chapter2/21_Protocols.html#property_requirements)

更多參見[變數宣告](../chapter3/05_Declarations.html#variable_declaration)

> 協定屬性宣告語法  
> *協定屬性宣告* → [*變數宣告頭(Head)*](..\chapter3\05_Declarations.html#variable_declaration_head) [*變數名*](..\chapter3\05_Declarations.html#variable_name) [*型別注解*](..\chapter3\03_Types.html#type_annotation) [*getter-setter關鍵字(Keyword)塊*](..\chapter3\05_Declarations.html#getter_setter_keyword_block)  

###協定方法宣告

協定宣告了一致性型別必須在協定宣告的主體裡通過引入一個協定方法宣告來實作一個方法.
協定方法宣告和函式方法宣告有著相同的形式，包含如下兩條規則：它們不包括函式體，你不能在類別的宣告內為它們的
參數提供初始值.舉例來說，符合的型別執行協定必需的方法。參見必需方法一節。

使用關鍵字class可以在協定宣告中宣告一個類別或必需的靜態方法。執行這些方法的類別也用關鍵字class宣告。
相反的，執行這些方法的結構必須以關鍵字static宣告。如果你想使用擴展方法，在擴展類別時使用class關鍵字，
在擴展結構時使用static關鍵字。

更多請參閱函式宣告。

> 協定方法宣告語法  
> *協定方法宣告* → [*函式頭*](..\chapter3\05_Declarations.html#function_head) [*函式名*](..\chapter3\05_Declarations.html#function_name) [*泛型參數子句*](GenericParametersAndArguments.html#generic_parameter_clause) _可選_ [*函式簽名(Signature)*](..\chapter3\05_Declarations.html#function_signature)  

###協定建構器宣告

協定宣告了一致性型別必須在協定宣告的主體裡通過引入一個協定建構器宣告來實作一個建構器。協定建構器宣告
除了不包含建構器體外，和建構器宣告有著相同的形式，

更多請參閱建構器宣告。

> 協定建構器宣告語法  
> *協定建構器宣告* → [*建構器頭(Head)*](..\chapter3\05_Declarations.html#initializer_head) [*泛型參數子句*](GenericParametersAndArguments.html#generic_parameter_clause) _可選_ [*參數子句*](..\chapter3\05_Declarations.html#parameter_clause)  

###協定下標腳本宣告

協定宣告了一致性型別必須在協定宣告的主體裡通過引入一個協定下標腳本宣告來實作一個下標腳本。協定屬性宣告
對下標腳本宣告有一個特殊的形式：

> subscript (`parameters`) -> `return type` { get set }

下標腳本宣告只為和協定一致的型別宣告了必需的最小數量的的getter和setter。如果下標腳本申明包含get和set關鍵字，
一致的型別也必須有一個getter和setter語句。如果下標腳本宣告值包含get關鍵字，一致的型別必須至少包含一個
getter語句，可以選擇是否包含setter語句。

更多參閱下標腳本宣告。

> 協定附屬腳本宣告語法  
> *協定附屬腳本宣告* → [*附屬腳本頭(Head)*](..\chapter3\05_Declarations.html#subscript_head) [*附屬腳本結果(Result)*](..\chapter3\05_Declarations.html#subscript_result) [*getter-setter關鍵字(Keyword)塊*](..\chapter3\05_Declarations.html#getter_setter_keyword_block)  

###協定相關型別宣告

協定宣告相關型別使用關鍵字typealias。相關型別為作為協定宣告的一部分的型別提供了一個別名。相關型別和參數
語句中的型別參數很相似，但是它們在宣告的協定中包含self關鍵字。在這些語句中，self指代和協定一致的可能的型別。
獲得更多資訊和範例，查看相關型別或型別別名宣告。

> 協定關聯型別宣告語法  
> *協定關聯型別宣告* → [*型別別名頭(Head)*](..\chapter3\05_Declarations.html#typealias_head) [*型別繼承子句*](..\chapter3\03_Types.html#type_inheritance_clause) _可選_ [*型別別名賦值*](..\chapter3\05_Declarations.html#typealias_assignment) _可選_  

<a name="initializer_declaration"></a>
##建構器宣告

建構器宣告會為程式內的類別，結構或列舉引入建構器。建構器使用關鍵字Init來宣告，遵循兩條基本形式。

結構，列舉，類別可以有任意數量的建構器，但是類別的建構器的規則和行為是不一樣的。不像結構和列舉那樣，類別
有兩種結構，designed initializers 和convenience initializers，參見建構器一節。

如下的形式宣告了結構，列舉和類別的指定建構器：

> init(`parameters`) {  
>     `statements`  
> }  

類別的指定建構器將類別的所有屬性直接初始化。如果類別有超類別，它不能呼叫該類別的其他建構器,它只能呼叫超類別的一個
指定建構器。如果該類別從它的超類別處繼承了任何屬性，這些屬性在當前類別內被賦值或修飾時，必須帶哦用一個超類別的
指定建構器。

指定建構器可以在類別宣告的上下文中宣告，因此它不能用擴展宣告的方法加入一個類別中。

結構和列舉的建構器可以帶哦用其他的已宣告的建構器，來委托其中一個火全部進行初始化過程。

以關鍵字convenience來宣告一個類別的便利建構器：

> convenience init(`parameters`) {  
>    `statements`  
> }  

便利建構器可以將初始化過程委托給另一個便利建構器或類別的一個指定建構器。這意味著，類別的初始化過程必須
以一個將所有類別屬性完全初始化的指定建構器的呼叫作為結束。便利建構器不能呼叫超類別的建構器。

你可以使用requierd關鍵字，將便利建構器和指定建構器標記為每個子類別的建構器都必須擁有的。因為指定建構器
不被子類別繼承，它們必須被立即執行。當子類別直接執行所有超類別的指定建構器(或使用便利建構器重寫指定建構器)時，
必需的便利建構器可以被隱式的執行，亦可以被繼承。不像方法，下標腳本那樣，你不需要為這些重寫的建構器標注
overrride關鍵字。

查看更多關於不同宣告方法的建構器的範例，參閱建構過程一節。

> 建構器宣告語法  
> *建構器宣告* → [*建構器頭(Head)*](..\chapter3\05_Declarations.html#initializer_head) [*泛型參數子句*](GenericParametersAndArguments.html#generic_parameter_clause) _可選_ [*參數子句*](..\chapter3\05_Declarations.html#parameter_clause) [*建構器主體*](..\chapter3\05_Declarations.html#initializer_body)  
> *建構器頭(Head)* → [*特性(Attributes)列表*](..\chapter3\06_Attributes.html#attributes) _可選_ **convenience** _可選_ **init**  
> *建構器主體* → [*程式碼區塊*](..\chapter3\05_Declarations.html#code_block)  

<a name="deinitializer_declaration"></a>
##析構宣告

析構宣告為類別宣告了一個析構器。析構器沒有參數，遵循如下的格式：

> deinit {  
>    `statements`  
> }  

當類別沒有任何語句時將要被釋放時，析構器會自動的被呼叫。析構器在類別的宣告體內只能被宣告一次——但是不能在
類別的擴展宣告內，每個類別最多只能有一個。

子類別繼承了它的超類別的析構器，在子類別將要被釋放時隱式的呼叫。子類別在所有析構器被執行完畢前不會被釋放。

析構器不會被直接呼叫。

查看範例和如何在類別的宣告中使用析構器，參見析構過程一節。

> 析構器宣告語法  
> *析構器宣告* → [*特性(Attributes)列表*](..\chapter3\06_Attributes.html#attributes) _可選_ **deinit** [*程式碼區塊*](..\chapter3\05_Declarations.html#code_block)  

<a name="extension_declaration"></a>
##擴展宣告

擴展宣告用於擴展一個現存的類別，結構，列舉的行為。擴展宣告以關鍵字extension開始，遵循如下的規則：

> extension `type`: `adopted protocols` {  
>    `declarations`  
> }  

一個擴展宣告體包括零個或多個宣告。這些宣告可以包括計算型屬性，計算型靜態屬性，實例方法，靜態和類別方法，建構器，
下標腳本宣告，甚至其他結構，類別，和列舉宣告。擴展宣告不能包含析構器，協定宣告，儲存型屬性，屬性監測器或其他
的擴展屬性。詳細討論和查看包含多種擴展宣告的實例，參見擴展一節。

擴展宣告可以向現存的類別，結構，列舉內添加一致的協定。擴展宣告不能向一個類別中添加繼承的類別，因此
type-inheritance-clause是一個只包含協定列表的擴展宣告。

屬性，方法，現存型別的建構器不能被它們型別的擴展所重寫。

擴展宣告可以包含建構器宣告，這意味著，如果你擴展的型別在其他模塊中定義，建構器宣告必須委托另一個在
那個模塊裡宣告的建構器來恰當的初始化。

> 擴展(Extension)宣告語法  
> *擴展宣告* → **extension** [*型別標識*](..\chapter3\03_Types.html#type_identifier) [*型別繼承子句*](..\chapter3\03_Types.html#type_inheritance_clause) _可選_ [*extension-body*](..\chapter3\05_Declarations.html#extension_body)  
> *extension-body* → **{** [*宣告(Declarations)列表*](..\chapter3\05_Declarations.html#declarations) _可選_ **}**  

<a name="subscript_declaration"></a>
##下標腳本宣告(translated by 林)

附屬腳本用於向特定型別添加附屬腳本支援，通常為存取集合，列表和序列的元素時提供語法便利。附屬腳本宣告使用關鍵字`subscript`，宣告形式如下：

> subscript (`parameter`) -> (return type){  
>     get{    
>       `statements`    
>     }    
>     set(`setter name`){    
>       `statements`    
>     }    
> }    

附屬腳本宣告只能在類別，結構，列舉，擴展和協定宣告的上下文進行宣告。

_變數(parameters)_指定一個或多個用於在相關型別的下標腳本中存取元素的索引（例如，表達式`object[i]`中的`i`）。儘管用於元素存取的索引可以是任意型別的，但是每個變數必須包含一個用於指定每種索引型別的型別標注。_回傳型別(return type)_指定被存取的元素的型別。

和計算性屬性一樣，下標腳本宣告支援對存取元素的讀寫操作。getter用於讀取值，setter用於寫入值。setter子句是可選的，當僅需要一個getter子句時，可以將二者都忽略且直接回傳請求的值即可。也就是說，如果使用了setter子句，就必須使用getter子句。

setter的名字和封閉的括號是可選的。如果使用了setter名稱，它會被當做傳給setter的變數的名稱。如果不使用setter名稱，那麼傳給setter的變數的名稱預設是`value`。setter名稱的型別必須與_回傳型別(return type)_的型別相同。

可以在下標腳本宣告的型別中，可以重載下標腳本，只要_變數(parameters)_或_回傳型別(return type)_與先前的不同即可。此時，必須使用`override`關鍵字宣告那個被覆蓋的下標腳本。(注：好亂啊！到底是重載還是覆蓋？！)

同樣可以在協定宣告的上下文中宣告下標腳本，[Protocol Subscript Declaration](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Declarations.html#//apple_ref/doc/uid/TP40014097-CH34-XID_619)中有所描述。

更多關於下標腳本和下標腳本宣告的範例，請參考[Subscripts](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Subscripts.html#//apple_ref/doc/uid/TP40014097-CH16-XID_393)。

> 附屬腳本宣告語法  
> *附屬腳本宣告* → [*附屬腳本頭(Head)*](..\chapter3\05_Declarations.html#subscript_head) [*附屬腳本結果(Result)*](..\chapter3\05_Declarations.html#subscript_result) [*程式碼區塊*](..\chapter3\05_Declarations.html#code_block)  
> *附屬腳本宣告* → [*附屬腳本頭(Head)*](..\chapter3\05_Declarations.html#subscript_head) [*附屬腳本結果(Result)*](..\chapter3\05_Declarations.html#subscript_result) [*getter-setter塊*](..\chapter3\05_Declarations.html#getter_setter_block)  
> *附屬腳本宣告* → [*附屬腳本頭(Head)*](..\chapter3\05_Declarations.html#subscript_head) [*附屬腳本結果(Result)*](..\chapter3\05_Declarations.html#subscript_result) [*getter-setter關鍵字(Keyword)塊*](..\chapter3\05_Declarations.html#getter_setter_keyword_block)  
> *附屬腳本頭(Head)* → [*特性(Attributes)列表*](..\chapter3\06_Attributes.html#attributes) _可選_ **subscript** [*參數子句*](..\chapter3\05_Declarations.html#parameter_clause)  
> *附屬腳本結果(Result)* → **->** [*特性(Attributes)列表*](..\chapter3\06_Attributes.html#attributes) _可選_ [*型別*](..\chapter3\03_Types.html#type)   

<a name="operator_declaration"></a>
##運算子宣告(translated by 林)

運算子宣告會向程式中引入中綴、前綴或後綴運算子，它使用上下文關鍵字`operator`宣告。
可以宣告三種不同的綴性：中綴、前綴和後綴。運算子的綴性描述了運算子與它的運算元的相對位置。
運算子宣告有三種基本形式，每種綴性各一種。運算子的綴性通過在`operator`和運算子之間添加上下文關鍵字`infix`，`prefix`或`postfix`來指定。每種形式中，運算子的名字只能包含[Operators](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/LexicalStructure.html#//apple_ref/doc/uid/TP40014097-CH30-XID_871)中定義的運算子字元。

下面的這種形式宣告了一個新的中綴運算子：
> operator infix `operator name`{  
>     previewprecedence `precedence level`  
>     associativity `associativity`  
> }  

_中綴_運算子是二元運算子，它可以被置於兩個運算元之間，比如表達式`1 + 2` 中的加法運算子(`+`)。

中綴運算子可以可選地指定優先級，結合性，或兩者同時指定。

運算子的_優先級_可以指定在沒有括號包圍的情況下，運算子與它的運算元如何緊密綁定的。可以使用上下文關鍵字`precedence`並_優先級(precedence level)_一起來指定一個運算子的優先級。_優先級_可以是0到255之間的任何一個數字(十進制整數)；與十進制整數字面量不同的是，它不可以包含任何底線字元。儘管優先級是一個特定的數字，但它僅用作與另一個運算子比較(大小)。也就是說，一個運算元可以同時被兩個運算子使用時，例如`2 + 3 * 5`，優先級更高的運算子將優先與運算元綁定。

運算子的_結合性_可以指定在沒有括號包圍的情況下，優先級相同的運算子以何種順序被分組的。可以使用上下文關鍵字`associativity`並_結合性(associativity)_一起來指定一個運算子的結合性，其中_結合性_可以說是上下文關鍵字`left`，`right`或`none`中的任何一個。左結合運算子以從左到右的形式分組。例如，減法運算子(`-`)具有左結合性，因此`4 - 5 - 6`被以`(4 - 5) - 6`的形式分組，其結果為`-7`。
右結合運算子以從右到左的形式分組，對於設置為`none`的非結合運算子，它們不以任何形式分組。具有相同優先級的非結合運算子，不可以互相鄰接。例如，表達式`1 < 2 < 3`非法的。

宣告時不指定任何優先級或結合性的中綴運算子，它們的優先級會被初始化為100，結合性被初始化為`none`。

下面的這種形式宣告了一個新的前綴運算子：
> operator prefix `operator name`{}  

緊跟在運算元前邊的_前綴運算子(prefix operator)_是一元運算子，例如表達式`++i`中的前綴遞增運算子(`++`)。

前綴運算子的宣告中不指定優先級。前綴運算子是非結合的。

下面的這種形式宣告了一個新的後綴運算子：

> operator postfix `operator name`{}  

緊跟在運算元後邊的_後綴運算子(postfix operator)_是一元運算子，例如表達式`i++`中的前綴遞增運算子(`++`)。

和前綴運算子一樣，後綴運算子的宣告中不指定優先級。後綴運算子是非結合的。

宣告了一個新的運算子以後，需要宣告一個跟這個運算子同名的函式來實作這個運算子。如何實作一個新的運算子，請參考[Custom Operators](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/AdvancedOperators.html#//apple_ref/doc/uid/TP40014097-CH27-XID_48)。

> 運算子宣告語法  
> *運算子宣告* → [*前綴運算子宣告*](..\chapter3\05_Declarations.html#prefix_operator_declaration) | [*後綴運算子宣告*](..\chapter3\05_Declarations.html#postfix_operator_declaration) | [*中綴運算子宣告*](..\chapter3\05_Declarations.html#infix_operator_declaration)  
> *前綴運算子宣告* → **運算子** **prefix** [*運算子*](LexicalStructure.html#operator) **{** **}**  
> *後綴運算子宣告* → **運算子** **postfix** [*運算子*](LexicalStructure.html#operator) **{** **}**  
> *中綴運算子宣告* → **運算子** **infix** [*運算子*](LexicalStructure.html#operator) **{** [*中綴運算子屬性*](..\chapter3\05_Declarations.html#infix_operator_attributes) _可選_ **}**  
> *中綴運算子屬性* → [*優先級子句*](..\chapter3\05_Declarations.html#precedence_clause) _可選_ [*結和性子句*](..\chapter3\05_Declarations.html#associativity_clause) _可選_  
> *優先級子句* → **precedence** [*優先級水平*](..\chapter3\05_Declarations.html#precedence_level)  
> *優先級水平* → 數值 0 到 255  
> *結和性子句* → **associativity** [*結和性*](..\chapter3\05_Declarations.html#associativity)  
> *結和性* → **left** | **right** | **none**  
