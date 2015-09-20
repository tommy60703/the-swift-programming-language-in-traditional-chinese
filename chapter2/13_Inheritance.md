> 1.0 翻譯：[Hawstein](https://github.com/Hawstein) 校對：[menlongsheng](https://github.com/menlongsheng)

> 2.0 翻譯：[shanksyang](https://github.com/shanksyang) 校對：[shanksyang](https://github.com/shanksyang), [youweit](https://github.com/youweit)

# 繼承（Inheritance）
-------------------

本頁包含內容：

- [定義一個基類別（Base class）](#defining_a_base_class)
- [子類別生成（Subclassing）](#subclassing)
- [覆寫（Overriding）](#overriding)
- [防止覆寫](#preventing_overrides)

一個類別可以*繼承（inherit）*另一個類別的方法（methods），屬性（property）和其它特性。當一個類別繼承其它類別時，繼承類別叫*子類別（subclass）*，被繼承類別叫*超類別（或父類別，superclass）*。在 Swift 中，繼承是區分「類別」與其它型別的一個基本特征。

在 Swift 中，類別可以呼叫和存取超類別的方法，屬性和下標腳本（subscripts），並且可以覆寫（override）這些方法，屬性和下標腳本來優化或修改它們的行為。Swift 會檢查你的覆寫定義在超類別中是否有匹配的定義，以此確保你的覆寫行為是正確的。

可以為類別中繼承來的屬性添加屬性觀察器（property observer），這樣一來，當屬性值改變時，類別就會被通知到。可以為任何屬性添加屬性觀察器，無論它原本被定義為儲存型屬性（stored property）還是計算型屬性（computed property）。

<a name="defining_a_base_class"></a>
## 定義一個基類別（Base class）

不繼承於其它類別的類別，稱之為*基類別（base calss）*。

> 注意：  
Swift 中的類別並不是從一個通用的基類別繼承而來。如果你不為你定義的類別指定一個超類別的話，這個類別就自動成為基類別。

下面的例子定義了一個叫`Vehicle`的基類。這個基類聲明了一個名為`currentSpeed`，預設值是0.0的存儲屬性(屬性類型推斷為`Double`)。`currentSpeed`屬性的值被一個`String`類型的只讀計算型屬性`description`使用，用來創建車輛的描述。

`Vehicle`基類也定義了一個名為`makeNoise`的方法。這個方法實際上不為`Vehicle`實例做任何事，但之後將會由`Vehicle`的子類實作：

```swift
class Vehicle {
    var currentSpeed = 0.0
    var description: String {
        return "traveling at \(currentSpeed) miles per hour"
    }
    func makeNoise() {
        // 什麼也不做，因為車子不一定會有噪音
    }
}
```
您可以用初始化語法創建一個`Vehicle`的新實例，即類名後面跟一個空括號：

```swift
let someVehicle = Vehicle()
```

現在已經創建了一個Vehicle的新實例，你可以訪問它的description屬性來打印車輛的當前速度。

```swift
print("Vehicle: \(someVehicle.description)")
// Vehicle: traveling at 0.0 miles per hour
```
`Vehicle`類定義了一個通用特性的車輛類，實際上沒什麼用處。為了讓它變得更加有用，需要改進它能夠描述一個更加具體的車輛類。

<a name="subclassing"></a>
## 子類別生成（Subclassing）

*子類別生成（Subclassing）*指的是在一個已有類別的基礎上創建一個新的類別。子類別繼承超類別的特性，並且可以優化或改變它。你還可以為子類別添加新的特性。

為了指明某個類別的超類別，將超類別名寫在子類別名的後面，用冒號分隔：

```swift
class SomeClass: SomeSuperclass {
    // 類別的定義
}
```

下一個範例，定義一個更具體的車輛類別叫`Bicycle`。

```swift
class Bicycle: Vehicle {
    var hasBasket = false
}
```
新的`Bicycle`類自動獲得`Vehicle`類的所有特性，比如 `currentSpeed`和`description`屬性，還有它的`makeNoise`方法。

除了它所繼承的特性，`Bicycle`類還定義了一個默認值為`false`的存儲型屬性`hasBasket`（屬性推斷為`Bool`）。

默認情況下，你創建任何新的`Bicycle`實例將不會有一個籃子，創建該實例之後，你可以為特定的`Bicycle`實例設置`hasBasket`屬性為`ture`：

```swift
let bicycle = Bicycle()
bicycle.hasBasket = true
```
你還可以修改`Bicycle`實例所繼承的`currentSpeed`屬性，和查詢實例所繼承的`description`屬性：

```swift
bicycle.currentSpeed = 15.0
print("Bicycle: \(bicycle.description)")
// Bicycle: traveling at 15.0 miles per hour
```
子類還可以繼續被其它類繼承，下面的示例為`Bicycle`創建了一個名為`Tandem`（雙人自行車）的子類：
```swift
class Tandem: Bicycle {
    var currentNumberOfPassengers = 0
}
```
`Tandem`從`Bicycle`繼承了所有的屬性與方法，這又使它同時繼承了`Vehicle`的所有屬性與方法。`Tandem`也增加了一個新的叫做`currentNumberOfPassengers`的存儲型屬性，預設值為0。

如果你創建了一個`Tandem`的實例，你可以使用它所有的新屬性和繼承的屬性，還能查詢從`Vehicle`繼承來的唯讀屬性`description`：

```swift
let tandem = Tandem()
tandem.hasBasket = true
tandem.currentNumberOfPassengers = 2
tandem.currentSpeed = 22.0
print("Tandem: \(tandem.description)")
// Tandem: traveling at 22.0 miles per hour
```
<a name="overriding"></a>
## 覆寫（Overriding）

子類別可以為繼承來的實例方法（instance method），類別方法（class method），實例屬性（instance property），或下標腳本（subscript）提供自己定制的實作（implementation）。我們把這種行為叫*覆寫（overriding）*。

如果要覆寫某個特性，你需要在覆寫定義的前面加上`override`關鍵字。這麼做，你就表明了你是想提供一個覆寫版本，而非錯誤地提供了一個相同的定義。意外的覆寫行為可能會導致不可預知的錯誤，任何缺少`override`關鍵字的覆寫都會在編譯時被診斷為錯誤。

`override`關鍵字會提醒 Swift 編譯器去檢查該類別的超類別（或其中一個父類別）是否有匹配覆寫版本的宣告。這個檢查可以確保你的覆寫定義是正確的。

### 存取超類別的方法，屬性及下標腳本

當你在子類別中覆寫超類別的方法，屬性或下標腳本時，有時在你的覆寫版本中使用已經存在的超類別實作會大有裨益。比如，你可以優化已有實作的行為，或在一個繼承來的變數中儲存一個修改過的值。

在合適的地方，你可以通過使用`super`前綴來存取超類別版本的方法，屬性或下標腳本：

* 在方法`someMethod`的覆寫實作中，可以通過`super.someMethod()`來呼叫超類別版本的`someMethod`方法。
* 在屬性`someProperty`的 getter 或 setter 的覆寫實作中，可以通過`super.someProperty`來存取超類別版本的`someProperty`屬性。
* 在下標腳本的覆寫實作中，可以通過`super[someIndex]`來存取超類別版本中的相同下標腳本。

### 覆寫方法

在子類別中，你可以覆寫繼承來的實例方法或類別方法，提供一個定制或替代的方法實作。

下面的例子定義了`Vehicle`的一個新的子類，叫`Train`，它覆寫了從`Vehicle`類繼承來的`makeNoise`方法：

```swift
class Train: Vehicle {
    override func makeNoise() {
        print("Choo Choo")
    }
}
```
如果你創建一個Train的新實例，並調用了它的`makeNoise`方法，你就會發現`Train`版本的方法被呼叫：

```swift
let train = Train()
train.makeNoise()
// prints "Choo Choo"
```
### 覆寫屬性

你可以覆寫繼承來的實例屬性或類別屬性，提供自己定制的getter和setter，或添加屬性觀察器使覆寫的屬性觀察屬性值什麼時候發生改變。

#### 覆寫屬性的Getters和Setters

你可以提供定制的 getter（或 setter）來覆寫任意繼承來的屬性，無論繼承來的屬性是儲存型的還是計算型的屬性。子類別並不知道繼承來的屬性是儲存型的還是計算型的，它只知道繼承來的屬性會有一個名字和型別。你在覆寫一個屬性時，必需將它的名字和型別都寫出來。這樣才能使編譯器去檢查你覆寫的屬性是與超類別中同名同型別的屬性相匹配的。

你可以將一個繼承來的唯讀屬性覆寫為一個讀寫屬性，只需要你在覆寫版本的屬性裡提供 getter 和 setter 即可。但是，你不可以將一個繼承來的讀寫屬性覆寫為一個唯讀屬性。

> 注意：
如果你在覆寫屬性中提供了 setter，那麼你也一定要提供 getter。如果你不想在覆寫版本中的 getter 裡修改繼承來的屬性值，你可以直接通過`super.someProperty`來返回繼承來的值，其中`someProperty`是你要覆寫的屬性的名字。

以下的例子定義了一個新類，叫`Car`，它是`Vehicle`的子類。這個類引入了一個新的存儲型屬性叫做gear，預設值為整數1。`Car`類覆寫了繼承自`Vehicle`的`description`屬性，提供自定義的，包含當前檔位的描述：

```swift
class Car: Vehicle {
    var gear = 1
    override var description: String {
        return super.description + " in gear \(gear)"
    }
}
```
覆寫的`description`屬性，首先要調用`super.description`返回`Vehicle`類的`description`屬性。之後，`Car`類版本的`description`在末尾增加了一些額外的文本來提供關於當前檔位的信息。

如果你創建了`Car`的實例並且設置了它的`gear`和`currentSpeed`屬性，你可以看到它的`description`返回了`Car`中定義的`description`：

```swift
let car = Car()
car.currentSpeed = 25.0
car.gear = 3
print("Car: \(car.description)")
// Car: traveling at 25.0 miles per hour in gear 3
```
####覆寫屬性觀察器（Property Observer）

你可以在屬性覆寫中為一個繼承來的屬性添加屬性觀察器。這樣一來，當繼承來的屬性值發生改變時，你就會被通知到，無論那個屬性原本是如何實現的。關於屬性觀察器的更多內容，請看[屬性觀察器]()。

> 注意：    
你不可以為繼承來的常量存儲型屬性或繼承來的只讀計算型屬性添加屬性觀察器。
這些屬性的值是不可以被設置的，所以，為它們提供`willSet`或`didSet`實現是不恰當。此外還要注意，你不可以同時提供覆寫的 setter 和覆寫的屬性觀察器。如果你想觀察屬性值的變化，並且你已經為那個屬性提供了定制的 setter，那麼你在 setter 中就可以觀察到任何值變化了。

下面的例子定義了一個新類叫`AutomaticCar`，它是`Car`的子類。`AutomaticCar`表示自動擋汽車，它可以根據當前的速度自動選擇合適的擋位:

```swift
class AutomaticCar: Car {
    override var currentSpeed: Double {
        didSet {
            gear = Int(currentSpeed / 10.0) + 1
        }
    }
}
```
當你設置`AutomaticCar`的`currentSpeed`屬性，屬性的`didSet`觀察器就會自動地設置`gear`屬性，為新的速度選擇一個合適的擋位。具體來說就是，屬性觀察器將新的速度值除以10，然後向下取得最接近的整數值，最後加1來得到檔位`gear`的值。例如，速度為10.0時，擋位為1；速度為35.0時，擋位為4：

```swift
let automatic = AutomaticCar()
automatic.currentSpeed = 35.0
print("AutomaticCar: \(automatic.description)")
// AutomaticCar: traveling at 35.0 miles per hour in gear 4
```

<a name="preventing_overrides"></a>
## 防止覆寫

你可以通過把方法，屬性或下標腳本標記為*`final`*來防止它們被覆寫，只需要在宣告關鍵字前加上`@final`特性即可。（例如：`@final var`, `@final func`, `@final class func`, 以及 `@final subscript`）

如果你覆寫了`final`方法，屬性或下標腳本，在編譯時會報錯。在擴展中，你添加到類別裡的方法，屬性或下標腳本也可以在擴展的定義裡標記為 final。

你可以通過在關鍵字`class`前添加`@final`特性（`@final class`）來將整個類別標記為 final 的，這樣的類別是不可被繼承的，否則會報編譯錯誤。

