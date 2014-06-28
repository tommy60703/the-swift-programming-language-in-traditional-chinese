> 翻譯：[shinyzhu](https://github.com/shinyzhu)
> 校對：[pp-prog](https://github.com/pp-prog)

# 屬性 (Properties)
---

本頁包含內容：

- [儲存屬性（Stored Properties）](#stored_properties)
- [計算屬性（Computed Properties）](#computed_properties)
- [屬性監視器（Property Observers）](#property_observers)
- [全域變數和局部變數（Global and Local Variables）](global_and_local_variables)
- [型別屬性（Type Properties）](#type_properties)

**屬性**將值跟特定的類別、結構或列舉關聯。儲存屬性儲存常數或變數作為實例的一部分，計算屬性計算（而不是儲存）一個值。計算屬性可以用於類別、結構和列舉裡，儲存屬性只能用於類別和結構。

儲存屬性和計算屬性通常用於特定型別的實例，但是，屬性也可以直接用於型別本身，這種屬性稱為型別屬性。

另外，還可以定義屬性監視器來監控屬性值的變化，以此來觸發一個自定義的操作。屬性監視器可以添加到自己寫的儲存屬性上，也可以添加到從父類別繼承的屬性上。

<a name="stored_properties"></a>
## 儲存屬性

簡單來說，一個儲存屬性就是儲存在特定類別或結構的實例裡的一個常數或變數，儲存屬性可以是*變數儲存屬性*（用關鍵字`var`定義），也可以是*常數儲存屬性*（用關鍵字`let`定義）。

可以在定義儲存屬性的時候指定預設值，請參考[建構過程](../chapter2/14_Initialization.html)一章的[預設屬性值](../chapter2/14_Initialization.html#default_property_values)一節。也可以在建構過程中設置或修改儲存屬性的值，甚至修改常數儲存屬性的值，請參考[建構過程](../chapter2/14_Initialization.html)一章的[在初始化階段修改常數儲存屬性](../chapter2/14_Initialization.html#modifying_constant_properties_during_initialization)一節。

下面的範例定義了一個名為`FixedLengthRange`的結構，它描述了一個在創建後無法修改值域寬度的區間：

```swift
struct FixedLengthRange {
    var firstValue: Int
    let length: Int
}
var rangeOfThreeItems = FixedLengthRange(firstValue: 0, length: 3)
// 該區間表示整數0，1，2
rangeOfThreeItems.firstValue = 6
// 該區間現在表示整數6，7，8
```

`FixedLengthRange`的實例包含一個名為`firstValue`的變數儲存屬性和一個名為`length`的常數儲存屬性。在上面的範例中，`length`在創建實例的時候被賦值，因為它是一個常數儲存屬性，所以之後無法修改它的值。

<a name="stored_properties_of_constant_structure_instances"></a>
### 常數和儲存屬性

如果創建了一個結構的實例並賦值給一個常數，則無法修改實例的任何屬性，即使定義了變數儲存屬性：

```swift
let rangeOfFourItems = FixedLengthRange(firstValue: 0, length: 4)
// 該區間表示整數0，1，2，3
rangeOfFourItems.firstValue = 6
// 儘管 firstValue 是個變數屬性，這裡還是會報錯
```

因為`rangeOfFourItems`宣告成了常數（用`let`關鍵字），即使`firstValue`是一個變數屬性，也無法再修改它了。

這種行為是由於結構（struct）屬於*值型別*。當值型別的實例被宣告為常數的時候，它的所有屬性也就成了常數。

屬於*參考型別*的類別（class）則不一樣，把一個參考型別的實例賦給一個常數後，仍然可以修改實例的變數屬性。

<a name="lazy_stored_properties"></a>
### 延遲儲存屬性

延遲儲存屬性是指當第一次被呼叫的時候才會計算其初始值的屬性。在屬性宣告前使用`@lazy`來標示一個延遲儲存屬性。

> 注意：  
> 必須將延遲儲存屬性宣告成變數（使用`var`關鍵字），因為屬性的值在實例建構完成之前可能無法得到。而常數屬性在建構過程完成之前必須要有初始值，因此無法宣告成延遲屬性。  

延遲屬性很有用，當屬性的值依賴於在實例的建構過程結束前無法知道具體值的外部因素時，或者當屬性的值需要複雜或大量計算時，可以只在需要的時候來計算它。

下面的範例使用了延遲儲存屬性來避免複雜類別的不必要的初始化。範例中定義了`DataImporter`和`DataManager`兩個類別，下面是部分程式碼：

```swift
class DataImporter {
    /*
    DataImporter 是一個將外部文件中的資料導入的類別。
    這個類別的初始化會消耗不少時間。
    */
    var fileName = "data.txt"
    // 這是提供資料導入功能
}

class DataManager {
    @lazy var importer = DataImporter()
    var data = String[]()
    // 這是提供資料管理功能
}

let manager = DataManager()
manager.data += "Some data"
manager.data += "Some more data"
// DataImporter 實例的 importer 屬性還沒有被創建
```

`DataManager`類別包含一個名為`data`的儲存屬性，初始值是一個空的字串（`String`）陣列。雖然沒有寫出全部程式碼，`DataManager`類別的目的是管理和提供對這個字串陣列的存取。

`DataManager`的一個功能是從文件導入資料，該功能由`DataImporter`類別提供，`DataImporter`需要消耗不少時間完成初始化：因為它的實例在初始化時可能要打開文件，還要讀取文件內容到內存。

`DataManager`也可能不從文件中導入資料。所以當`DataManager`的實例被創建時，沒必要創建一個`DataImporter`的實例，更明智的是當用到`DataImporter`的時候才去創建它。

由於使用了`@lazy`，`importer`屬性只有在第一次被存取的時候才被創建。比如存取它的屬性`fileName`時：

```swift
println(manager.importer.fileName)
// DataImporter 實例的 importer 屬性現在被創建了
// 輸出 "data.txt」
```

<a name="stored_properties_and_instance_variables"></a>
### 儲存屬性和實例變數

如果你有過 Objective-C 經驗，應該知道有兩種方式在類別實例儲存值和參考。對於屬性來說，也可以使用實例變數作為屬性值的後端儲存。

Swift 程式語言中把這些理論統一用屬性來實作。Swift 中的屬性沒有對應的實例變數，屬性的後端儲存也無法直接存取。這就避免了不同場景下存取方式的困擾，同時也將屬性的定義簡化成一個語句。
一個型別中屬性的全部資訊——包括命名、型別和內存管理特征——都在唯一一個地方（型別定義中）定義。

<a name="computed_properties"></a>
## 計算屬性

除儲存屬性外，類別、結構和列舉可以定義*計算屬性*，計算屬性不直接儲存值，而是提供一個 getter 來獲取值，一個可選的 setter 來間接設置其他屬性或變數的值。

```swift
struct Point {
    var x = 0.0, y = 0.0
}
struct Size {
    var width = 0.0, height = 0.0
}
struct Rect {
    var origin = Point()
    var size = Size()
    var center: Point {
    get {
        let centerX = origin.x + (size.width / 2)
        let centerY = origin.y + (size.height / 2)
        return Point(x: centerX, y: centerY)
    }
    set(newCenter) {
        origin.x = newCenter.x - (size.width / 2)
        origin.y = newCenter.y - (size.height / 2)
    }
    }
}
var square = Rect(origin: Point(x: 0.0, y: 0.0),
    size: Size(width: 10.0, height: 10.0))
let initialSquareCenter = square.center
square.center = Point(x: 15.0, y: 15.0)
println("square.origin is now at (\(square.origin.x), \(square.origin.y))")
// 輸出 "square.origin is now at (10.0, 10.0)」
```

這個範例定義了 3 個幾何形狀的結構：

- `Point`封裝了一個`(x, y)`的坐標
- `Size`封裝了一個`width`和`height`
- `Rect`表示一個有原點和尺寸的矩形

`Rect`也提供了一個名為`center`的計算屬性。一個矩形的中心點可以從原點和尺寸來算出，所以不需要將它以顯式宣告的`Point`來保存。`Rect`的計算屬性`center`提供了自定義的 getter 和 setter 來獲取和設置矩形的中心點，就像它有一個儲存屬性一樣。

範例中接下來創建了一個名為`square`的`Rect`實例，初始值原點是`(0, 0)`，寬度高度都是`10`。如圖所示藍色正方形。

`square`的`center`屬性可以通過點運算子（`square.center`）來存取，這會呼叫 getter 來獲取屬性的值。跟直接回傳已經存在的值不同，getter 實際上通過計算然後回傳一個新的`Point`來表示`square`的中心點。如程式碼所示，它正確回傳了中心點`(5, 5)`。

`center`屬性之後被設置了一個新的值`(15, 15)`，表示向右上方移動正方形到如圖所示橙色正方形的位置。設置屬性`center`的值會呼叫 setter 來修改屬性`origin`的`x`和`y`的值，從而實作移動正方形到新的位置。

<img src="https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Art/computedProperties_2x.png" alt="Computed Properties sample" width="388" height="387" />

<a name="shorthand_setter_declaration"></a>
### 便捷 setter 宣告

如果計算屬性的 setter 沒有定義表示新值的參數名，則可以使用預設名稱`newValue`。下面是使用了便捷 setter 宣告的`Rect`結構程式碼：

```swift
struct AlternativeRect {
    var origin = Point()
    var size = Size()
    var center: Point {
    get {
        let centerX = origin.x + (size.width / 2)
        let centerY = origin.y + (size.height / 2)
        return Point(x: centerX, y: centerY)
    }
    set {
        origin.x = newValue.x - (size.width / 2)
        origin.y = newValue.y - (size.height / 2)
    }
    }
}
```

<a name="readonly_computed_properties"></a>
### 唯讀計算屬性

只有 getter 沒有 setter 的計算屬性就是*唯讀計算屬性*。唯讀計算屬性總是回傳一個值，可以通過點運算子存取，但不能設置新的值。

<<<<<<< HEAD
> 注意：  
> 必須使用`var`關鍵字定義計算屬性，包括唯讀計算屬性，因為他們的值不是固定的。`let`關鍵字只用來宣告常數屬性，表示初始化後再也無法修改的值。  
=======
> 注意：
>
> 必須使用`var`關鍵字定義計算屬性，包括唯讀計算屬性，因為它們的值不是固定的。`let`關鍵字只用來宣告常數屬性，表示初始化後再也無法修改的值。
>>>>>>> a516af6a531a104ec88da0d236ecf389a5ec72af

唯讀計算屬性的宣告可以去掉`get`關鍵字和花括號：

```swift
struct Cuboid {
    var width = 0.0, height = 0.0, depth = 0.0
    var volume: Double {
    return width * height * depth
    }
}
let fourByFiveByTwo = Cuboid(width: 4.0, height: 5.0, depth: 2.0)
println("the volume of fourByFiveByTwo is \(fourByFiveByTwo.volume)")
// 輸出 "the volume of fourByFiveByTwo is 40.0"
```

這個範例定義了一個名為`Cuboid`的結構，表示三維空間的立方體，包含`width`、`height`和`depth`屬性，還有一個名為`volume`的唯讀計算屬性用來回傳立方體的體積。設置`volume`的值毫無意義，因為通過`width`、`height`和`depth`就能算出`volume`。然而，`Cuboid`提供一個唯讀計算屬性來讓外部使用者直接獲取體積是很有用的。

<a name="property_observers"></a>
## 屬性監視器

*屬性監視器*監控和響應屬性值的變化，每次屬性被設置值的時候都會呼叫屬性監視器，甚至新的值和現在的值相同的時候也不例外。

可以為除了延遲儲存屬性之外的其他儲存屬性添加屬性監視器，也可以通過重載屬性的方式為繼承的屬性（包括儲存屬性和計算屬性）添加屬性監視器。屬性重載請參考[繼承](chapter/13_Inheritance.html)一章的[重載](chapter/13_Inheritance.html#overriding)。

> 注意：  
> 不需要為無法重載的計算屬性添加屬性監視器，因為可以通過 setter 直接監控和響應值的變化。  

可以為屬性添加如下的一個或全部監視器：

- `willSet`在設置新的值之前呼叫
- `didSet`在新的值被設置之後立即呼叫

`willSet`監視器會將新的屬性值作為固定參數傳入，在`willSet`的實作程式碼中可以為這個參數指定一個名稱，如果不指定則參數仍然可用，這時使用預設名稱`newValue`表示。

類似地，`didSet`監視器會將舊的屬性值作為參數傳入，可以為該參數命名或者使用預設參數名`oldValue`。

<<<<<<< HEAD
> 注意：  
> `willSet`和`didSet`監視器在屬性初始化過程中不會被呼叫，他們只會當屬性的值在初始化之外的地方被設置時被呼叫。  
=======
> 注意：
>
> `willSet`和`didSet`監視器在屬性初始化過程中不會被呼叫，它們只會當屬性的值在初始化之外的地方被設置時被呼叫。
>>>>>>> a516af6a531a104ec88da0d236ecf389a5ec72af

這裡是一個`willSet`和`didSet`的實際範例，其中定義了一個名為`StepCounter`的類別，用來統計當人步行時的總步數，可以跟計步器或其他日常鍛煉的統計裝置的輸入資料配合使用。

```swift
class StepCounter {
    var totalSteps: Int = 0 {
    willSet(newTotalSteps) {
        println("About to set totalSteps to \(newTotalSteps)")
    }
    didSet {
        if totalSteps > oldValue  {
            println("Added \(totalSteps - oldValue) steps")
        }
    }
    }
}
let stepCounter = StepCounter()
stepCounter.totalSteps = 200
// About to set totalSteps to 200
// Added 200 steps
stepCounter.totalSteps = 360
// About to set totalSteps to 360
// Added 160 steps
stepCounter.totalSteps = 896
// About to set totalSteps to 896
// Added 536 steps
```

`StepCounter`類別定義了一個`Int`型別的屬性`totalSteps`，它是一個儲存屬性，包含`willSet`和`didSet`監視器。

當`totalSteps`設置新值的時候，它的`willSet`和`didSet`監視器都會被呼叫，甚至當新的值和現在的值完全相同也會呼叫。

範例中的`willSet`監視器將表示新值的參數自定義為`newTotalSteps`，這個監視器只是簡單的將新的值輸出。

`didSet`監視器在`totalSteps`的值改變後被呼叫，它把新的值和舊的值進行對比，如果總的步數增加了，就輸出一個訊息表示增加了多少步。`didSet`沒有提供自定義名稱，所以預設值`oldValue`表示舊值的參數名。

> 注意：  
> 如果在`didSet`監視器裡為屬性賦值，這個值會替換監視器之前設置的值。  

<a name="global_and_local_variables"></a>
##全域變數和局部變數

計算屬性和屬性監視器所描述的模式也可以用於*全域變數*和*局部變數*，全域變數是在函式、方法、閉包或任何型別之外定義的變數，局部變數是在函式、方法或閉包內部定義的變數。

前面章節提到的全域或局部變數都屬於儲存型變數，跟儲存屬性類似，它提供特定型別的儲存空間，並允許讀取和寫入。

另外，在全域或局部範圍都可以定義計算型變數和為儲存型變數定義監視器，計算型變數跟計算屬性一樣，回傳一個計算的值而不是儲存值，宣告格式也完全一樣。

> 注意：  
> 全域的常數或變數都是延遲計算的，跟[延遲儲存屬性](#lazy_stored_properties)相似，不同的地方在於，全域的常數或變數不需要標記`@lazy`特性。  
> 局部範圍的常數或變數不會延遲計算。  

<a name="type_properties"></a>
##型別屬性

實例的屬性屬於一個特定型別實例，每次型別實例化後都擁有自己的一套屬性值，實例之間的屬性相互獨立。

也可以為型別本身定義屬性，不管型別有多少個實例，這些屬性都只有唯一一份。這種屬性就是*型別屬性*。

型別屬性用於定義特定型別所有實例共享的資料，比如所有實例都能用的一個常數（就像 C 語言中的靜態常數），或者所有實例都能存取的一個變數（就像 C 語言中的靜態變數）。

對於值型別（指結構和列舉）可以定義儲存型和計算型型別屬性，對於類別（class）則只能定義計算型型別屬性。

值型別的儲存型型別屬性可以是變數或常數，計算型型別屬性跟實例的計算屬性一樣定義成變數屬性。

> 注意：  
> 跟實例的儲存屬性不同，必須給儲存型型別屬性指定預設值，因為型別本身無法在初始化過程中使用建構器給型別屬性賦值。  

<a name="type_property_syntax"></a>
###型別屬性語法

在 C 或 Objective-C 中，靜態常數和靜態變數的定義是通過特定型別加上`global`關鍵字。在 Swift 程式語言中，型別屬性是作為型別定義的一部分寫在型別最外層的花括號內，因此它的作用範圍也就在型別支援的範圍內。

使用關鍵字`static`來定義值型別的型別屬性，關鍵字`class`來為類別（class）定義型別屬性。下面的範例演示了儲存型和計算型型別屬性的語法：

```swift
struct SomeStructure {
    static var storedTypeProperty = "Some value."
    static var computedTypeProperty: Int {
    // 這裡回傳一個 Int 值
    }
}
enum SomeEnumeration {
    static var storedTypeProperty = "Some value."
    static var computedTypeProperty: Int {
    // 這裡回傳一個 Int 值
    }
}
class SomeClass {
    class var computedTypeProperty: Int {
    // 這裡回傳一個 Int 值
    }
}
```

> 注意：  
> 範例中的計算型型別屬性是唯讀的，但也可以定義可讀可寫的計算型型別屬性，跟實例計算屬性的語法類似。  

<a name="querying_and_setting_type_properties"></a>
###獲取和設置型別屬性的值

跟實例的屬性一樣，型別屬性的存取也是通過點運算子來進行，但是，型別屬性是通過型別本身來獲取和設置，而不是通過實例。比如：

```swift
println(SomeClass.computedTypeProperty)
// 輸出 "42"

println(SomeStructure.storedTypeProperty)
// 輸出 "Some value."
SomeStructure.storedTypeProperty = "Another value."
println(SomeStructure.storedTypeProperty)
// 輸出 "Another value.」
```

下面的範例定義了一個結構，使用兩個儲存型型別屬性來表示多個聲道的聲音電平值，每個聲道有一個 0 到 10 之間的整數表示聲音電平值。

後面的圖表展示了如何聯合使用兩個聲道來表示一個立體聲的聲音電平值。當聲道的電平值是 0，沒有一個燈會亮；當聲道的電平值是 10，所有燈點亮。本圖中，左聲道的電平是 9，右聲道的電平是 7。

<img src="https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Art/staticPropertiesVUMeter_2x.png" alt="Static Properties VUMeter" width="243" height="357" />

上面所描述的聲道模型使用`AudioChannel`結構來表示：

```swift
struct AudioChannel {
    static let thresholdLevel = 10
    static var maxInputLevelForAllChannels = 0
    var currentLevel: Int = 0 {
    didSet {
        if currentLevel > AudioChannel.thresholdLevel {
            // 將新電平值設置為閥值
            currentLevel = AudioChannel.thresholdLevel
        }
        if currentLevel > AudioChannel.maxInputLevelForAllChannels {
            // 儲存當前電平值作為新的最大輸入電平
            AudioChannel.maxInputLevelForAllChannels = currentLevel
        }
    }
    }
}
```

結構`AudioChannel`定義了 2 個儲存型型別屬性來實作上述功能。第一個是`thresholdLevel`，表示聲音電平的最大上限閾值，它是一個取值為 10 的常數，對所有實例都可見，如果聲音電平高於 10，則取最大上限值 10（見後面描述）。

第二個型別屬性是變數儲存型屬性`maxInputLevelForAllChannels`，它用來表示所有`AudioChannel`實例的電平值的最大值，初始值是 0。

`AudioChannel`也定義了一個名為`currentLevel`的實例儲存屬性，表示當前聲道現在的電平值，取值為 0 到 10。

屬性`currentLevel`包含`didSet`屬性監視器來檢查每次新設置後的屬性值，有如下兩個檢查：

- 如果`currentLevel`的新值大於允許的閾值`thresholdLevel`，屬性監視器將`currentLevel`的值限定為閾值`thresholdLevel`。
- 如果修正後的`currentLevel`值大於任何之前任意`AudioChannel`實例中的值，屬性監視器將新值保存在靜態屬性`maxInputLevelForAllChannels`中。

> 注意：  
> 在第一個檢查過程中，`didSet`屬性監視器將`currentLevel`設置成了不同的值，但這時不會再次呼叫屬性監視器。  

可以使用結構`AudioChannel`來創建表示立體聲系統的兩個聲道`leftChannel`和`rightChannel`：

```swift
var leftChannel = AudioChannel()
var rightChannel = AudioChannel()
```

如果將左聲道的電平設置成 7，型別屬性`maxInputLevelForAllChannels`也會更新成 7：

```swift
leftChannel.currentLevel = 7
println(leftChannel.currentLevel)
// 輸出 "7"
println(AudioChannel.maxInputLevelForAllChannels)
// 輸出 "7"
```

如果試圖將右聲道的電平設置成 11，則會將右聲道的`currentLevel`修正到最大值 10，同時`maxInputLevelForAllChannels`的值也會更新到 10：

```swift
rightChannel.currentLevel = 11
println(rightChannel.currentLevel)
// 輸出 "10"
println(AudioChannel.maxInputLevelForAllChannels)
// 輸出 "10"
```
