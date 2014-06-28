> 翻譯：[zqp](https://github.com/zqp)
> 校對：[shinyzhu](https://github.com/shinyzhu), [stanzhai](https://github.com/stanzhai)  

# 集合型別 (Collection Types)
-----------------

本頁包含內容：

- [陣列（Arrays）](#arrays)
- [字典（Dictionaries）](#dictionaries)
- [集合的可變性（Mutability of Collections）](#mutability_of_collections)

Swift 語言提供經典的陣列和字典兩種集合型別來儲存集合資料。陣列用來按順序儲存相同型別的資料。字典雖然無序儲存相同型別資料值但是需要由獨有的識別符號參考和尋址（就是鍵值對）。

Swift 語言裡的陣列和字典中儲存的資料值型別必須明確。 這意味著我們不能把不正確的資料型別插入其中。 同時這也說明我們完全可以對獲取出的值型別非常自信。 Swift 對顯式型別集合的使用確保了我們的程式碼對工作所需要的型別非常清楚，也讓我們在開發中可以早早地找到任何的型別不匹配錯誤。

> 注意：  
Swift 的陣列結構在被宣告成常數和變數或者被傳入函式與方法中時會相對於其他型別展現出不同的特性。 獲取更多資訊請參見[集合的可變性](#mutability_of_collections)與[集合在賦值和複製中的行為](09_Classes_and_Structures.html#assignment_and_copy_behavior_for_collection_types)章節。

<a name="arrays"></a>
## 陣列

陣列使用有序列表儲存同一型別的多個值。相同的值可以多次出現在一個陣列的不同位置中。

Swift 陣列特定於它所儲存元素的型別。這與 Objective-C 的 NSArray 和 NSMutableArray 不同，這兩個類別可以儲存任意型別的物件，並且不提供所回傳物件的任何特別資訊。在 Swift 中，資料值在被儲存進入某個陣列之前型別必須明確，方法是通過顯式的型別標注或型別推斷，而且不是必須是`class`型別。例如： 如果我們創建了一個`Int`值型別的陣列，我們不能往其中插入任何不是`Int`型別的資料。 Swift 中的陣列是型別安全的，並且它們中包含的型別必須明確。

<a name="array_type_shorthand_syntax"></a>
### 陣列的簡單語法

寫 Swift 陣列應該遵循像`Array<SomeType>`這樣的形式，其中`SomeType`是這個陣列中唯一允許存在的資料型別。 我們也可以使用像`SomeType[]`這樣的簡單語法。 儘管兩種形式在功能上是一樣的，但是推薦較短的那種，而且在本文中都會使用這種形式來使用陣列。

<a name="array_literals"></a>
### 陣列建構語法

我們可以使用字面量來進行陣列建構，這是一種用一個或者多個數值建構陣列的簡單方法。字面量是一系列由逗號分割並由方括號包含的數值。
`[value 1, value 2, value 3]`。

下面這個範例創建了一個叫做`shoppingList`並且儲存字串的陣列：

```swift
var shoppingList: String[] = ["Eggs", "Milk"]
// shoppingList 已經被建構並且擁有兩個初始項。
```

`shoppingList`變數被宣告為「字串值型別的陣列「，記作`String[]`。 因為這個陣列被規定只有`String`一種資料結構，所以只有`String`型別可以在其中被存取。 在這裡，`shoppinglist`陣列由兩個`String`值（`"Eggs"` 和`"Milk"`）建構，並且由字面量定義。

> 注意：  
> `Shoppinglist`陣列被宣告為變數（`var`關鍵字創建）而不是常數（`let`創建）是因為以後可能會有更多的資料項被插入其中。  

在這個範例中，字面量僅僅包含兩個`String`值。匹配了該陣列的變數宣告（只能包含`String`的陣列），所以這個字面量的分配過程就是允許用兩個初始項來建構`shoppinglist`。

由於 Swift 的型別推斷機制，當我們用字面量建構只擁有相同型別值陣列的時候，我們不必把陣列的型別定義清楚。 `shoppinglist`的建構也可以這樣寫：

```swift
var shoppingList = ["Eggs", "Milk"]
```

因為所有字面量中的值都是相同的型別，Swift 可以推斷出`String[]`是`shoppinglist`中變數的正確型別。

<a name="accessing_and_modifying_an_array"></a>
### 存取和修改陣列

我們可以通過陣列的方法和屬性來存取和修改陣列，或者下標語法。
還可以使用陣列的唯讀屬性`count`來獲取陣列中的資料項數量。

```swift
println("The shopping list contains \(shoppingList.count) items.")
// 輸出"The shopping list contains 2 items."（這個陣列有2個項）
```

使用布林項`isEmpty`來作為檢查`count`屬性的值是否為 0 的捷徑。

```swift
if shoppingList.isEmpty {
    println("The shopping list is empty.")
} else {
    println("The shopping list is not empty.")
}
// 列印 "The shopping list is not empty."（shoppinglist不是空的）
```

也可以使用`append`方法在陣列後面添加新的資料項：

```swift
shoppingList.append("Flour")
// shoppingList 現在有3個資料項，有人在攤煎餅
```

除此之外，使用加法賦值運算子（`+=`）也可以直接在陣列後面添加資料項：

```swift
shoppingList += "Baking Powder"
// shoppingList 現在有四項了
```

我們也可以使用加法賦值運算子（`+=`）直接添加擁有相同型別資料的陣列。

```swift
shoppingList += ["Chocolate Spread", "Cheese", "Butter"]
// shoppingList 現在有7項了
```

可以直接使用下標語法來獲取陣列中的資料項，把我們需要的資料項的索引值放在直接放在陣列名稱的方括號中：

```swift
var firstItem = shoppingList[0]
// 第一項是 "Eggs"
```

注意第一項在陣列中的索引值是`0`而不是`1`。 Swift 中的陣列索引總是從零開始。

我們也可以用下標來改變某個已有索引值對應的資料值：

```swift
shoppingList[0] = "Six eggs"
// 其中的第一項現在是 "Six eggs" 而不是 "Eggs"
```

還可以利用下標來一次改變一系列資料值，即使新資料和原有資料的數量是不一樣的。下面的範例把`"Chocolate Spread"`，`"Cheese"`，和`"Butter"`替換為`"Bananas"`和 `"Apples"`：

```swift
shoppingList[4...6] = ["Bananas", "Apples"]
// shoppingList 現在有六項
```

> 注意：  
>我們不能使用下標語法在陣列尾部添加新項。如果我們試著用這種方法對索引越界的資料進行檢索或者設置新值的操作，我們會引發一個執行期錯誤。我們可以使用索引值和陣列的`count`屬性進行比較來在使用某個索引之前先檢驗是否有效。除了當`count`等於 0 時（說明這是個空陣列），最大索引值一直是`count - 1`，因為陣列都是零起索引。  

呼叫陣列的`insert(atIndex:)`方法來在某個具體索引值之前添加資料項：

```swift
shoppingList.insert("Maple Syrup", atIndex: 0)
// shoppingList 現在有7項
// "Maple Syrup" 現在是這個列表中的第一項
```

這次`insert`函式呼叫把值為`"Maple Syrup"`的新資料項插入列表的最開始位置，並且使用`0`作為索引值。

類似的我們可以使用`removeAtIndex`方法來移除陣列中的某一項。這個方法把陣列在特定索引值中儲存的資料項移除並且回傳這個被移除的資料項（我們不需要的時候就可以無視它）:

```swift
let mapleSyrup = shoppingList.removeAtIndex(0)
// 索引值為0的資料項被移除
// shoppingList 現在只有6項，而且不包括Maple Syrup
// mapleSyrup常數的值等於被移除資料項的值 "Maple Syrup"
```

資料項被移除後陣列中的空出項會被自動填補，所以現在索引值為`0`的資料項的值再次等於`"Six eggs"`:

```swift
firstItem = shoppingList[0]
// firstItem 現在等於 "Six eggs"
```

如果我們只想把陣列中的最後一項移除，可以使用`removeLast`方法而不是`removeAtIndex`方法來避免我們需要獲取陣列的`count`屬性。就像後者一樣，前者也會回傳被移除的資料項：

```swift
let apples = shoppingList.removeLast()
// 陣列的最後一項被移除了
// shoppingList現在只有5項，不包括cheese
// apples 常數的值現在等於"Apples" 字串
```

<a name="iterating_over_an_array"></a>
### 陣列的遍歷

我們可以使用`for-in`迴圈來遍歷所有陣列中的資料項：

```swift
for item in shoppingList {
    println(item)
}
// Six eggs
// Milk
// Flour
// Baking Powder
// Bananas
```

如果我們同時需要每個資料項的值和索引值，可以使用全域`enumerate`函式來進行陣列遍歷。`enumerate`回傳一個由每一個資料項索引值和資料值組成的元組。我們可以把這個元組分解成臨時常數或者變數來進行遍歷：

```swift
for (index, value) in enumerate(shoppingList) {
    println("Item \(index + 1): \(value)")
}
// Item 1: Six eggs
// Item 2: Milk
// Item 3: Flour
// Item 4: Baking Powder
// Item 5: Bananas
```

更多關於`for-in`迴圈的介紹請參見[for 迴圈](05_Control_Flow.html#for_loops)。

<a name="creating_and_initializing_an_array"></a>
### 創建並且建構一個陣列

我們可以使用建構語法來創建一個由特定資料型別構成的空陣列：

```swift
var someInts = Int[]()
println("someInts is of type Int[] with \(someInts.count) items。")
// 列印 "someInts is of type Int[] with 0 items。"（someInts是0資料項的Int[]陣列）
```

注意`someInts`被設置為一個`Int[]`建構函式的輸出所以它的變數型別被定義為`Int[]`。

除此之外，如果程式碼上下文中提供了型別資訊， 例如一個函式參數或者一個已經定義好型別的常數或者變數，我們可以使用空陣列語句創建一個空陣列，它的寫法很簡單：`[]`（一對空方括號）：

```swift
someInts.append(3)
// someInts 現在包含一個INT值
someInts = []
// someInts 現在是空陣列，但是仍然是Int[]型別的。
```

Swift 中的`Array`型別還提供一個可以創建特定大小並且所有資料都被預設的建構方法。我們可以把準備加入新陣列的資料項數量（`count`）和適當型別的初始值（`repeatedValue`）傳入陣列建構函式：

```swift
var threeDoubles = Double[](count: 3, repeatedValue:0.0)
// threeDoubles 是一種 Double[]陣列, 等於 [0.0, 0.0, 0.0]
```

因為型別推斷的存在，我們使用這種建構方法的時候不需要特別指定陣列中儲存的資料型別，因為型別可以從預設值推斷出來：

```swift
var anotherThreeDoubles = Array(count: 3, repeatedValue: 2.5)
// anotherThreeDoubles is inferred as Double[], and equals [2.5, 2.5, 2.5]
```

最後，我們可以使用加法運算子（`+`）來組合兩種已存在的相同型別陣列。新陣列的資料型別會被從兩個陣列的資料型別中推斷出來：

```swift
var sixDoubles = threeDoubles + anotherThreeDoubles
// sixDoubles 被推斷為 Double[], 等於 [0.0, 0.0, 0.0, 2.5, 2.5, 2.5]
```

<a name="dictionaries"></a>
## 字典

字典是一種儲存多個相同型別的值的容器。每個值（value）都關聯唯一的鍵（key），鍵作為字典中的這個值資料的識別符號。和陣列中的資料項不同，字典中的資料項並沒有具體順序。我們在需要通過識別符號（鍵）存取資料的時候使用字典，這種方法很大程度上和我們在現實世界中使用字典查字義的方法一樣。

Swift 的字典使用時需要具體規定可以儲存鍵和值型別。不同於 Objective-C 的`NSDictionary`和`NSMutableDictionary` 類別可以使用任何型別的物件來作鍵和值並且不提供任何關於這些物件的本質資訊。在 Swift 中，在某個特定字典中可以儲存的鍵和值必須提前定義清楚，方法是通過顯性型別標注或者型別推斷。

Swift 的字典使用`Dictionary<KeyType, ValueType>`定義,其中`KeyType`是字典中鍵的資料型別，`ValueType`是字典中對應於這些鍵所儲存值的資料型別。

`KeyType`的唯一限制就是可雜湊的，這樣可以保證它是獨一無二的，所有的 Swift 基本型別（例如`String`，`Int`， `Double`和`Bool`）都是預設可雜湊的，並且所有這些型別都可以在字典中當做鍵使用。未關聯值的列舉成員（參見[列舉](08_Enumerations.html)）也是預設可雜湊的。

<a name="dictionary_literals"></a>
## 字典字面量

我們可以使用字典字面量來建構字典，它們和我們剛才介紹過的陣列字面量擁有相似語法。一個字典字面量是一個定義擁有一個或者多個鍵值對的字典集合的簡單語句。

一個鍵值對是一個`key`和一個`value`的結合體。在字典字面量中，每一個鍵值對的鍵和值都由冒號分割。這些鍵值對構成一個列表，其中這些鍵值對由方括號包含並且由逗號分割：

```swift
[key 1: value 1, key 2: value 2, key 3: value 3]
```

下面的範例創建了一個儲存國際機場名稱的字典。在這個字典中鍵是三個字母的國際航空運輸相關程式碼，值是機場名稱：

```swift
var airports: Dictionary<String, String> = ["TYO": "Tokyo", "DUB": "Dublin"]
```

`airports`字典被定義為一種`Dictionary<String, String>`,它意味著這個字典的鍵和值都是`String`型別。

> 注意：  
> `airports`字典被宣告為變數（用`var`關鍵字）而不是常數（`let`關鍵字）因為後來更多的機場資訊會被添加到這個示例字典中。  

`airports`字典使用字典字面量初始化，包含兩個鍵值對。第一對的鍵是`TYO`，值是`Tokyo`。第二對的鍵是`DUB`，值是`Dublin`。

這個字典語句包含了兩個`String: String`型別的鍵值對。它們對應`airports`變數宣告的型別（一個只有`String`鍵和`String`值的字典）所以這個字典字面量是建構兩個初始資料項的`airport`字典。

和陣列一樣，如果我們使用字面量建構字典就不用把型別定義清楚。`airports`的也可以用這種方法簡短定義：

```swift
var airports = ["TYO": "Tokyo", "DUB": "Dublin"]
```

因為這個語句中所有的鍵和值都分別是相同的資料型別，Swift 可以推斷出`Dictionary<String, String>`是`airports`字典的正確型別。

<a name="accessing_and_modifying_a_dictionary"></a>
### 讀取和修改字典

我們可以通過字典的方法和屬性來讀取和修改字典，或者使用下標語法。和陣列一樣，我們可以通過字典的唯讀屬性`count`來獲取某個字典的資料項數量：

```swift
println("The dictionary of airports contains \(airports.count) items.")
// 列印 "The dictionary of airports contains 2 items."（這個字典有兩個資料項）
```

我們也可以在字典中使用下標語法來添加新的資料項。可以使用一個合適型別的 key 作為下標索引，並且分配新的合適型別的值：

```swift
airports["LHR"] = "London"
// airports 字典現在有三個資料項
```

我們也可以使用下標語法來改變特定鍵對應的值：

```swift
airports["LHR"] = "London Heathrow"
// "LHR"對應的值 被改為 "London Heathrow
```

作為另一種下標方法，字典的`updateValue(forKey:)`方法可以設置或者更新特定鍵對應的值。就像上面所示的示例，`updateValue(forKey:)`方法在這個鍵不存在對應值的時候設置值或者在存在時更新已存在的值。和上面的下標方法不一樣，這個方法回傳更新值之前的原值。這樣方便我們檢查更新是否成功。

`updateValue(forKey:)`函式會回傳包含一個字典值型別的可選值。舉例來說：對於儲存`String`值的字典，這個函式會回傳一個`String?`或者「可選 `String`」型別的值。如果值存在，則這個可選值值等於被替換的值，否則將會是`nil`。

```swift
if let oldValue = airports.updateValue("Dublin Internation", forKey: "DUB") {
    println("The old value for DUB was \(oldValue).")
}
// 輸出 "The old value for DUB was Dublin."（DUB原值是dublin）
```

我們也可以使用下標語法來在字典中檢索特定鍵對應的值。由於使用一個沒有值的鍵這種情況是有可能發生的，可選型別回傳這個鍵存在的相關值，否則就回傳`nil`：

```swift
if let airportName = airports["DUB"] {
    println("The name of the airport is \(airportName).")
} else {
    println("That airport is not in the airports dictionary.")
}
// 列印 "The name of the airport is Dublin Internation."（機場的名字是都柏林國際）
```

我們還可以使用下標語法來通過給某個鍵的對應值賦值為`nil`來從字典裡移除一個鍵值對：

```swift
airports["APL"] = "Apple Internation"
// "Apple Internation"不是真的 APL機場, 刪除它
airports["APL"] = nil
// APL現在被移除了
```

另外，`removeValueForKey`方法也可以用來在字典中移除鍵值對。這個方法在鍵值對存在的情況下會移除該鍵值對並且回傳被移除的value或者在沒有值的情況下回傳`nil`：

```swift
if let removedValue = airports.removeValueForKey("DUB") {
    println("The removed airport's name is \(removedValue).")
} else {
    println("The airports dictionary does not contain a value for DUB.")
}
// prints "The removed airport's name is Dublin International."
```

<a name="iterating_over_a_dictionary"></a>
### 字典遍歷

我們可以使用`for-in`迴圈來遍歷某個字典中的鍵值對。每一個字典中的資料項都由`(key, value)`元組形式回傳，並且我們可以使用臨時常數或者變數來分解這些元組：

```swift
for (airportCode, airportName) in airports {
    println("\(airportCode): \(airportName)")
}
// TYO: Tokyo
// LHR: London Heathrow
```

`for-in`迴圈請參見[For 迴圈](05_Control_Flow.html#for_loops)。

我們也可以通過存取它的`keys`或者`values`屬性（都是可遍歷集合）檢索一個字典的鍵或者值：

```swift
for airportCode in airports.keys {
    println("Airport code: \(airportCode)")
}
// Airport code: TYO
// Airport code: LHR

for airportName in airports.values {
    println("Airport name: \(airportName)")
}
// Airport name: Tokyo
// Airport name: London Heathrow
```

如果我們只是需要使用某個字典的鍵集合或者值集合來作為某個接受`Array`實例 API 的參數，可以直接使用`keys`或者`values`屬性直接建構一個新陣列：

```swift
let airportCodes = Array(airports.keys)
// airportCodes is ["TYO", "LHR"]

let airportNames = Array(airports.values)
// airportNames is ["Tokyo", "London Heathrow"]
```

> 注意：  
> Swift 的字典型別是無序集合型別。其中字典鍵，值，鍵值對在遍歷的時候會重新排列，而且其中順序是不固定的。  

<a name="creating_an_empty_dictionary"></a>
### 創建一個空字典

我們可以像陣列一樣使用建構語法創建一個空字典：

```swift
var namesOfIntegers = Dictionary<Int, String>()
// namesOfIntegers 是一個空的 Dictionary<Int, String>
```

這個範例創建了一個`Int, String`型別的空字典來儲存英語對整數的命名。它的鍵是`Int`型，值是`String`型。

如果上下文已經提供了資訊型別，我們可以使用空字典字面量來創建一個空字典，記作`[:]`（中括號中放一個冒號）：

```swift
namesOfIntegers[16] = "sixteen"
// namesOfIntegers 現在包含一個鍵值對
namesOfIntegers = [:]
// namesOfIntegers 又成為了一個 Int, String型別的空字典
```

> 注意：  
> 在後台，Swift 的陣列和字典都是由泛型集合來實作的，想了解更多泛型和集合資訊請參見[泛型](22_Generics.html)。  

<a name="mutability_of_collections"></a>
## 集合的可變性

陣列和字典都是在單個集合中儲存可變值。如果我們創建一個陣列或者字典並且把它分配成一個變數，這個集合將會是可變的。這意味著我們可以在創建之後添加更多或移除已存在的資料項來改變這個集合的大小。與此相反，如果我們把陣列或字典分配成常數，那麼它就是不可變的，它的大小不能被改變。

對字典來說，不可變性也意味著我們不能替換其中任何現有鍵所對應的值。不可變字典的內容在被首次設定之後不能更改。
不可變性對陣列來說有一點不同，當然我們不能試著改變任何不可變陣列的大小，但是我們可以重新設定相對現存索引所對應的值。這使得 Swift 陣列在大小被固定的時候依然可以做的很棒。

Swift 陣列的可變性行為同時影響了陣列實例如何被分配和修改，想獲取更多資訊，請參見[集合在賦值和複製中的行為](09_Classes_and_Structures.html#assignment_and_copy_behavior_for_collection_types)。

> 注意：  
> 在我們不需要改變陣列大小的時候創建不可變陣列是很好的習慣。如此 Swift 編譯器可以優化我們創建的集合。  