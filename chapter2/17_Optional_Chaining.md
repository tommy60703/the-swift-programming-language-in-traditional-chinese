> 翻譯：[Jasonbroker](https://github.com/Jasonbroker)
> 校對：[numbbbbb](https://github.com/numbbbbb), [stanzhai](https://github.com/stanzhai)

# Optional Chaining
-----------------

本頁包含內容：

- [可選鏈可替代強制解析](#optional_chaining_as_an_alternative_to_forced_unwrapping)
- [為可選鏈定義模型類別](#defining_model_classes_for_optional_chaining)
- [通過可選鏈呼叫屬性](#calling_properties_through_optional_chaining)
- [通過可選鏈呼叫方法](#calling_methods_through_optional_chaining)
- [使用可選鏈呼叫子腳本](#calling_subscripts_through_optional_chaining)
- [連接多層鏈接](#linking_multiple_levels_of_chaining)
- [鏈接可選回傳值的方法](#chaining_on_methods_with_optional_return_values)

可選鏈（Optional Chaining）是一種可以請求和呼叫屬性、方法及子腳本的過程，它的可選性體現於請求或呼叫的目標當前可能為空（`nil`）。如果可選的目標有值，那麼呼叫就會成功；相反，如果選擇的目標為空（`nil`），則這種呼叫將回傳空（`nil`）。多次請求或呼叫可以被鏈接在一起形成一個鏈，如果任何一個節點為空（`nil`）將導致整個鏈失效。

> 注意：  
Swift 的可選鏈和 Objective-C 中的消息為空有些相像，但是 Swift 可以使用在任意型別中，並且失敗與否可以被檢測到。

<a name="optional_chaining_as_an_alternative_to_forced_unwrapping"></a>
## 可選鏈可替代強制解析

通過在想呼叫的屬性、方法、或子腳本的可選值（`optional value`）（非空）後面放一個問號，可以定義一個可選鏈。這一點很像在可選值後面放一個嘆號來強制拆得其封包內的值。它們的主要的區別在於當可選值為空時可選鏈即刻失敗，然而一般的強制解析將會引發執行時錯誤。

為了反映可選鏈可以呼叫空（`nil`），不論你呼叫的屬性、方法、子腳本等回傳的值是不是可選值，它的回傳結果都是一個可選值。你可以利用這個回傳值來檢測你的可選鏈是否呼叫成功，有回傳值即成功，回傳nil則失敗。

呼叫可選鏈的回傳結果與原本的回傳結果具有相同的型別，但是原本的回傳結果被包裝成了一個可選值，當可選鏈呼叫成功時，一個應該回傳`Int`的屬性將會回傳`Int?`。

下面幾段程式碼將解釋可選鏈和強制解析的不同。

首先定義兩個類別`Person`和`Residence`。

```swift
class Person {
    var residence: Residence?
}

class Residence {
    var numberOfRooms = 1
}
```

`Residence`具有一個`Int`型別的`numberOfRooms`，其值為 1。`Person`具有一個可選`residence`屬性，它的型別是`Residence？`。

如果你創建一個新的`Person`實例，它的`residence`屬性由於是被定義為可選型的，此屬性將預設初始化為空：

```swift
let john = Person()
```

如果你想使用感嘆號（`!`）強制解析獲得這個人`residence`屬性`numberOfRooms`屬性值，將會引發執行時錯誤，因為這時沒有可以供解析的`residence`值。

```swift
let roomCount = john.residence!.numberOfRooms
//將導致執行時錯誤
```
當`john.residence`不是`nil`時，會執行通過，且會將`roomCount` 設置為一個`int`型別的合理值。然而，如上所述，當`residence`為空時，這個程式碼將會導致執行時錯誤。

可選鏈提供了一種另一種獲得`numberOfRooms`的方法。利用可選鏈，使用問號來代替原來`!`的位置：

```swift
if let roomCount = john.residence?.numberOfRooms {
    println("John's residence has \(roomCount) room(s).")
} else {
	println("Unable to retrieve the number of rooms.")
}
// 列印 "Unable to retrieve the number of rooms.
```

這告訴 Swift 來鏈接可選`residence?`屬性，如果`residence`存在則取回`numberOfRooms`的值。

因為這種嘗試獲得`numberOfRooms`的操作有可能失敗，可選鏈會回傳`Int?`型別值，或者稱作“可選`Int`”。當`residence`是空的時候（上例），選擇`Int`將會為空，因此會出先無法存取`numberOfRooms`的情況。

要注意的是，即使numberOfRooms是非可選`Int`（`Int?`）時這一點也成立。只要是通過可選鏈的請求就意味著最後`numberOfRooms`總是回傳一個`Int?`而不是`Int`。

你可以自己定義一個`Residence`實例給`john.residence`，這樣它就不再為空了：

```swift
john.residence = Residence()
```

`john.residence` 現在有了實際存在的實例而不是nil了。如果你想使用和前面一樣的可選鏈來獲得`numberOfRoooms`，它將回傳一個包含預設值 1 的`Int?`：

```swift
if let roomCount = john.residence?.numberOfRooms {
    println("John's residence has \(roomCount) room(s).")
} else {
    println("Unable to retrieve the number of rooms.")
}
// 列印 "John's residence has 1 room(s)"。
```

<a name="defining_model_classes_for_optional_chaining"></a>
##為可選鏈定義模型類別

你可以使用可選鏈來多層呼叫屬性，方法，和子腳本。這讓你可以利用它們之間的複雜模型來獲取更底層的屬性，並檢查是否可以成功獲取此類別底層屬性。

後面的程式碼定義了四個將在後面使用的模型類別，其中包括多層可選鏈。這些類別是由上面的`Person`和`Residence`模型通過添加一個`Room`和一個`Address`類別拓展來。

`Person`類別定義與之前相同。

```swift
class Person {
    var residence: Residence?
}
```

`Residence`類別比之前複雜些。這次，它定義了一個變數 `rooms`，它被初始化為一個`Room[]`型別的空陣列：

```swift
class Residence {
    var rooms = Room[]()
    var numberOfRooms: Int {
    return rooms.count
    }
    subscript(i: Int) -> Room {
        return rooms[i]
    }
    func printNumberOfRooms() {
        println("The number of rooms is \(numberOfRooms)")
    }
    var address: Address?
}
```

因為`Residence`儲存了一個`Room`實例的陣列，它的`numberOfRooms`屬性值不是一個固定的儲存值，而是通過計算而來的。`numberOfRooms`屬性值是由回傳`rooms`陣列的`count`屬性值得到的。

為了能快速存取`rooms`陣列，`Residence`定義了一個只讀的子腳本，通過插入陣列的元素角標就可以成功呼叫。如果該角標存在，子腳本則將該元素回傳。

`Residence`中也提供了一個`printNumberOfRooms`的方法，即簡單的列印房間個數。

最後，`Residence`定義了一個可選屬性叫`address`（`address?`）。`Address`類別的屬性將在後面定義。
用於`rooms`陣列的`Room`類別是一個很簡單的類別，它只有一個`name`屬性和一個設定`room`名的初始化器。

```swift
class Room {
    let name: String
    init(name: String) { self.name = name }
}
```


這個模型中的最終類別叫做`Address`。它有三個型別是`String?`的可選屬性。前面兩個可選屬性`buildingName`和 `buildingNumber`作為地址的一部分，是定義某個建築物的兩種方式。第三個屬性`street`，用於命名地址的街道名：

```swift
class Address {
    var buildingName: String?
    var buildingNumber: String?
    var street: String?
    func buildingIdentifier() -> String? {
        if buildingName {
            return buildingName
        } else if buildingNumber {
            return buildingNumber
        } else {
            return nil
        }
    }
}
```

`Address`類別還提供了一個`buildingIdentifier`的方法，它的回傳值型別為`String?`。這個方法檢查`buildingName`和`buildingNumber`的屬性，如果`buildingName`有值則將其回傳，或者如果`buildingNumber`有值則將其回傳，再或如果沒有一個屬性有值，回傳空。

<a name="calling_properties_through_optional_chaining"></a>
##通過可選鏈呼叫屬性

正如上面“ [可選鏈可替代強制解析](#optional_chaining_as_an_alternative_to_forced_unwrapping)”中所述，你可以利用可選鏈的可選值獲取屬性，並且檢查屬性是否獲取成功。然而，你不能使用可選鏈為屬性賦值。

使用上述定義的類別來創建一個人實例，並再次嘗試後去它的`numberOfRooms`屬性：

```swift
let john = Person()
if let roomCount = john.residence?.numberOfRooms {
    println("John's residence has \(roomCount) room(s).")
} else {
    println("Unable to retrieve the number of rooms.")
}
// 列印 "Unable to retrieve the number of rooms。
```

由於`john.residence`是空，所以這個可選鏈和之前一樣失敗了，但是沒有執行時錯誤。

<a name="calling_methods_through_optional_chaining"></a>
##通過可選鏈呼叫方法

你可以使用可選鏈的來呼叫可選值的方法並檢查方法呼叫是否成功。即使這個方法沒有回傳值，你依然可以使用可選鏈來達成這一目的。

`Residence`的`printNumberOfRooms`方法會列印`numberOfRooms`的當前值。方法如下：

```swift
func printNumberOfRooms(){
	println(“The number of rooms is \(numberOfRooms)”)
}
```

這個方法沒有回傳值。但是，沒有回傳值型別的函式和方法有一個隱式的回傳值型別`Void`（參見Function Without Return Values）。

如果你利用可選鏈呼叫此方法，這個方法的回傳值型別將是`Void?`，而不是`Void`，因為當通過可選鏈呼叫方法時回傳值總是可選型別（optional type）。即使這個方法本身沒有定義回傳值，你也可以使用`if`語句來檢查是否能成功呼叫`printNumberOfRooms`方法：如果方法通過可選鏈呼叫成功，`printNumberOfRooms`的隱式回傳值將會是`Void`，如果沒有成功，將回傳`nil`：

```swift
if john.residence?.printNumberOfRooms() {
    println("It was possible to print the number of rooms.")
} else {
    println("It was not possible to print the number of rooms.")
}
// 列印 "It was not possible to print the number of rooms."。
```

<a name="calling_subscripts_through_optional_chaining"></a>
##使用可選鏈呼叫子腳本

你可以使用可選鏈來嘗試從子腳本獲取值並檢查子腳本的呼叫是否成功，然而，你不能通過可選鏈來設置子程式碼。

> 注意：  
當你使用可選鏈來獲取子腳本的時候，你應該將問號放在子腳本括號的前面而不是後面。可選鏈的問號一般直接跟在表達語句的後面。

下面這個範例用在`Residence`類別中定義的子腳本來獲取`john.residence`陣列中第一個房間的名字。因為`john.residence`現在是`nil`，子腳本的呼叫失敗了。

```swift
if let firstRoomName = john.residence?[0].name {
    println("The first room name is \(firstRoomName).")
} else {
    println("Unable to retrieve the first room name.")
}
// 列印 "Unable to retrieve the first room name."。
```

在子程式碼呼叫中可選鏈的問號直接跟在`john.residence`的後面，在子腳本括號的前面，因為`john.residence`是可選鏈試圖獲得的可選值。

如果你創建一個`Residence`實例給`john.residence`，且在他的`rooms`陣列中有一個或多個`Room`實例，那麼你可以使用可選鏈通過`Residence`子腳本來獲取在`rooms`陣列中的實例了：

```swift
let johnsHouse = Residence()
johnsHouse.rooms += Room(name: "Living Room")
johnsHouse.rooms += Room(name: "Kitchen")
john.residence = johnsHouse

if let firstRoomName = john.residence?[0].name {
    println("The first room name is \(firstRoomName).")
} else {
    println("Unable to retrieve the first room name.")
}
// 列印 "The first room name is Living Room."。
```

<a name="linking_multiple_levels_of_chaining"></a>
##連接多層鏈接

你可以將多層可選鏈連接在一起，可以掘取模型內更下層的屬性方法和子腳本。然而多層可選鏈不能再添加比已經回傳的可選值更多的層。
也就是說：

如果你試圖獲得的型別不是可選型別，由於使用了可選鏈它將變成可選型別。
如果你試圖獲得的型別已經是可選型別，由於可選鏈它也不會提高可選性。

因此：

如果你試圖通過可選鏈獲得`Int`值，不論使用了多少層鏈接回傳的總是`Int?`。
相似的，如果你試圖通過可選鏈獲得`Int?`值，不論使用了多少層鏈接回傳的總是`Int?`。

下面的範例試圖獲取`john`的`residence`屬性裡的`address`的`street`屬性。這裡使用了兩層可選鏈來聯系`residence`和`address`屬性，它們兩者都是可選型別：

```swift
if let johnsStreet = john.residence?.address?.street {
    println("John's street name is \(johnsStreet).")
} else {
    println("Unable to retrieve the address.")
}
// 列印 "Unable to retrieve the address.”。
```

`john.residence`的值現在包含一個`Residence`實例，然而`john.residence.address`現在是`nil`，因此`john.residence?.address?.street`呼叫失敗。

從上面的範例發現，你試圖獲得`street`屬性值。這個屬性的型別是`String?`。因此儘管在可選型別屬性前使用了兩層可選鏈，`john.residence?.address?.street`的回傳值型別也是`String?`。

如果你為`Address`設定一個實例來作為`john.residence.address`的值，並為`address`的`street`屬性設定一個實際值，你可以通過多層可選鏈來得到這個屬性值。

```swift
let johnsAddress = Address()
johnsAddress.buildingName = "The Larches"
johnsAddress.street = "Laurel Street"
john.residence!.address = johnsAddress
```

```swift
if let johnsStreet = john.residence?.address?.street {
    println("John's street name is \(johnsStreet).")
} else {
    println("Unable to retrieve the address.")
}
// 列印 "John's street name is Laurel Street."。
```

值得注意的是，“`!`”符號在給`john.residence.address`分配`address`實例時的使用。`john.residence`屬性是一個可選型別，因此你需要在它獲取`address`屬性之前使用`!`解析以獲得它的實際值。

<a name="chaining_on_methods_with_optional_return_values"></a>
##鏈接可選回傳值的方法

前面的範例解釋了如何通過可選鏈來獲得可選型別屬性值。你也可以通過可選鏈呼叫一個回傳可選型別值的方法並按需鏈接該方法的回傳值。

下面的範例通過可選鏈呼叫了`Address`類別中的`buildingIdentifier` 方法。這個方法的回傳值型別是`String?`。如上所述，這個方法在可選鏈呼叫後最終的回傳值型別依然是`String?`：

```swift
if let buildingIdentifier = john.residence?.address?.buildingIdentifier() {
    println("John's building identifier is \(buildingIdentifier).")
}
// 列印 "John's building identifier is The Larches."。
```

如果你還想進一步對方法回傳值執行可選鏈，將可選鏈問號符放在方法括號的後面：

```swift
if let upper = john.residence?.address?.buildingIdentifier()?.uppercaseString {
    println("John's uppercase building identifier is \(upper).")
}
// 列印 "John's uppercase building identifier is THE LARCHES."。
```

> 注意：  
在上面的範例中，你將可選鏈問號符放在括號後面是因為你想要鏈接的可選值是`buildingIdentifier`方法的回傳值，不是`buildingIdentifier`方法本身。
