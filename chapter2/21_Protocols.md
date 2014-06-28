> 翻譯：[geek5nan](https://github.com/geek5nan)
> 校對：[dabing1022](https://github.com/dabing1022)

# 協定
-----------------

本頁包含內容：

- [協定的語法（Protocol Syntax）](#protocol_syntax)
- [屬性要求（Property Requirements）](#property_requirements)
- [方法要求（Method Requirements）](#method_requirements)
- [突變方法要求（Mutating Method Requirements）](#mutating_method_requirements)
- [協定型別（Protocols as Types）](#protocols_as_types)
- [委托(代理)模式（Delegation）](#delegation)
- [在擴展中添加協定成員（Adding Protocol Conformance with an Extension）](#adding_protocol_conformance_with_an_extension)
- [通過擴展補充協定宣告（Declaring Protocol Adoption with an Extension）](#declaring_protocol_adoption_with_an_extension)
- [集合中的協定型別（Collections of Protocol Types）](#collections_of_protocol_types)
- [協定的繼承（Protocol Inheritance）](#protocol_inheritance)
- [協定合成（Protocol Composition）](#protocol_composition)
- [檢驗協定的一致性（Checking for Protocol Conformance）](#checking_for_protocol_conformance)
- [可選協定要求（Optional Protocol Requirements）](#optional_protocol_requirements)

`Protocol(協定)`用於**統一**方法和屬性的名稱，而不實作任何功能。`協定`能夠被類別，列舉，結構實作，滿足協定要求的類別，列舉，結構被稱為協定的`遵循者`。

`遵循者`需要提供`協定`指定的成員，如屬性，方法，運算子，下標等。

<a name="protocol_syntax"></a>
## 協定的語法

`協定`的定義與類別，結構，列舉的定義非常相似，如下所示：

```swift
protocol SomeProtocol {
	// 協定內容
}
```

在類別，結構，列舉的名稱後加上`協定名稱`，中間以冒號`:`分隔即可實作協定；實作多個協定時，各協定之間用逗號`,`分隔，如下所示：

```swift
struct SomeStructure: FirstProtocol, AnotherProtocol {
	// 結構內容
}
```

當某個類別含有父類別的同時並實作了協定，應當把父類別放在所有的協定之前，如下所示：

```swift
class SomeClass: SomeSuperClass, FirstProtocol, AnotherProtocol {
	// 類別的內容
}
```

<a name="property_requirements"></a>
## 屬性要求

`協定`能夠要求其`遵循者`必須含有一些**特定名稱和型別**的`實例屬性(instance property)`或`類別屬性 (type property)`，也能夠要求屬性具有`(設置權限)settable` 和`(存取權限)gettable`，但它不要求`屬性`是`儲存型屬性(stored property)`還是`計算型屬性(calculate property)`。

如果協定要求屬性具有設置權限和存取權限，那常數儲存型屬性或者唯讀計算型屬性都無法滿足此要求。如果協定只要求屬性具有存取權限，那任何型別的屬性都可以滿足此要求，無論這些屬性是否具有設置權限。

通常前綴`var`關鍵字將屬性宣告為變數。在屬性宣告後寫上`{ get set }`表示屬性為可讀寫的。`{ get }`用來表示屬性為可讀的。即使你為可讀的屬性實作了`setter`方法，它也不會出錯。

```swift
protocol SomeProtocol {
	var musBeSettable : Int { get set }
	var doesNotNeedToBeSettable: Int { get }
}
```

用類別來實作協定時，使用`class`關鍵字來表示該屬性為類別成員；用結構或列舉實作協定時，則使用`static`關鍵字來表示：

```swift
protocol AnotherProtocol {
	class var someTypeProperty: Int { get set }
}

protocol FullyNamed {
	var fullName: String { get }
}
```

`FullyNamed`協定含有`fullName`屬性。因此其`遵循者`必須含有一個名為`fullName`，型別為`String`的可讀屬性。

```swift
struct Person: FullyNamed{
	var fullName: String
}
let john = Person(fullName: "John Appleseed")
//john.fullName 為 "John Appleseed"
```

`Person`結構含有一個名為`fullName`的`儲存型屬性`，完整的`遵循`了協定。(*若協定未被完整遵循，編譯時則會報錯*)。

如下所示，`Startship`類別`遵循`了`FullyNamed`協定：

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

`Starship`類別將`fullName`實作為可讀的`計算型屬性`。它的每一個實例都有一個名為`name`的必備屬性和一個名為`prefix`的可選屬性。 當`prefix`存在時，將`prefix`插入到`name`之前來為`Starship`構建`fullName`。

<a name="method_requirements"></a>
## 方法要求

`協定`能夠要求其`遵循者`必備某些特定的`實例方法`和`類別方法`。協定方法的宣告與普通方法宣告相似，但它不需要`方法`內容。

> 注意：
協定方法支援`變長參數(variadic parameter)`，不支援`預設參數(default parameter)`。

前綴`class`關鍵字表示協定中的成員為`類別成員`；當協定用於被`列舉`或`結構`遵循時，則使用`static`關鍵字。如下所示：

```swift
protocol SomeProtocol {
	class func someTypeMethod()
}

protocol RandomNumberGenerator {
	func random() -> Double
}
```

`RandomNumberGenerator`協定要求其`遵循者`必須擁有一個名為`random`， 回傳值型別為`Double`的實例方法。(我們假設隨機數在[0，1]區間內)。

`LinearCongruentialGenerator`類別`遵循`了`RandomNumberGenerator`協定，並提供了一個叫做*線性同余生成器(linear congruential generator)*的偽隨機數算法。

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

能在`方法`或`函式`內部改變實例型別的方法稱為`突變方法`。在`值型別(Value Type)`(*譯者注：特指結構和列舉*)中的的`函式`前綴加上`mutating`關鍵字來表示該函式允許改變該實例和其屬性的型別。 這一變換過程在[實例方法(Instance Methods)](11_Methods.html#instance_methods)章節中有詳細描述。

(*譯者注：類別中的成員為`參考型別(Reference Type)`，可以方便的修改實例及其屬性的值而無需改變型別；而`結構`和`列舉`中的成員均為`值型別(Value Type)`，修改變數的值就相當於修改變數的型別，而`Swift`預設不允許修改型別，因此需要前綴`mutating`關鍵字用來表示該`函式`中能夠修改型別*)

> 注意：
用`class`實作協定中的`mutating`方法時，不用寫`mutating`關鍵字；用`結構`，`列舉`實作協定中的`mutating`方法時，必須寫`mutating`關鍵字。

如下所示，`Togglable`協定含有`toggle`函式。根據函式名稱推測，`toggle`可能用於**切換或恢復**某個屬性的狀態。`mutating`關鍵字表示它為`突變方法`：

```swift
protocol Togglable {
	mutating func toggle()
}
```

當使用`列舉`或`結構`來實作`Togglabl`協定時，必須在`toggle`方法前加上`mutating`關鍵字。

如下所示，`OnOffSwitch`列舉`遵循`了`Togglable`協定，`On`，`Off`兩個成員用於表示當前狀態

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
## 協定型別

`協定`本身不實作任何功能，但你可以將它當做`型別`來使用。

使用場景：

* 作為函式，方法或建構器中的參數型別，回傳值型別
* 作為常數，變數，屬性的型別
* 作為陣列，字典或其他容器中的元素型別

> 注意：
協定型別應與其他型別(Int，Double，String)的寫法相同，使用駝峰式

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

這裡定義了一個名為 `Dice`的類別，用來代表桌游中的N個面的骰子。

`Dice`含有`sides`和`generator`兩個屬性，前者用來表示骰子有幾個面，後者為骰子提供一個隨機數生成器。由於後者為`RandomNumberGenerator`的協定型別。所以它能夠被賦值為任意`遵循`該協定的型別。

此外，使用`建構器(init)`來代替之前版本中的`setup`操作。建構器中含有一個名為`generator`，型別為`RandomNumberGenerator`的形參，使得它可以接收任意遵循`RandomNumberGenerator`協定的型別。

`roll`方法用來模擬骰子的面值。它先使用`generator`的`random`方法來創建一個[0-1]區間內的隨機數種子，然後加工這個隨機數種子生成骰子的面值。

如下所示，`LinearCongruentialGenerator`的實例作為隨機數生成器傳入`Dice`的`建構器`

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

委托是一種設計模式，它允許類別或結構將一些需要它們負責的功能`交由(委托)`給其他的型別。

委托模式的實作很簡單： 定義`協定`來`封裝`那些需要被委托的`函式和方法`， 使其`遵循者`擁有這些被委托的`函式和方法`。

委托模式可以用來響應特定的動作或接收外部資料源提供的資料，而無需要知道外部資料源的型別。

下文是兩個基於骰子遊戲的協定：

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

`DiceGame`協定可以在任意含有骰子的遊戲中實作，`DiceGameDelegate`協定可以用來追蹤`DiceGame`的遊戲過程。

如下所示，`SnakesAndLadders`是`Snakes and Ladders`(譯者注：[控制流程](05_Control_Flow.html)章節有該遊戲的詳細介紹)遊戲的新版本。新版本使用`Dice`作為骰子，並且實作了`DiceGame`和`DiceGameDelegate`協定

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

遊戲的`初始化設置(setup)`被`SnakesAndLadders`類別的`建構器(initializer)`實作。所有的遊戲邏輯被轉移到了`play`方法中。

> 注意：
因為`delegate`並不是該遊戲的必備條件，`delegate`被定義為遵循`DiceGameDelegate`協定的可選屬性

`DicegameDelegate`協定提供了三個方法用來追蹤遊戲過程。被放置於遊戲的邏輯中，即`play()`方法內。分別在遊戲開始時，新一輪開始時，遊戲結束時被呼叫。

因為`delegate`是一個遵循`DiceGameDelegate`的可選屬性，因此在`play()`方法中使用了`可選鏈`來呼叫委托方法。 若`delegate`屬性為`nil`， 則委托呼叫*優雅地*失效。若`delegate`不為`nil`，則委托方法被呼叫

如下所示，`DiceGameTracker`遵循了`DiceGameDelegate`協定

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

`DiceGameTracker`實作了`DiceGameDelegate`協定的方法要求，用來記錄遊戲已經進行的輪數。 當遊戲開始時，`numberOfTurns`屬性被賦值為0；在每新一輪中遞加；遊戲結束後，輸出列印遊戲的總輪數。

`gameDidStart`方法從`game`參數獲取遊戲資訊並輸出。`game`在方法中被當做`DiceGame`型別而不是`SnakeAndLadders`型別，所以方法中只能存取`DiceGame`協定中的成員。

`DiceGameTracker`的執行情況，如下所示：

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
## 在擴展中添加協定成員

即便無法修改源程式碼，依然可以通過`擴展(Extension)`來擴充已存在型別(*譯者注： 類別，結構，列舉等*)。`擴展`可以為已存在的型別添加`屬性`，`方法`，`下標`，`協定`等成員。詳情請在[擴展](20_Extensions.html)章節中查看。

> 注意：
通過`擴展`為已存在的型別`遵循`協定時，該型別的所有實例也會隨之添加協定中的方法

`TextRepresentable`協定含有一個`asText`，如下所示：

```swift
protocol TextRepresentable {
	func asText() -> String
}
```

通過`擴展`為上一節中提到的`Dice`類別遵循`TextRepresentable`協定

```swift
extension Dice: TextRepresentable {
	cun asText() -> String {
		return "A \(sides)-sided dice"
	}
}
```

從現在起，`Dice`型別的實例可被當作`TextRepresentable`型別：

```swift
let d12 = Dice(sides: 12,generator: LinearCongruentialGenerator())
println(d12.asText())
// 輸出 "A 12-sided dice"
```

`SnakesAndLadders`類別也可以通過`擴展`的方式來遵循協定：

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
## 通過擴展補充協定宣告

當一個型別已經實作了協定中的所有要求，卻沒有宣告時，可以通過`擴展`來補充協定宣告：

```swift
struct Hamster {
	var name: String
	func asText() -> String {
		return "A hamster named \(name)"
	}
}
extension Hamster: TextRepresentabl {}
```

從現在起，`Hamster`的實例可以作為`TextRepresentable`型別使用

```swift
let simonTheHamster = Hamster(name: "Simon")
let somethingTextRepresentable: TextRepresentabl = simonTheHamester
println(somethingTextRepresentable.asText())
// 輸出 "A hamster named Simon"
```

> 注意：
即時滿足了協定的所有要求，型別也不會自動轉變，因此你必須為它做出明顯的協定宣告

<a name="collections_of_protocol_types"></a>
## 集合中的協定型別

協定型別可以被集合使用，表示集合中的元素均為協定型別：

```swift
let things: TextRepresentable[] = [game,d12,simoTheHamster]
```

如下所示，`things`陣列可以被直接遍歷，並呼叫其中元素的`asText()`函式：

```swift
for thing in things {
	println(thing.asText())
}
// A game of Snakes and Ladders with 25 squares
// A 12-sided dice
// A hamster named Simon
```

`thing`被當做是`TextRepresentable`型別而不是`Dice`，`DiceGame`，`Hamster`等型別。因此能且僅能呼叫`asText`方法

<a name="protocol_inheritance"></a>
## 協定的繼承

協定能夠*繼承*一到多個其他協定。語法與類別的繼承相似，多個協定間用逗號`,`分隔

```swift
protocol InheritingProtocol: SomeProtocol, AnotherProtocol {
	// 協定定義
}
```

如下所示，`PrettyTextRepresentable`協定繼承了`TextRepresentable`協定

```swift
protocol PrettyTextRepresentable: TextRepresentable {
	func asPrettyText() -> String
}
```

`遵循``PrettyTextRepresentable`協定的同時，也需要`遵循`TextRepresentable`協定。

如下所示，用`擴展`為`SnakesAndLadders`遵循`PrettyTextRepresentable`協定：

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

在`for in`中迭代出了`board`陣列中的每一個元素：

* 當從陣列中迭代出的元素的值大於0時，用`▲`表示
* 當從陣列中迭代出的元素的值小於0時，用`▼`表示
* 當從陣列中迭代出的元素的值等於0時，用`○`表示

任意`SankesAndLadders`的實例都可以使用`asPrettyText()`方法。

```swift
println(game.asPrettyText())
// A game of Snakes and Ladders with 25 squares:
// ○ ○ ▲ ○ ○ ▲ ○ ○ ▲ ▲ ○ ○ ○ ▼ ○ ○ ○ ○ ▼ ○ ○ ▼ ○ ▼ ○
```

<a name="protocol_composition"></a>
## 協定合成

一個協定可由多個協定采用`protocol<SomeProtocol, AnotherProtocol>`這樣的格式進行組合，稱為`協定合成(protocol composition)`。

舉個範例：

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

`Named`協定包含`String`型別的`name`屬性；`Aged`協定包含`Int`型別的`age`屬性。`Person`結構`遵循`了這兩個協定。

`wishHappyBirthday`函式的形參`celebrator`的型別為`protocol<Named,Aged>`。可以傳入任意`遵循`這兩個協定的型別的實例

> 注意：
`協定合成`並不會生成一個新協定型別，而是將多個協定合成為一個臨時的協定，超出範圍後立即失效。

<a name="checking_for_protocol_conformance"></a>
## 檢驗協定的一致性

使用`is`檢驗協定一致性，使用`as`將協定型別`向下轉換(downcast)`為的其他協定型別。檢驗與轉換的語法和之前相同(*詳情查看[型別檢查](18_Type_Casting.html)*)：

* `is`運算子用來檢查實例是否`遵循`了某個`協定`。
* `as?`回傳一個可選值，當實例`遵循`協定時，回傳該協定型別；否則回傳`nil`
* `as`用以強制向下轉換型。

```swift
@objc protocol HasArea {
    var area: Double { get }
}
```


> 注意：
`@objc`用來表示協定是可選的，也可以用來表示暴露給`Objective-C`的程式碼，此外，`@objc`型協定只對`類別`有效，因此只能在`類別`中檢查協定的一致性。詳情查看*[Using Siwft with Cocoa and Objectivei-c](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/BuildingCocoaApps/index.html#//apple_ref/doc/uid/TP40014216)*。

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

`Circle`和`Country`都遵循了`HasArea`協定，前者把`area`寫為計算型屬性（computed property），後者則把`area`寫為儲存型屬性（stored property）。

如下所示，`Animal`類別沒有實作任何協定

```swift
class Animal {
	var legs: Int
	init(legs: Int) { self.legs = legs }
}
```

`Circle,Country,Animal`並沒有一個相同的基類別，所以采用`AnyObject`型別的陣列來裝載在它們的實例，如下所示：

```swift
let objects: AnyObject[] = [
	Circle(radius: 2.0),
	Country(area: 243_610),
	Animal(legs: 4)
]
```

如下所示，在迭代時檢查`object`陣列的元素是否`遵循`了`HasArea`協定：

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

當陣列中的元素遵循`HasArea`協定時，通過`as?`運算子將其`可選綁定(optional binding)`到`objectWithArea`常數上。

`objects`陣列中元素的型別並不會因為`向下轉型`而改變，當它們被賦值給`objectWithArea`時只被視為`HasArea`型別，因此只有`area`屬性能夠被存取。

<a name="optional_protocol_requirements"></a>
## 可選協定要求

可選協定含有可選成員，其`遵循者`可以選擇是否實作這些成員。在協定中使用`@optional`關鍵字作為前綴來定義可選成員。

可選協定在呼叫時使用`可選鏈`，詳細內容在[可選鏈](17_Optional_Chaining.html)章節中查看。

像`someOptionalMethod?(someArgument)`一樣，你可以在可選方法名稱後加上`?`來檢查該方法是否被實作。`可選方法`和`可選屬性`都會回傳一個`可選值(optional value)`，當其不可存取時，`?`之後語句不會執行，並回傳`nil`。

> 注意：
可選協定只能在含有`@objc`前綴的協定中生效。且`@objc`的協定只能被`類別`遵循。

`Counter`類別使用`CounterDataSource`型別的外部資料源來提供`增量值(increment amount)`，如下所示：

```swift
@objc protocol CounterDataSource {
    @optional func incrementForCount(count: Int) -> Int
    @optional var fixedIncrement: Int { get }
}
```

`CounterDataSource`含有`incrementForCount`的`可選方法`和`fiexdIncrement`的`可選屬性`。

> 注意：
`CounterDataSource`中的屬性和方法都是可選的，因此可以在類別中宣告但不實作這些成員，儘管技術上允許這樣做，不過最好不要這樣寫。

`Counter`類別含有`CounterDataSource?`型別的可選屬性`dataSource`，如下所示：

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

`count`屬性用於儲存當前的值，`increment`方法用來為`count`賦值。

`increment`方法通過`可選鏈`，嘗試從兩種`可選成員`中獲取`count`。

1. 由於`dataSource`可能為`nil`，因此在`dataSource`後邊加上了`?`標記來表明只在`dataSource`非空時才去呼叫incrementForCount`方法。
2. 即使`dataSource`存在，但是也無法保證其是否實作了`incrementForCount`方法，因此在`incrementForCount`方法後邊也加有`?`標記。

在呼叫`incrementForCount`方法後，`Int`型`可選值`通過`可選綁定(optional binding)`自動拆包並賦值給常數`amount`。

當`incrementForCount`不能被呼叫時，嘗試使用`可選屬性``fixedIncrement`來代替。

`ThreeSource`實作了`CounterDataSource`協定，如下所示：

```swift
class ThreeSource: CounterDataSource {
	let fixedIncrement = 3
}
```

使用`ThreeSource`作為資料源開實例化一個`Counter`：

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

`TowardsZeroSource`實作了`CounterDataSource`協定中的`incrementForCount`方法，如下所示：

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

下邊是執行的程式碼：

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
