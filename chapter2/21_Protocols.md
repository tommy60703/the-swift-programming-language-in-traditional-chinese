> 翻譯：[geek5nan](https://github.com/geek5nan)
> 校對：[dabing1022](https://github.com/dabing1022)

# 協議
-----------------

本頁包含內容：

- [協議的語法（Protocol Syntax）](#protocol_syntax)
- [屬性要求（Property Requirements）](#property_requirements)
- [方法要求（Method Requirements）](#method_requirements)
- [突變方法要求（Mutating Method Requirements）](#mutating_method_requirements)
- [協議類型（Protocols as Types）](#protocols_as_types)
- [委托(代理)模式（Delegation）](#delegation)
- [在擴展中添加協議成員（Adding Protocol Conformance with an Extension）](#adding_protocol_conformance_with_an_extension)
- [通過擴展補充協議聲明（Declaring Protocol Adoption with an Extension）](#declaring_protocol_adoption_with_an_extension)
- [集合中的協議類型（Collections of Protocol Types）](#collections_of_protocol_types)
- [協議的繼承（Protocol Inheritance）](#protocol_inheritance)
- [協議合成（Protocol Composition）](#protocol_composition)
- [檢驗協議的一致性（Checking for Protocol Conformance）](#checking_for_protocol_conformance)
- [可選協議要求（Optional Protocol Requirements）](#optional_protocol_requirements)

`Protocol(協議)`用於**統一**方法和屬性的名稱，而不實現任何功能。`協議`能夠被類，枚舉，結構體實現，滿足協議要求的類，枚舉，結構體被稱為協議的`遵循者`。

`遵循者`需要提供`協議`指定的成員，如屬性，方法，操作符，下標等。

<a name="protocol_syntax"></a>
## 協議的語法

`協議`的定義與類，結構體，枚舉的定義非常相似，如下所示：

```swift
protocol SomeProtocol {
	// 協議內容
}
```

在類，結構體，枚舉的名稱後加上`協議名稱`，中間以冒號`:`分隔即可實現協議；實現多個協議時，各協議之間用逗號`,`分隔，如下所示：

```swift
struct SomeStructure: FirstProtocol, AnotherProtocol {
	// 結構體內容
}
```

當某個類含有父類的同時並實現了協議，應當把父類放在所有的協議之前，如下所示：

```swift
class SomeClass: SomeSuperClass, FirstProtocol, AnotherProtocol {
	// 類的內容
}
```

<a name="property_requirements"></a>
## 屬性要求

`協議`能夠要求其`遵循者`必須含有一些**特定名稱和類型**的`實例屬性(instance property)`或`類屬性 (type property)`，也能夠要求屬性具有`(設置權限)settable` 和`(訪問權限)gettable`，但它不要求`屬性`是`存儲型屬性(stored property)`還是`計算型屬性(calculate property)`。

如果協議要求屬性具有設置權限和訪問權限，那常量存儲型屬性或者只讀計算型屬性都無法滿足此要求。如果協議只要求屬性具有訪問權限，那任何類型的屬性都可以滿足此要求，無論這些屬性是否具有設置權限。

通常前置`var`關鍵字將屬性聲明為變量。在屬性聲明後寫上`{ get set }`表示屬性為可讀寫的。`{ get }`用來表示屬性為可讀的。即使你為可讀的屬性實現了`setter`方法，它也不會出錯。

```swift
protocol SomeProtocol {
	var musBeSettable : Int { get set }
	var doesNotNeedToBeSettable: Int { get }
}
```

用類來實現協議時，使用`class`關鍵字來表示該屬性為類成員；用結構體或枚舉實現協議時，則使用`static`關鍵字來表示：

```swift
protocol AnotherProtocol {
	class var someTypeProperty: Int { get set }
}

protocol FullyNamed {
	var fullName: String { get }
}
```

`FullyNamed`協議含有`fullName`屬性。因此其`遵循者`必須含有一個名為`fullName`，類型為`String`的可讀屬性。

```swift
struct Person: FullyNamed{
	var fullName: String
}
let john = Person(fullName: "John Appleseed")
//john.fullName 為 "John Appleseed"
```

`Person`結構體含有一個名為`fullName`的`存儲型屬性`，完整的`遵循`了協議。(*若協議未被完整遵循，編譯時則會報錯*)。

如下所示，`Startship`類`遵循`了`FullyNamed`協議：

```swift
class Starship: FullyNamed {
	var prefix: String?
	var name: String
	init(name: String, prefix: String? = nil ) {
		self.anme = name
		self.prefix = prefix
	}
	var fullName: String {
	return (prefix ? prefix ! + " " : " ") + name
	}
}
var ncc1701 = Starship(name: "Enterprise", prefix: "USS")
// ncc1701.fullName == "USS Enterprise"
```

`Starship`類將`fullName`實現為可讀的`計算型屬性`。它的每一個實例都有一個名為`name`的必備屬性和一個名為`prefix`的可選屬性。 當`prefix`存在時，將`prefix`插入到`name`之前來為`Starship`構建`fullName`。

<a name="method_requirements"></a>
## 方法要求

`協議`能夠要求其`遵循者`必備某些特定的`實例方法`和`類方法`。協議方法的聲明與普通方法聲明相似，但它不需要`方法`內容。

> 注意：
協議方法支持`變長參數(variadic parameter)`，不支持`默認參數(default parameter)`。

前置`class`關鍵字表示協議中的成員為`類成員`；當協議用於被`枚舉`或`結構體`遵循時，則使用`static`關鍵字。如下所示：

```swift
protocol SomeProtocol {
	class func someTypeMethod()
}

protocol RandomNumberGenerator {
	func random() -> Double
}
```

`RandomNumberGenerator`協議要求其`遵循者`必須擁有一個名為`random`， 返回值類型為`Double`的實例方法。(我們假設隨機數在[0，1]區間內)。

`LinearCongruentialGenerator`類`遵循`了`RandomNumberGenerator`協議，並提供了一個叫做*線性同余生成器(linear congruential generator)*的偽隨機數算法。

```swift
class LinearCongruentialGenerator: RandomNumberGenerator {
	var lastRandom = 42.0
	let m = 139968.0
	let a = 3877.0
	let c = 29573.0
	func random() -> Double {
		lastRandom = ((lastRandom * a + c) % m)
		return lastRandom / m
	}
}
let generator = LinearCongruentialGenerator()
println("Here's a random number: \(generator.random())")
// 輸出 : "Here's a random number: 0.37464991998171"
println("And another one: \(generator.random())")
// 輸出 : "And another one: 0.729023776863283"
```

<a name="mutating_method_requirements"></a>
## 突變方法要求

能在`方法`或`函數`內部改變實例類型的方法稱為`突變方法`。在`值類型(Value Type)`(*譯者注：特指結構體和枚舉*)中的的`函數`前綴加上`mutating`關鍵字來表示該函數允許改變該實例和其屬性的類型。 這一變換過程在[實例方法(Instance Methods)](11_Methods.html#instance_methods)章節中有詳細描述。

(*譯者注：類中的成員為`引用類型(Reference Type)`，可以方便的修改實例及其屬性的值而無需改變類型；而`結構體`和`枚舉`中的成員均為`值類型(Value Type)`，修改變量的值就相當於修改變量的類型，而`Swift`默認不允許修改類型，因此需要前置`mutating`關鍵字用來表示該`函數`中能夠修改類型*)

> 注意：
用`class`實現協議中的`mutating`方法時，不用寫`mutating`關鍵字；用`結構體`，`枚舉`實現協議中的`mutating`方法時，必須寫`mutating`關鍵字。

如下所示，`Togglable`協議含有`toggle`函數。根據函數名稱推測，`toggle`可能用於**切換或恢復**某個屬性的狀態。`mutating`關鍵字表示它為`突變方法`：

```swift
protocol Togglable {
	mutating func toggle()
}
```

當使用`枚舉`或`結構體`來實現`Togglabl`協議時，必須在`toggle`方法前加上`mutating`關鍵字。

如下所示，`OnOffSwitch`枚舉`遵循`了`Togglable`協議，`On`，`Off`兩個成員用於表示當前狀態

```swift
enum OnOffSwitch: Togglable {
	case Off, On
	mutating func toggle() {
		switch self {
		case Off:
			self = On
		case On:
			self = Off
		}
	}
}
var lightSwitch = OnOffSwitch.Off
lightSwitch.toggle()
//lightSwitch 現在的值為 .On
```

<a name="protocols_as_types"></a>
## 協議類型

`協議`本身不實現任何功能，但你可以將它當做`類型`來使用。

使用場景：

* 作為函數，方法或構造器中的參數類型，返回值類型
* 作為常量，變量，屬性的類型
* 作為數組，字典或其他容器中的元素類型

> 注意：
協議類型應與其他類型(Int，Double，String)的寫法相同，使用駝峰式

```swift
class Dice {
	let sides: Int
	let generator: RandomNumberGenerator
	init(sides: Int, generator: RandomNumberGenerator) {
		self.sides = sides
		self.generator = generator
	}
	func roll() -> Int {
		return Int(generator.random() * Double(sides)) +1
	}
}
```

這裡定義了一個名為 `Dice`的類，用來代表桌游中的N個面的骰子。

`Dice`含有`sides`和`generator`兩個屬性，前者用來表示骰子有幾個面，後者為骰子提供一個隨機數生成器。由於後者為`RandomNumberGenerator`的協議類型。所以它能夠被賦值為任意`遵循`該協議的類型。

此外，使用`構造器(init)`來代替之前版本中的`setup`操作。構造器中含有一個名為`generator`，類型為`RandomNumberGenerator`的形參，使得它可以接收任意遵循`RandomNumberGenerator`協議的類型。

`roll`方法用來模擬骰子的面值。它先使用`generator`的`random`方法來創建一個[0-1]區間內的隨機數種子，然後加工這個隨機數種子生成骰子的面值。

如下所示，`LinearCongruentialGenerator`的實例作為隨機數生成器傳入`Dice`的`構造器`

```swift
var d6 = Dice(sides: 6,generator: LinearCongruentialGenerator())
for _ in 1...5 {
	println("Random dice roll is \(d6.roll())")
}
//輸出結果
//Random dice roll is 3
//Random dice roll is 5
//Random dice roll is 4
//Random dice roll is 5
//Random dice roll is 4
```

<a name="delegation"></a>
## 委托(代理)模式

委托是一種設計模式，它允許類或結構體將一些需要它們負責的功能`交由(委托)`給其他的類型。

委托模式的實現很簡單： 定義`協議`來`封裝`那些需要被委托的`函數和方法`， 使其`遵循者`擁有這些被委托的`函數和方法`。

委托模式可以用來響應特定的動作或接收外部數據源提供的數據，而無需要知道外部數據源的類型。

下文是兩個基於骰子游戲的協議：

```swift
protocol DiceGame {
	var dice: Dice { get }
	func play()
}

protocol DiceGameDelegate {
	func gameDidStart(game: DiceGame)
	func game(game: DiceGame, didStartNewTurnWithDiceRoll diceRoll:Int)
	func gameDidEnd(game: DiceGame)
}
```

`DiceGame`協議可以在任意含有骰子的游戲中實現，`DiceGameDelegate`協議可以用來追蹤`DiceGame`的游戲過程。

如下所示，`SnakesAndLadders`是`Snakes and Ladders`(譯者注：[控制流](05_Control_Flow.html)章節有該游戲的詳細介紹)游戲的新版本。新版本使用`Dice`作為骰子，並且實現了`DiceGame`和`DiceGameDelegate`協議

```swift
class SnakesAndLadders: DiceGame {
	let finalSquare = 25
	let dic = Dice(sides: 6, generator: LinearCongruentialGenerator())
	var square = 0
	var board: Int[]
	init() {
		board = Int[](count: finalSquare + 1, repeatedValue: 0)
		board[03] = +08; board[06] = +11; borad[09] = +09; board[10] = +02
		borad[14] = -10; board[19] = -11; borad[22] = -02; board[24] = -08
	}
 	var delegate: DiceGameDelegate?
 	func play() {
 		square = 0
 		delegate?.gameDidStart(self)
 		gameLoop: while square != finalSquare {
 			let diceRoll = dice.roll()
 			delegate?.game(self,didStartNewTurnWithDiceRoll: diceRoll)
 			switch square + diceRoll {
 			case finalSquare:
 				break gameLoop
 			case let newSquare where newSquare > finalSquare:
 				continue gameLoop
 			default:
 			square += diceRoll
 			square += board[square]
 			}
 		}
 		delegate?.gameDIdEnd(self)
 	}
}
```

游戲的`初始化設置(setup)`被`SnakesAndLadders`類的`構造器(initializer)`實現。所有的游戲邏輯被轉移到了`play`方法中。

> 注意：
因為`delegate`並不是該游戲的必備條件，`delegate`被定義為遵循`DiceGameDelegate`協議的可選屬性

`DicegameDelegate`協議提供了三個方法用來追蹤游戲過程。被放置於游戲的邏輯中，即`play()`方法內。分別在游戲開始時，新一輪開始時，游戲結束時被調用。

因為`delegate`是一個遵循`DiceGameDelegate`的可選屬性，因此在`play()`方法中使用了`可選鏈`來調用委托方法。 若`delegate`屬性為`nil`， 則委托調用*優雅地*失效。若`delegate`不為`nil`，則委托方法被調用

如下所示，`DiceGameTracker`遵循了`DiceGameDelegate`協議

```swift
class DiceGameTracker: DiceGameDelegate {
    var numberOfTurns = 0
    func gameDidStart(game: DiceGame) {
        numberOfTurns = 0
        if game is SnakesAndLadders {
            println("Started a new game of Snakes and Ladders")
        }
        println("The game is using a \(game.dice.sides)-sided dice")
    }
    func game(game: DiceGame, didStartNewTurnWithDiceRoll diceRoll: Int) {
        ++numberOfTurns
        println("Rolled a \(diceRoll)")
    }
    func gameDidEnd(game: DiceGame) {
        println("The game lasted for \(numberOfTurns) turns")
    }
}
```

`DiceGameTracker`實現了`DiceGameDelegate`協議的方法要求，用來記錄游戲已經進行的輪數。 當游戲開始時，`numberOfTurns`屬性被賦值為0；在每新一輪中遞加；游戲結束後，輸出打印游戲的總輪數。

`gameDidStart`方法從`game`參數獲取游戲信息並輸出。`game`在方法中被當做`DiceGame`類型而不是`SnakeAndLadders`類型，所以方法中只能訪問`DiceGame`協議中的成員。

`DiceGameTracker`的運行情況，如下所示：

```swift
let tracker = DiceGameTracker()
let game = SnakesAndLadders()
game.delegate = tracker
game.play()
// Started a new game of Snakes and Ladders
// The game is using a 6-sided dice
// Rolled a 3
// Rolled a 5
// Rolled a 4
// Rolled a 5
// The game lasted for 4 turns
```

<a name="adding_protocol_conformance_with_an_extension"></a>
## 在擴展中添加協議成員

即便無法修改源代碼，依然可以通過`擴展(Extension)`來擴充已存在類型(*譯者注： 類，結構體，枚舉等*)。`擴展`可以為已存在的類型添加`屬性`，`方法`，`下標`，`協議`等成員。詳情請在[擴展](20_Extensions.html)章節中查看。

> 注意：
通過`擴展`為已存在的類型`遵循`協議時，該類型的所有實例也會隨之添加協議中的方法

`TextRepresentable`協議含有一個`asText`，如下所示：

```swift
protocol TextRepresentable {
	func asText() -> String
}
```

通過`擴展`為上一節中提到的`Dice`類遵循`TextRepresentable`協議

```swift
extension Dice: TextRepresentable {
	cun asText() -> String {
		return "A \(sides)-sided dice"
	}
}
```

從現在起，`Dice`類型的實例可被當作`TextRepresentable`類型：

```swift
let d12 = Dice(sides: 12,generator: LinearCongruentialGenerator())
println(d12.asText())
// 輸出 "A 12-sided dice"
```

`SnakesAndLadders`類也可以通過`擴展`的方式來遵循協議：

```swift
extension SnakeAndLadders: TextRepresentable {
	func asText() -> String {
		return "A game of Snakes and Ladders with \(finalSquare) squares"
	}
}
println(game.asText())
// 輸出 "A game of Snakes and Ladders with 25 squares"
```

<a name="declaring_protocol_adoption_with_an_extension"></a>
## 通過擴展補充協議聲明

當一個類型已經實現了協議中的所有要求，卻沒有聲明時，可以通過`擴展`來補充協議聲明：

```swift
struct Hamster {
	var name: String
	func asText() -> String {
		return "A hamster named \(name)"
	}
}
extension Hamster: TextRepresentabl {}
```

從現在起，`Hamster`的實例可以作為`TextRepresentable`類型使用

```swift
let simonTheHamster = Hamster(name: "Simon")
let somethingTextRepresentable: TextRepresentabl = simonTheHamester
println(somethingTextRepresentable.asText())
// 輸出 "A hamster named Simon"
```

> 注意：
即時滿足了協議的所有要求，類型也不會自動轉變，因此你必須為它做出明顯的協議聲明

<a name="collections_of_protocol_types"></a>
## 集合中的協議類型

協議類型可以被集合使用，表示集合中的元素均為協議類型：

```swift
let things: TextRepresentable[] = [game,d12,simoTheHamster]
```

如下所示，`things`數組可以被直接遍歷，並調用其中元素的`asText()`函數：

```swift
for thing in things {
	println(thing.asText())
}
// A game of Snakes and Ladders with 25 squares
// A 12-sided dice
// A hamster named Simon
```

`thing`被當做是`TextRepresentable`類型而不是`Dice`，`DiceGame`，`Hamster`等類型。因此能且僅能調用`asText`方法

<a name="protocol_inheritance"></a>
## 協議的繼承

協議能夠*繼承*一到多個其他協議。語法與類的繼承相似，多個協議間用逗號`,`分隔

```swift
protocol InheritingProtocol: SomeProtocol, AnotherProtocol {
	// 協議定義
}
```

如下所示，`PrettyTextRepresentable`協議繼承了`TextRepresentable`協議

```swift
protocol PrettyTextRepresentable: TextRepresentable {
	func asPrettyText() -> String
}
```

`遵循``PrettyTextRepresentable`協議的同時，也需要`遵循`TextRepresentable`協議。

如下所示，用`擴展`為`SnakesAndLadders`遵循`PrettyTextRepresentable`協議：

```swift
extension SnakesAndLadders: PrettyTextRepresentable {
    func asPrettyText() -> String {
        var output = asText() + ":\n"
        for index in 1...finalSquare {
            switch board[index] {
            	case let ladder where ladder > 0:
                output += "▲ "
            case let snake where snake < 0:
                output += "▼ "
            default:
                output += "○ "
            }
        }
        return output
    }
}
```

在`for in`中迭代出了`board`數組中的每一個元素：

* 當從數組中迭代出的元素的值大於0時，用`▲`表示
* 當從數組中迭代出的元素的值小於0時，用`▼`表示
* 當從數組中迭代出的元素的值等於0時，用`○`表示

任意`SankesAndLadders`的實例都可以使用`asPrettyText()`方法。

```swift
println(game.asPrettyText())
// A game of Snakes and Ladders with 25 squares:
// ○ ○ ▲ ○ ○ ▲ ○ ○ ▲ ▲ ○ ○ ○ ▼ ○ ○ ○ ○ ▼ ○ ○ ▼ ○ ▼ ○
```

<a name="protocol_composition"></a>
## 協議合成

一個協議可由多個協議采用`protocol<SomeProtocol, AnotherProtocol>`這樣的格式進行組合，稱為`協議合成(protocol composition)`。

舉個例子：

```swift
protocol Named {
    var name: String { get }
}
protocol Aged {
    var age: Int { get }
}
struct Person: Named, Aged {
    var name: String
    var age: Int
}
func wishHappyBirthday(celebrator: protocol<Named, Aged>) {
    println("Happy birthday \(celebrator.name) - you're \(celebrator.age)!")
}
let birthdayPerson = Person(name: "Malcolm", age: 21)
wishHappyBirthday(birthdayPerson)
// 輸出 "Happy birthday Malcolm - you're 21!
```

`Named`協議包含`String`類型的`name`屬性；`Aged`協議包含`Int`類型的`age`屬性。`Person`結構體`遵循`了這兩個協議。

`wishHappyBirthday`函數的形參`celebrator`的類型為`protocol<Named,Aged>`。可以傳入任意`遵循`這兩個協議的類型的實例

> 注意：
`協議合成`並不會生成一個新協議類型，而是將多個協議合成為一個臨時的協議，超出範圍後立即失效。

<a name="checking_for_protocol_conformance"></a>
## 檢驗協議的一致性

使用`is`檢驗協議一致性，使用`as`將協議類型`向下轉換(downcast)`為的其他協議類型。檢驗與轉換的語法和之前相同(*詳情查看[類型檢查](18_Type_Casting.html)*)：

* `is`操作符用來檢查實例是否`遵循`了某個`協議`。
* `as?`返回一個可選值，當實例`遵循`協議時，返回該協議類型；否則返回`nil`
* `as`用以強制向下轉換型。

```swift
@objc protocol HasArea {
    var area: Double { get }
}
```


> 注意：
`@objc`用來表示協議是可選的，也可以用來表示暴露給`Objective-C`的代碼，此外，`@objc`型協議只對`類`有效，因此只能在`類`中檢查協議的一致性。詳情查看*[Using Siwft with Cocoa and Objectivei-c](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/BuildingCocoaApps/index.html#//apple_ref/doc/uid/TP40014216)*。

```swift
class Circle: HasArea {
    let pi = 3.1415927
    var radius: Double
    var area:≈radius }
    init(radius: Double) { self.radius = radius }
}
class Country: HasArea {
    var area: Double
    init(area: Double) { self.area = area }
}
```

`Circle`和`Country`都遵循了`HasArea`協議，前者把`area`寫為計算型屬性（computed property），後者則把`area`寫為存儲型屬性（stored property）。

如下所示，`Animal`類沒有實現任何協議

```swift
class Animal {
	var legs: Int
	init(legs: Int) { self.legs = legs }
}
```

`Circle,Country,Animal`並沒有一個相同的基類，所以采用`AnyObject`類型的數組來裝載在它們的實例，如下所示：

```swift
let objects: AnyObject[] = [
	Circle(radius: 2.0),
	Country(area: 243_610),
	Animal(legs: 4)
]
```

如下所示，在迭代時檢查`object`數組的元素是否`遵循`了`HasArea`協議：

```swift
for object in objects {
    if let objectWithArea = object as? HasArea {
        println("Area is \(objectWithArea.area)")
    } else {
        println("Something that doesn't have an area")
    }
}
// Area is 12.5663708
// Area is 243610.0
// Something that doesn't have an area
```

當數組中的元素遵循`HasArea`協議時，通過`as?`操作符將其`可選綁定(optional binding)`到`objectWithArea`常量上。

`objects`數組中元素的類型並不會因為`向下轉型`而改變，當它們被賦值給`objectWithArea`時只被視為`HasArea`類型，因此只有`area`屬性能夠被訪問。

<a name="optional_protocol_requirements"></a>
## 可選協議要求

可選協議含有可選成員，其`遵循者`可以選擇是否實現這些成員。在協議中使用`@optional`關鍵字作為前綴來定義可選成員。

可選協議在調用時使用`可選鏈`，詳細內容在[可選鏈](17_Optional_Chaining.html)章節中查看。

像`someOptionalMethod?(someArgument)`一樣，你可以在可選方法名稱後加上`?`來檢查該方法是否被實現。`可選方法`和`可選屬性`都會返回一個`可選值(optional value)`，當其不可訪問時，`?`之後語句不會執行，並返回`nil`。

> 注意：
可選協議只能在含有`@objc`前綴的協議中生效。且`@objc`的協議只能被`類`遵循。

`Counter`類使用`CounterDataSource`類型的外部數據源來提供`增量值(increment amount)`，如下所示：

```swift
@objc protocol CounterDataSource {
    @optional func incrementForCount(count: Int) -> Int
    @optional var fixedIncrement: Int { get }
}
```

`CounterDataSource`含有`incrementForCount`的`可選方法`和`fiexdIncrement`的`可選屬性`。

> 注意：
`CounterDataSource`中的屬性和方法都是可選的，因此可以在類中聲明但不實現這些成員，盡管技術上允許這樣做，不過最好不要這樣寫。

`Counter`類含有`CounterDataSource?`類型的可選屬性`dataSource`，如下所示：

```swift
@objc class Counter {
    var count = 0
    var dataSource: CounterDataSource?
    func increment() {
        if let amount = dataSource?.incrementForCount?(count) {
            count += amount
        } else if let amount = dataSource?.fixedIncrement? {
            count += amount
        }
    }
}
```

`count`屬性用於存儲當前的值，`increment`方法用來為`count`賦值。

`increment`方法通過`可選鏈`，嘗試從兩種`可選成員`中獲取`count`。

1. 由於`dataSource`可能為`nil`，因此在`dataSource`後邊加上了`?`標記來表明只在`dataSource`非空時才去調用incrementForCount`方法。
2. 即使`dataSource`存在，但是也無法保證其是否實現了`incrementForCount`方法，因此在`incrementForCount`方法後邊也加有`?`標記。

在調用`incrementForCount`方法後，`Int`型`可選值`通過`可選綁定(optional binding)`自動拆包並賦值給常量`amount`。

當`incrementForCount`不能被調用時，嘗試使用`可選屬性``fixedIncrement`來代替。

`ThreeSource`實現了`CounterDataSource`協議，如下所示：

```swift
class ThreeSource: CounterDataSource {
	let fixedIncrement = 3
}
```

使用`ThreeSource`作為數據源開實例化一個`Counter`：

```swift
var counter = Counter()
counter.dataSource = ThreeSource()
for _ in 1...4 {
    counter.increment()
    println(counter.count)
}
// 3
// 6
// 9
// 12
```

`TowardsZeroSource`實現了`CounterDataSource`協議中的`incrementForCount`方法，如下所示：

```swift
class TowardsZeroSource: CounterDataSource {
func incrementForCount(count: Int) -> Int {
        if count == 0 {
            return 0
        } else if count < 0 {
            return 1
        } else {
            return -1
        }
    }
}
```

下邊是執行的代碼：

```swift
counter.count = -4
counter.dataSource = TowardsZeroSource()
for _ in 1...5 {
    counter.increment()
    println(counter.count)
}
// -3
// -2
// -1
// 0
// 0
```
