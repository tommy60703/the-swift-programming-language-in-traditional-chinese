> 翻譯：[superkam](https://github.com/superkam)
> 校對：[numbbbbb](https://github.com/numbbbbb)

# 詞法結構
-----------------

本頁包含內容：

- [空白與註解（*Whitespace and Comments*）](#whitespace_and_comments)
- [識別符號（*Identifiers*）](#identifiers)
- [關鍵字（*Keywords*）](#keywords)
- [字面量（*Literals*）](#literals)
- [運算子（*Operators*）](#operators)

Swift 的“詞法結構（*lexical structure*）”描述了如何在該語言中用字元序列構建合法標記，組成該語言中最底層的程式碼區塊，並在之後的章節中用於描述語言的其他部分。

通常，標記在隨後介紹的語法約束下，由 Swift 原始碼的輸入文字中提取可能的最長子字串生成。這種方法稱為“最長匹配項（*longest match*）”，或者“最大適合”（*maximal munch*）。

<a name="whitespace_and_comments"></a>
## 空白與註解

空白（*whitespace*）有兩個用途：分隔原始碼中的標記和區分運算子屬於前綴還是後綴，（參見 [運算子](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/LexicalStructure.html#//apple_ref/doc/uid/TP40014097-CH30-XID_871)）在其他情況下則會被忽略。以下的字元會被當作空白：空格（*space*）（U+0020）、換行（*line feed*）（U+000A）、回車（*carriage return*）（U+000D）、水平 tab（*horizontal tab*）（U+0009）、垂直 tab（*vertical tab*）（U+000B）、換頁符號（*form feed*）（U+000C）以及空（*null*）（U+0000）。

註解（*comments*）被編譯器當作空白處理。單行註解由 `//` 開始直到該行結束。多行註解由 `/*` 開始，以 `*/` 結束。可以嵌套註解，但注意註解標記必須匹配。

<a name="identifiers"></a>
## 識別符號

識別符號（*identifiers*）可以由以下的字元開始：大寫或小寫的字母 `A` 到 `Z`、底線 `_`、基本多語言面（*Basic Multilingual Plane*）中的 Unicode 非組合字元以及基本多語言面以外的非專用區（*Private Use Area*）字元。首字元之後，識別符號允許使用數字和 Unicode 字元組合。

使用保留字（*reserved word*）作為識別符號，需要在其前後增加反引號 <code>\`</code>。例如，<code>class</code> 不是合法的識別符號，但可以使用 <code>\`class\`</code>。反引號不屬於識別符號的一部分，<code>\`x\`</code> 和 `x` 表示同一識別符號。

閉包（*closure*）中如果沒有明確指定參數名稱，參數將被隱式命名為 <code>$0</code>、<code>$1</code>、<code>$2</code>... 這些命名在閉包作用域內是合法的識別符號。

> 識別符號語法  
> *識別符號* → [*識別符號頭(Head)*](LexicalStructure.html#identifier_head) [*識別符號字元列表*](LexicalStructure.html#identifier_characters) _可選_  
> *識別符號* → **`** [*識別符號頭(Head)*](LexicalStructure.html#identifier_head) [*識別符號字元列表*](LexicalStructure.html#identifier_characters) _可選_ **`**  
> *識別符號* → [*隱式參數名*](LexicalStructure.html#implicit_parameter_name)  
> *識別符號列表* → [*識別符號*](LexicalStructure.html#identifier) | [*識別符號*](LexicalStructure.html#identifier) **,** [*識別符號列表*](LexicalStructure.html#identifier_list)  
> *識別符號頭(Head)* → Upper- or lowercase letter A through Z  
> *識別符號頭(Head)* → U+00A8, U+00AA, U+00AD, U+00AF, U+00B2–U+00B5, or U+00B7–U+00BA  
> *識別符號頭(Head)* → U+00BC–U+00BE, U+00C0–U+00D6, U+00D8–U+00F6, or U+00F8–U+00FF  
> *識別符號頭(Head)* → U+0100–U+02FF, U+0370–U+167F, U+1681–U+180D, or U+180F–U+1DBF  
> *識別符號頭(Head)* → U+1E00–U+1FFF  
> *識別符號頭(Head)* → U+200B–U+200D, U+202A–U+202E, U+203F–U+2040, U+2054, or U+2060–U+206F  
> *識別符號頭(Head)* → U+2070–U+20CF, U+2100–U+218F, U+2460–U+24FF, or U+2776–U+2793  
> *識別符號頭(Head)* → U+2C00–U+2DFF or U+2E80–U+2FFF  
> *識別符號頭(Head)* → U+3004–U+3007, U+3021–U+302F, U+3031–U+303F, or U+3040–U+D7FF  
> *識別符號頭(Head)* → U+F900–U+FD3D, U+FD40–U+FDCF, U+FDF0–U+FE1F, or U+FE30–U+FE44  
> *識別符號頭(Head)* → U+FE47–U+FFFD  
> *識別符號頭(Head)* → U+10000–U+1FFFD, U+20000–U+2FFFD, U+30000–U+3FFFD, or U+40000–U+4FFFD  
> *識別符號頭(Head)* → U+50000–U+5FFFD, U+60000–U+6FFFD, U+70000–U+7FFFD, or U+80000–U+8FFFD  
> *識別符號頭(Head)* → U+90000–U+9FFFD, U+A0000–U+AFFFD, U+B0000–U+BFFFD, or U+C0000–U+CFFFD  
> *識別符號頭(Head)* → U+D0000–U+DFFFD or U+E0000–U+EFFFD  
> *識別符號字元* → 數值 0 到 9  
> *識別符號字元* → U+0300–U+036F, U+1DC0–U+1DFF, U+20D0–U+20FF, or U+FE20–U+FE2F  
> *識別符號字元* → [*識別符號頭(Head)*](LexicalStructure.html#identifier_head)  
> *識別符號字元列表* → [*識別符號字元*](LexicalStructure.html#identifier_character) [*識別符號字元列表*](LexicalStructure.html#identifier_characters) _可選_  
> *隱式參數名* → **$** [*十進制數字列表*](LexicalStructure.html#decimal_digits)  

<a name="keywords"></a>
## 關鍵字

被保留的關鍵字（*keywords*）不允許用作識別符號，除非被反引號跳脫，參見 [識別符號](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/LexicalStructure.html#//apple_ref/doc/uid/TP40014097-CH30-XID_796)。

* **用作宣告的關鍵字：** *class*、*deinit*、*enum*、*extension*、*func*、*import*、*init*、*let*、*protocol*、*static*、*struct*、*subscript*、*typealias*、*var*
* **用作語句的關鍵字：** *break*、*case*、*continue*、*default*、*do*、*else*、*fallthrough*、*if*、*in*、*for*、*return*、*switch*、*where*、*while*
* **用作表達和型別的關鍵字：** *as*、*dynamicType*、*is*、*new*、*super*、*self*、*Self*、*Type*、*\_\_COLUMN\_\_*、*\_\_FILE\_\_*、*\_\_FUNCTION\_\_*、*\_\_LINE\_\_*
* **特定上下文中被保留的關鍵字：** *associativity*、*didSet*、*get*、*infix*、*inout*、*left*、*mutating*、*none*、*nonmutating*、*operator*、*override*、*postfix*、*precedence*、*prefix*、*right*、*set*、*unowned*、*unowned(safe)*、*unowned(unsafe)*、*weak*、*willSet*，這些關鍵字在特定上下文之外可以被用於識別符號。

<a name="literals"></a>
## 字面量

字面值表示整型、浮點型數字或文字型別的值，舉例如下：

```swift
42                 // 整型字面量
3.14159            // 浮點型字面量
"Hello, world!"    // 文字型字面量
```

> 字面量語法  
> *字面量* → [*整型字面量*](LexicalStructure.html#integer_literal) | [*浮點數字面量*](LexicalStructure.html#floating_point_literal) | [*字串字面量*](LexicalStructure.html#string_literal)  

### 整型字面量

整型字面量（*integer literals*）表示未指定精度整型數的值。整型字面量預設用十進制表示，可以加前綴來指定其他的進制，二進制字面量加 `0b`，八進制字面量加 `0o`，十六進制字面量加 `0x`。

十進制字面量包含數字 `0` 至 `9`。二進制字面量只包含 `0` 或 `1`，八進制字面量包含數字 `0` 至 `7`，十六進制字面量包含數字 `0` 至 `9` 以及字母 `A` 至 `F` （大小寫均可）。

負整數的字面量在數字前加減號 `-`，比如 `-42`。

允許使用底線 `_` 來增加數字的可讀性，底線不會影響字面量的值。整型字面量也可以在數字前加 `0`，同樣不會影響字面量的值。

```swift
1000_000     // 等於 1000000
005          // 等於 5
```

除非特殊指定，整型字面量的預設型別為 Swift 標準函式庫型別中的 `Int`。Swift 標準函式庫還定義了其他不同長度以及是否帶符號的整數型別，請參考 [整數型別](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/TheBasics.html#//apple_ref/doc/uid/TP40014097-CH5-XID_411)。

> 整型字面量語法  
> *整型字面量* → [*二進制字面量*](LexicalStructure.html#binary_literal)  
> *整型字面量* → [*八進制字面量*](LexicalStructure.html#octal_literal)  
> *整型字面量* → [*十進制字面量*](LexicalStructure.html#decimal_literal)  
> *整型字面量* → [*十六進制字面量*](LexicalStructure.html#hexadecimal_literal)  
> *二進制字面量* → **0b** [*二進制數字*](LexicalStructure.html#binary_digit) [*二進制字面量字元列表*](LexicalStructure.html#binary_literal_characters) _可選_  
> *二進制數字* → 數值 0 到 1  
> *二進制字面量字元* → [*二進制數字*](LexicalStructure.html#binary_digit) | **_**  
> *二進制字面量字元列表* → [*二進制字面量字元*](LexicalStructure.html#binary_literal_character) [*二進制字面量字元列表*](LexicalStructure.html#binary_literal_characters) _可選_  
> *八進制字面量* → **0o** [*八進字數字*](LexicalStructure.html#octal_digit) [*八進制字元列表*](LexicalStructure.html#octal_literal_characters) _可選_  
> *八進字數字* → 數值 0 到 7  
> *八進制字元* → [*八進字數字*](LexicalStructure.html#octal_digit) | **_**  
> *八進制字元列表* → [*八進制字元*](LexicalStructure.html#octal_literal_character) [*八進制字元列表*](LexicalStructure.html#octal_literal_characters) _可選_  
> *十進制字面量* → [*十進制數字*](LexicalStructure.html#decimal_digit) [*十進制字元列表*](LexicalStructure.html#decimal_literal_characters) _可選_  
> *十進制數字* → 數值 0 到 9  
> *十進制數字列表* → [*十進制數字*](LexicalStructure.html#decimal_digit) [*十進制數字列表*](LexicalStructure.html#decimal_digits) _可選_  
> *十進制字元* → [*十進制數字*](LexicalStructure.html#decimal_digit) | **_**  
> *十進制字元列表* → [*十進制字元*](LexicalStructure.html#decimal_literal_character) [*十進制字元列表*](LexicalStructure.html#decimal_literal_characters) _可選_  
> *十六進制字面量* → **0x** [*十六進制數字*](LexicalStructure.html#hexadecimal_digit) [*十六進制字面量字元列表*](LexicalStructure.html#hexadecimal_literal_characters) _可選_  
> *十六進制數字* → 數值 0 到 9, a through f, or A through F  
> *十六進制字元* → [*十六進制數字*](LexicalStructure.html#hexadecimal_digit) | **_**  
> *十六進制字面量字元列表* → [*十六進制字元*](LexicalStructure.html#hexadecimal_literal_character) [*十六進制字面量字元列表*](LexicalStructure.html#hexadecimal_literal_characters) _可選_  

### 浮點型字面量

浮點型字面量（*floating-point literals*）表示未指定精度浮點數的值。

浮點型字面量預設用十進制表示（無前綴），也可以用十六進制表示（加前綴 `0x`）。

十進制浮點型字面量（*decimal floating-point literals*）由十進制數字串後跟小數部分或指數部分（或兩者皆有）組成。十進制小數部分由小數點 `.` 後跟十進制數字串組成。指數部分由大寫或小寫字母 `e` 後跟十進制數字串組成，這串數字表示 `e` 之前的數量乘以 10 的幾次方。例如：`1.25e2` 表示 `1.25 ⨉ 10^2`，也就是 `125.0`；同樣，`1.25e－2` 表示 `1.25 ⨉ 10^－2`，也就是 `0.0125`。

十六進制浮點型字面量（*hexadecimal floating-point literals*）由前綴 `0x` 後跟可選的十六進制小數部分以及十六進制指數部分組成。十六進制小數部分由小數點後跟十六進制數字串組成。指數部分由大寫或小寫字母 `p` 後跟十進制數字串組成，這串數字表示 `p` 之前的數量乘以 2 的幾次方。例如：`0xFp2` 表示 `15 ⨉ 2^2`，也就是 `60`；同樣，`0xFp-2` 表示 `15 ⨉ 2^-2`，也就是 `3.75`。

與整型字面量不同，負的浮點型字面量由一元運算子減號 `-` 和浮點型字面量組成，例如 `-42.0`。這代表一個表達式，而不是一個浮點整型字面量。

允許使用底線 `_` 來增強可讀性，底線不會影響字面量的值。浮點型字面量也可以在數字前加 `0`，同樣不會影響字面量的值。

```swift
10_000.56     // 等於 10000.56
005000.76     // 等於 5000.76
```

除非特殊指定，浮點型字面量的預設型別為 Swift 標準函式庫型別中的 `Double`，表示64位浮點數。Swift 標準函式庫也定義 `Float` 型別，表示32位浮點數。

> 浮點型字面量語法  
> *浮點數字面量* → [*十進制字面量*](LexicalStructure.html#decimal_literal) [*十進制分數*](LexicalStructure.html#decimal_fraction) _可選_ [*十進制指數*](LexicalStructure.html#decimal_exponent) _可選_  
> *浮點數字面量* → [*十六進制字面量*](LexicalStructure.html#hexadecimal_literal) [*十六進制分數*](LexicalStructure.html#hexadecimal_fraction) _可選_ [*十六進制指數*](LexicalStructure.html#hexadecimal_exponent)  
> *十進制分數* → **.** [*十進制字面量*](LexicalStructure.html#decimal_literal)  
> *十進制指數* → [*浮點數e*](LexicalStructure.html#floating_point_e) [*正負號*](LexicalStructure.html#sign) _可選_ [*十進制字面量*](LexicalStructure.html#decimal_literal)  
> *十六進制分數* → **.** [*十六進制字面量*](LexicalStructure.html#hexadecimal_literal) _可選_  
> *十六進制指數* → [*浮點數p*](LexicalStructure.html#floating_point_p) [*正負號*](LexicalStructure.html#sign) _可選_ [*十六進制字面量*](LexicalStructure.html#hexadecimal_literal)  
> *浮點數e* → **e** | **E**  
> *浮點數p* → **p** | **P**  
> *正負號* → **+** | **-**  

### 文字型字面量

文字型字面量（*string literal*）由雙引號中的字串組成，形式如下：

```swift
"characters"
```

文字型字面量中不能包含未跳脫的雙引號 `"`、未跳脫的反斜線`\`、回車（*carriage return*）或換行（*line feed*）。

可以在文字型字面量中使用的跳脫特殊符號如下：

* 空字元（Null Character）`\0`
* 反斜線（Backslash）`\\`
* 水平 Tab （Horizontal Tab）`\t`
* 換行（Line Feed）`\n`
* 回車（Carriage Return）`\r`
* 雙引號（Double Quote）`\"`
* 單引號（Single Quote）`\'`

字元也可以用以下方式表示：

* `\x` 後跟兩位十六進制數字
* `\u` 後跟四位十六進制數字
* `\U` 後跟八位十六進制數字

後跟的數字表示一個 Unicode 碼點。

文字型字面量允許在反斜線小括號 `\()` 中插入表達式的值。插入表達式（*interpolated expression*）不能包含未跳脫的雙引號 `"`、反斜線 `\`、回車或者換行。表達式值的型別必須在 *String* 類別中有對應的初始化方法。

例如，以下所有文字型字面量的值相同：

```swift
"1 2 3"
"1 2 \(3)"
"1 2 \(1 + 2)"
var x = 3; "1 2 \(x)"
```

文字型字面量的預設型別為 `String`。組成字串的字元型別為 `Character`。更多有關 `String` 和 `Character` 的資訊請參照 [字串和字元](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/StringsAndCharacters.html#//apple_ref/doc/uid/TP40014097-CH7-XID_368)。

> 字元型字面量語法  
> *字串字面量* → **"** [*參考文字*](LexicalStructure.html#quoted_text) **"**  
> *參考文字* → [*參考文字條目*](LexicalStructure.html#quoted_text_item) [*參考文字*](LexicalStructure.html#quoted_text) _可選_  
> *參考文字條目* → [*跳脫字元*](LexicalStructure.html#escaped_character)  
> *參考文字條目* → **\(** [*表達式*](..\chapter3\04_Expressions.html#expression) **)**  
> *參考文字條目* → 除了"­, \­, U+000A, or U+000D的所有Unicode的字元  
> *跳脫字元* → **\0** | **\\** | **\t** | **\n** | **\r** | **\"** | **\'**  
> *跳脫字元* → **\x** [*十六進制數字*](LexicalStructure.html#hexadecimal_digit) [*十六進制數字*](LexicalStructure.html#hexadecimal_digit)  
> *跳脫字元* → **\u** [*十六進制數字*](LexicalStructure.html#hexadecimal_digit) [*十六進制數字*](LexicalStructure.html#hexadecimal_digit) [*十六進制數字*](LexicalStructure.html#hexadecimal_digit) [*十六進制數字*](LexicalStructure.html#hexadecimal_digit)  
> *跳脫字元* → **\U** [*十六進制數字*](LexicalStructure.html#hexadecimal_digit) [*十六進制數字*](LexicalStructure.html#hexadecimal_digit) [*十六進制數字*](LexicalStructure.html#hexadecimal_digit) [*十六進制數字*](LexicalStructure.html#hexadecimal_digit) [*十六進制數字*](LexicalStructure.html#hexadecimal_digit) [*十六進制數字*](LexicalStructure.html#hexadecimal_digit) [*十六進制數字*](LexicalStructure.html#hexadecimal_digit) [*十六進制數字*](LexicalStructure.html#hexadecimal_digit)  

<a name="operators"></a>
## 運算子

Swift 標準函式庫定義了許多可供使用的運算子，其中大部分在 [基礎運算子](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/BasicOperators.html#//apple_ref/doc/uid/TP40014097-CH6-XID_70) 和 [高級運算子](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/AdvancedOperators.html#//apple_ref/doc/uid/TP40014097-CH27-XID_28) 中進行了闡述。這裡將描述哪些字元能用作運算子。

運算子由一個或多個以下字元組成：
`/`、`=`、`-`、`+`、`!`、`*`、`%`、`<`、`>`、`&`、`|`、`^`、`~`、`.`。也就是說，標記 `=`, `->`、`//`、`/*`、`*/`、`.` 以及一元前綴運算子 `&` 屬於保留字，這些標記不能被重寫或用於自定義運算子。

運算子兩側的空白被用來區分該運算子是否為前綴運算子（*prefix operator*）、後綴運算子（*postfix operator*）或二元運算子（*binary operator*）。規則總結如下：

* 如果運算子兩側都有空白或兩側都無空白，將被看作二元運算子。例如：`a+b` 和 `a + b` 中的運算子 `+` 被看作二元運算子。
* 如果運算子只有左側空白，將被看作前綴一元運算子。例如 `a ++b` 中的 `++` 被看作前綴一元運算子。
* 如果運算子只有右側空白，將被看作後綴一元運算子。例如 `a++ b` 中的 `++` 被看作後綴一元運算子。
* 如果運算子左側沒有空白並緊跟 `.`，將被看作後綴一元運算子。例如 `a++.b` 中的 `++` 被看作後綴一元運算子（同理， `a++ . b` 中的 `++` 是後綴一元運算子而 `a ++ .b` 中的 `++` 不是）.

鑒於這些規則，運算子前的字元 `(`、`[` 和 `{` ；運算子後的字元 `)`、`]` 和 `}` 以及字元 `,`、`;` 和 `:` 都將用於空白檢測。

以上規則需注意一點，如果運算子 `!` 或 `?` 左側沒有空白，則不管右側是否有空白都將被看作後綴運算子。如果將 `?` 用作可選型別（*optional type*）修飾，左側必須無空白。如果用於條件運算子 `? :`，必須兩側都有空白。

在特定構成中 ，以 `<` 或 `>` 開頭的運算子會被分離成兩個或多個標記，剩余部分以同樣的方式會被再次分離。因此，在 `Dictionary<String, Array<Int>>` 中沒有必要添加空白來消除閉合字元 `>` 的歧義。在這個範例中， 閉合字元 `>` 被看作單字元標記，而不會被誤解為移位運算子 `>>`。

要學習如何自定義新的運算子，請參考 [自定義運算子](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/AdvancedOperators.html#//apple_ref/doc/uid/TP40014097-CH27-XID_48) 和 [運算子宣告](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Declarations.html#//apple_ref/doc/uid/TP40014097-CH34-XID_644)。學習如何重寫現有運算子，請參考 [運算子方法](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/AdvancedOperators.html#//apple_ref/doc/uid/TP40014097-CH27-XID_43)。

> 運算子語法語法  
> *運算子* → [*運算子字元*](LexicalStructure.html#operator_character) [*運算子*](LexicalStructure.html#operator) _可選_  
> *運算子字元* → **/** | **=** | **-** | **+** | **!** | **&#42;** | **%** | **<** | **>** | **&** | **|** | **^** | **~** | **.**  
> *二元運算子* → [*運算子*](LexicalStructure.html#operator)  
> *前綴運算子* → [*運算子*](LexicalStructure.html#operator)  
> *後綴運算子* → [*運算子*](LexicalStructure.html#operator)  
