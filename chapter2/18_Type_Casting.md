> 翻譯：[xiehurricane](https://github.com/xiehurricane)
> 校對：[happyming](https://github.com/happyming)

# 型別檢查（Type Casting）
-----------------

本頁包含內容：

- [定義一個類別層次作為範例](#defining_a_class_hierarchy_for_type_casting)
- [檢查型別](#checking_type)
- [向下轉型（Downcasting）](#downcasting)
- [`Any`和`AnyObject`的型別檢查](#type_casting_for_any_and_anyobject)


_型別檢查_是一種檢查類別實例的方式，並且或者也是讓實例作為它的父類別或者子類別的一種方式。

型別檢查在 Swift 中使用`is` 和 `as`操作符實作。這兩個操作符提供了一種簡單達意的方式去檢查值的型別或者轉換它的型別。

你也可以用來檢查一個類別是否實作了某個協定，就像在 [Checking for Protocol Conformance](Protocols.html#//apple_ref/doc/uid/TP40014097-CH25-XID_363)部分講述的一樣。

<a name="defining_a_class_hierarchy_for_type_casting"></a>
## 定義一個類別層次作為範例

你可以將它用在類別和子類別的層次結構上，檢查特定類別實例的型別並且轉換這個類別實例的型別成為這個層次結構中的其他型別。這下面的三個程式碼段定義了一個類別層次和一個包含了幾個這些類別實例的陣列，作為型別檢查的範例。

第一個程式碼片段定義了一個新的基礎類別`MediaItem`。這個類別為任何出現在數字媒體函式庫的媒體項提供基礎功能。特別的，它宣告了一個 `String` 型別的 `name` 屬性，和一個`init name`初始化器。（它假定所有的媒體項都有個名稱。）

```swift
class MediaItem {
    var name: String
    init(name: String) {
        self.name = name
    }
}
```

下一個程式碼段定義了 `MediaItem` 的兩個子類別。第一個子類別`Movie`，在父類別（或者說基類別）的基礎上增加了一個 `director`（導演） 屬性，和相應的初始化器。第二個類別在父類別的基礎上增加了一個 `artist`（藝術家） 屬性，和相應的初始化器：

```swift
class Movie: MediaItem {
    var director: String
    init(name: String, director: String) {
        self.director = director
        super.init(name: name)
    }
}

class Song: MediaItem {
    var artist: String
    init(name: String, artist: String) {
        self.artist = artist
        super.init(name: name)
    }
}
```

最後一個程式碼段創建了一個陣列常數 `library`，包含兩個`Movie`實例和三個`Song`實例。`library`的型別是在它被初始化時根據它陣列中所包含的內容推斷來的。Swift 的型別檢測器能夠演繹出`Movie` 和 `Song` 有共同的父類別 `MediaItem` ，所以它推斷出 `MediaItem[]` 類別作為 `library` 的型別。

```swift
let library = [
    Movie(name: "Casablanca", director: "Michael Curtiz"),
    Song(name: "Blue Suede Shoes", artist: "Elvis Presley"),
    Movie(name: "Citizen Kane", director: "Orson Welles"),
    Song(name: "The One And Only", artist: "Chesney Hawkes"),
    Song(name: "Never Gonna Give You Up", artist: "Rick Astley")
]
// the type of "library" is inferred to be MediaItem[]
```

在幕後`library` 裡儲存的媒體項依然是 `Movie` 和 `Song` 型別的，但是，若你迭代它，取出的實例會是 `MediaItem` 型別的，而不是 `Movie` 和 `Song` 型別的。為了讓它們作為它們本來的型別工作，你需要檢查它們的型別或者向下轉換它們的型別到其它型別，就像下面描述的一樣。

<a name="checking_type"></a>
## 檢查型別（Checking Type）

用型別檢查操作符(`is`)來檢查一個實例是否屬於特定子型別。若實例屬於那個子型別，型別檢查操作符回傳 `true` ，否則回傳 `false` 。

下面的範例定義了兩個變數，`movieCount` 和 `songCount`，用來計算陣列`library` 中 `Movie` 和 `Song` 型別的實例數量。

```swift
var movieCount = 0
var songCount = 0

for item in library {
    if item is Movie {
        ++movieCount
    } else if item is Song {
        ++songCount
    }
}

println("Media library contains \(movieCount) movies and \(songCount) songs")
// prints "Media library contains 2 movies and 3 songs"
```

示例迭代了陣列 `library` 中的所有項。每一次， `for`-`in` 迴圈設置
`item` 為陣列中的下一個 `MediaItem`。

若當前 `MediaItem` 是一個 `Movie` 型別的實例， `item is Movie` 回傳
`true`，相反回傳 `false`。同樣的，`item is
Song`檢查item是否為`Song`型別的實例。在迴圈結束後，`movieCount` 和 `songCount`的值就是被找到屬於各自的型別的實例數量。

<a name="downcasting"></a>
## 向下轉型（Downcasting）

某型別的一個常數或變數可能在幕後實際上屬於一個子類別。你可以相信，上面就是這種情況。你可以嘗試向下轉到它的子型別，用型別檢查操作符(`as`)

因為向下轉型可能會失敗，型別轉型操作符帶有兩種不同形式。可選形式（ optional form） `as?` 回傳一個你試圖下轉成的型別的可選值（optional value）。強制形式 `as` 把試圖向下轉型和強制解包（force-unwraps）結果作為一個混合動作。

當你不確定向下轉型可以成功時，用型別檢查的可選形式(`as?`)。可選形式的型別檢查總是回傳一個可選值（optional value），並且若下轉是不可能的，可選值將是 `nil` 。這使你能夠檢查向下轉型是否成功。

只有你可以確定向下轉型一定會成功時，才使用強制形式。當你試圖向下轉型為一個不正確的型別時，強制形式的型別檢查會觸發一個執行時錯誤。

下面的範例，迭代了`library`裡的每一個 `MediaItem` ，並列印出適當的描述。要這樣做，`item`需要真正作為`Movie` 或 `Song`的型別來使用。不僅僅是作為 `MediaItem`。為了能夠使用`Movie` 或 `Song`的 `director` 或 `artist`屬性，這是必要的。

在這個示例中，陣列中的每一個`item`可能是 `Movie` 或 `Song`。   事前你不知道每個`item`的真實型別，所以這裡使用可選形式的型別檢查 （`as?`）去檢查迴圈裡的每次下轉。

```swift
for item in library {
    if let movie = item as? Movie {
        println("Movie: '\(movie.name)', dir. \(movie.director)")
    } else if let song = item as? Song {
        println("Song: '\(song.name)', by \(song.artist)")
    }
}

// Movie: 'Casablanca', dir. Michael Curtiz
// Song: 'Blue Suede Shoes', by Elvis Presley
// Movie: 'Citizen Kane', dir. Orson Welles
// Song: 'The One And Only', by Chesney Hawkes
// Song: 'Never Gonna Give You Up', by Rick Astley
```

示例首先試圖將 `item` 下轉為 `Movie`。因為 `item` 是一個 `MediaItem`
型別的實例，它可能是一個`Movie`；同樣，它可能是一個 `Song`，或者僅僅是基類別
`MediaItem`。因為不確定，`as?`形式在試圖下轉時將返還一個可選值。 `item as Movie` 的回傳值是`Movie?`型別或 “optional `Movie`”。

當向下轉型為 `Movie` 應用在兩個 `Song`
實例時將會失敗。為了處理這種情況，上面的範例使用了可選綁定（optional binding）來檢查可選 `Movie`真的包含一個值（這個是為了判斷下轉是否成功。）可選綁定是這樣寫的“`if let movie = item as? Movie`”，可以這樣解讀：

“嘗試將 `item` 轉為 `Movie`型別。若成功，設置一個新的臨時常數 `movie`  來儲存回傳的可選`Movie`”

若向下轉型成功，然後`movie`的屬性將用於列印一個`Movie`實例的描述，包括它的導演的名字`director`。當`Song`被找到時，一個相近的原理被用來檢測 `Song` 實例和列印它的描述。

> 注意：  
轉換沒有真的改變實例或它的值。潛在的根本的實例保持不變；只是簡單地把它作為它被轉換成的類別來使用。

<a name="type_casting_for_any_and_anyobject"></a>
## `Any`和`AnyObject`的型別檢查

Swift為不確定型別提供了兩種特殊型別別名：

* `AnyObject`可以代表任何class型別的實例。
* `Any`可以表示任何型別，除了方法型別（function types）。

> 注意：  
只有當你明確的需要它的行為和功能時才使用`Any`和`AnyObject`。在你的程式碼裡使用你期望的明確的型別總是更好的。

### `AnyObject`型別

當需要在工作中使用 Cocoa
APIs，它一般接收一個`AnyObject[]`型別的陣列，或者說“一個任何物件型別的陣列”。這是因為 Objective-C 沒有明確的型別化陣列。但是，你常常可以確定包含在僅從你知道的 API 資訊提供的這樣一個陣列中的物件的型別。

在這些情況下，你可以使用強制形式的型別檢查(`as`)來下轉在陣列中的每一項到比 `AnyObject` 更明確的型別，不需要可選解析（optional unwrapping）。

下面的示例定義了一個 `AnyObject[]` 型別的陣列並填入三個`Movie`型別的實例：

```swift
let someObjects: AnyObject[] = [
    Movie(name: "2001: A Space Odyssey", director: "Stanley Kubrick"),
    Movie(name: "Moon", director: "Duncan Jones"),
    Movie(name: "Alien", director: "Ridley Scott")
]
```

因為知道這個陣列只包含 `Movie` 實例，你可以直接用(`as`)下轉並解包到不可選的`Movie`型別（ps：其實就是我們常用的正常型別，這裡是為了和可選型別相對比）。

```swift
for object in someObjects {
    let movie = object as Movie
    println("Movie: '\(movie.name)', dir. \(movie.director)")
}
// Movie: '2001: A Space Odyssey', dir. Stanley Kubrick
// Movie: 'Moon', dir. Duncan Jones
// Movie: 'Alien', dir. Ridley Scott
```

為了變為一個更短的形式，下轉`someObjects`陣列為`Movie[]`型別來代替下轉每一項方式。

```swift
for movie in someObjects as Movie[] {
    println("Movie: '\(movie.name)', dir. \(movie.director)")
}
// Movie: '2001: A Space Odyssey', dir. Stanley Kubrick
// Movie: 'Moon', dir. Duncan Jones
// Movie: 'Alien', dir. Ridley Scott
```

### `Any`型別

這裡有個示例，使用 `Any` 型別來和混合的不同型別一起工作，包括非`class`型別。它創建了一個可以儲存`Any`型別的陣列 `things`。

```swift
var things = Any[]()

things.append(0)
things.append(0.0)
things.append(42)
things.append(3.14159)
things.append("hello")
things.append((3.0, 5.0))
things.append(Movie(name: "Ghostbusters", director: "Ivan Reitman"))
```

`things` 陣列包含兩個 `Int` 值，2個 `Double` 值，1個 `String` 值，一個元組 `(Double, Double)` ，Ivan Reitman 導演的電影“Ghostbusters”。

你可以在 `switch` `cases`裡用`is` 和 `as` 操作符來發覺只知道是 `Any` 或 `AnyObject`的常數或變數的型別。 下面的示例迭代 `things`陣列中的每一項的並用`switch`語句查找每一項的型別。這幾種`switch`語句的情形綁定它們匹配的值到一個規定型別的常數，讓它們可以列印它們的值：

```swift
for thing in things {
    switch thing {
    case 0 as Int:
        println("zero as an Int")
    case 0 as Double:
        println("zero as a Double")
    case let someInt as Int:
        println("an integer value of \(someInt)")
    case let someDouble as Double where someDouble > 0:
        println("a positive double value of \(someDouble)")
    case is Double:
        println("some other double value that I don't want to print")
    case let someString as String:
        println("a string value of \"\(someString)\"")
    case let (x, y) as (Double, Double):
        println("an (x, y) point at \(x), \(y)")
    case let movie as Movie:
        println("a movie called '\(movie.name)', dir. \(movie.director)")
    default:
        println("something else")
    }
}

// zero as an Int
// zero as a Double
// an integer value of 42
// a positive double value of 3.14159
// a string value of "hello"
// an (x, y) point at 3.0, 5.0
// a movie called 'Ghostbusters', dir. Ivan Reitman
```


> 注意：  
在一個switch語句的case中使用強制形式的型別檢查操作符（as, 而不是 as?）來檢查和轉換到一個明確的型別。在 switch case 語句的內容中這種檢查總是安全的。