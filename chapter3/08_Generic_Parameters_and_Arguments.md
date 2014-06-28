> 翻譯：[fd5788](https://github.com/fd5788)
> 校對：[yankuangshi](https://github.com/yankuangshi), [stanzhai](https://github.com/stanzhai)

# 泛型參數
---------

本頁包含內容：

- [泛型形參子句](#generic_parameter)
- [泛型實參子句](#generic_argument)

本節涉及泛型型別、泛型函式以及泛型建構器的參數，包括形參和實參。宣告泛型型別、函式或建構器時，須指定相應的型別參數。型別參數相當於一個占位符，當實例化泛型型別、呼叫泛型函式或泛型建構器時，就用具體的型別實參替代之。

關於 Swift 語言的泛型概述，見[泛型](../charpter2/22_Generics.md)(第二部分第22章)。

<a name="generic_parameter"></a>
## 泛型形參子句

泛型形參子句指定泛型型別或函式的型別形參，以及這些參數的關聯約束和要求。泛型形參子句用角括號（<>）包住，並且有以下兩種形式：

> <`generic parameter list`>  
> <`generic parameter list` where `requirements`>

泛型形參列表中泛型形參用逗號分開，每一個采用以下形式：

> `type parameter` : `constrain`

泛型形參由兩部分組成：型別形參及其後的可選約束。型別形參只是占位符型別（如T，U，V，KeyType，ValueType等）的名字而已。你可以在泛型型別、函式的其余部分或者建構器宣告，以及函式或建構器的簽名中使用它。

約束用於指明該型別形參繼承自某個類別或者遵守某個協定或協定的一部分。例如，在下面的泛型中，泛型形參`T: Comparable`表示任何用於替代型別形參`T`的型別實參必須滿足`Comparable`協定。

```swift
func simpleMin<T: COmparable>(x: T, y: T) -> T {
    if x < y {
        return y
    }
    return x
}
```

如，`Int`和`Double`均滿足`Comparable`協定，該函式接受任何一種型別。與泛型型別相反，呼叫泛型函式或建構器時不需要指定泛型實參子句。型別實參由傳遞給函式或建構器的實參推斷而出。

```swift
simpleMin(17, 42) // T is inferred to be Int
simpleMin(3.14159, 2.71828) // T is inferred to be Double
```

## Where 子句

要想對型別形參及其關聯型別指定額外要求，可以在泛型形參列表之後添加`where`子句。`where`子句由關鍵字`where`及其後的用逗號分割的多個要求組成。

`where`子句中的要求用於指明該型別形參繼承自某個類別或遵守某個協定或協定的一部分。儘管`where`子句有助於表達型別形參上的簡單約束（如`T: Comparable`等同於`T where T: Comparable`，等等），但是依然可以用來對型別形參及其關聯約束提供更複雜的約束。如，`<T where T: C, T: P>`表示泛型型別`T`繼承自類別`C`且遵守協定`P`。

如上所述，可以強制約束型別形參的關聯型別遵守某個協定。`<T: Generator where T.Element: Equatable>`表示`T`遵守`Generator`協定，而且`T`的關聯型別`T.Element`遵守`Eauatable`協定（`T`有關聯型別是因為`Generator`宣告了`Element`，而`T`遵守`Generator`協定）。

也可以用運算子`==`來指定兩個型別等效的要求。例如，有這樣一個約束：`T`和`U`遵守`Generator`協定，同時要求它們的關聯型別等同，可以這樣來表達：`<T: Generator, U: Generator where T.Element == U.Element>`。

當然，替代型別形參的型別實參必須滿足所有型別形參所要求的約束和要求。

泛型函式或建構器可以重載，但在泛型形參子句中的型別形參必須有不同的約束或要求，抑或二者皆不同。當呼叫重載的泛型函式或建構器時，編譯器會用這些約束來決定呼叫哪個重載函式或建構器。

泛型類別可以生成一個子類別，但是這個子類別也必須是泛型類別。

> 泛型形參子句語法  
> *泛型參數子句* → **<** [*泛型參數列表*](GenericParametersAndArguments.html#generic_parameter_list) [*約束子句*](GenericParametersAndArguments.html#requirement_clause) _可選_ **>**  
> *泛型參數列表* → [*泛形參數*](GenericParametersAndArguments.html#generic_parameter) | [*泛形參數*](GenericParametersAndArguments.html#generic_parameter) **,** [*泛型參數列表*](GenericParametersAndArguments.html#generic_parameter_list)  
> *泛形參數* → [*型別名稱*](..\chapter3\03_Types.html#type_name)  
> *泛形參數* → [*型別名稱*](..\chapter3\03_Types.html#type_name) **:** [*型別標識*](..\chapter3\03_Types.html#type_identifier)  
> *泛形參數* → [*型別名稱*](..\chapter3\03_Types.html#type_name) **:** [*協定合成型別*](..\chapter3\03_Types.html#protocol_composition_type)  
> *約束子句* → **where** [*約束列表*](GenericParametersAndArguments.html#requirement_list)  
> *約束列表* → [*約束*](GenericParametersAndArguments.html#requirement) | [*約束*](GenericParametersAndArguments.html#requirement) **,** [*約束列表*](GenericParametersAndArguments.html#requirement_list)  
> *約束* → [*一致性約束*](GenericParametersAndArguments.html#conformance_requirement) | [*同型別約束*](GenericParametersAndArguments.html#same_type_requirement)  
> *一致性約束* → [*型別標識*](..\chapter3\03_Types.html#type_identifier) **:** [*型別標識*](..\chapter3\03_Types.html#type_identifier)  
> *一致性約束* → [*型別標識*](..\chapter3\03_Types.html#type_identifier) **:** [*協定合成型別*](..\chapter3\03_Types.html#protocol_composition_type)  
> *同型別約束* → [*型別標識*](..\chapter3\03_Types.html#type_identifier) **==** [*型別標識*](..\chapter3\03_Types.html#type_identifier)  


<a name="generic_argument"></a>
## 泛型實參子句

泛型實參子句指定_泛型型別_的型別實參。泛型實參子句用角括號（<>）包住，形式如下：

> <`generic argument list`>

泛型實參列表中型別實參有逗號分開。型別實參是實際具體型別的名字，用來替代泛型型別的泛型形參子句中的相應的型別形參。從而得到泛型型別的一個特化版本。如，Swift標準函式庫的泛型字典型別定義如下：

```swift
struct Dictionary<KeyTypel: Hashable, ValueType>: Collection, DictionaryLiteralConvertible {
    /* .. */
}
```

泛型`Dictionary`型別的特化版本，`Dictionary<String, Int>`就是用具體的`String`和`Int`型別替代泛型型別`KeyType: Hashable`和`ValueType`產生的。每一個型別實參必須滿足它所替代的泛型形參的所有約束，包括任何`where`子句所指定的額外的要求。上面的範例中，型別形參`KeyType`要求滿足`Hashable`協定，因此`String`也必須滿足`Hashable`協定。

可以用本身就是泛型型別的特化版本的型別實參替代型別形參（假設已滿足合適的約束和要求）。例如，為了生成一個元素型別是整型陣列的陣列，可以用陣列的特化版本`Array<Int>`替代泛型型別`Array<T>`的型別形參`T`來實作。

```swift
let arrayOfArrays: Array<Array<Int>> = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
```

如[泛型形參子句](#generic_parameter)所述，不能用泛型實參子句來指定泛型函式或建構器的型別實參。

> 泛型實參子句語法  
> *(泛型參數子句Generic Argument Clause)* → **<** [*泛型參數列表*](GenericParametersAndArguments.html#generic_argument_list) **>**  
> *泛型參數列表* → [*泛型參數*](GenericParametersAndArguments.html#generic_argument) | [*泛型參數*](GenericParametersAndArguments.html#generic_argument) **,** [*泛型參數列表*](GenericParametersAndArguments.html#generic_argument_list)  
> *泛型參數* → [*型別*](..\chapter3\03_Types.html#type)  