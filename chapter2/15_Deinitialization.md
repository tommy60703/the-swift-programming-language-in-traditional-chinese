> 翻譯：[bruce0505](https://github.com/bruce0505)
> 校對：[fd5788](https://github.com/fd5788)

# 析構過程（Deinitialization）
---------------------------

本頁包含內容：

- [析構過程原理](#how_deinitialization_works)
- [析構函式操作](#deinitializers_in_action)

在一個類別的實例被釋放之前，析構函式被立即呼叫。用關鍵字`deinit`來標示析構函式，類似於初始化函式用`init`來標示。析構函式只適用於類型別。

<a name="how_deinitialization_works"></a>
##析構過程原理

Swift 會自動釋放不再需要的實例以釋放資源。如[自動引用計數](16_Automatic_Reference_Counting.html)那一章描述，Swift 通過_自動引用計數_（ARC）處理實例的內存管理。通常當你的實例被釋放時不需要手動地去清理。但是，當使用自己的資源時，你可能需要進行一些額外的清理。例如，如果創建了一個自定義的類別來打開一個文件，並寫入一些資料，你可能需要在類別實例被釋放之前關閉該文件。

在類別的定義中，每個類別最多只能有一個析構函式。析構函式不帶任何參數，在寫法上不帶括號：

```swift
deinit {
    // 執行析構過程
}
```

析構函式是在實例釋放發生前一步被自動呼叫。不允許主動呼叫自己的析構函式。子類別繼承了父類別的析構函式，並且在子類別析構函式實作的最後，父類別的析構函式被自動呼叫。即使子類別沒有提供自己的析構函式，父類別的析構函式也總是被呼叫。

因為直到實例的析構函式被呼叫時，實例才會被釋放，所以析構函式可以存取所有請求實例的屬性，並且根據那些屬性可以修改它的行為（比如查找一個需要被關閉的文件的名稱）。

<a name="deinitializers_in_action"></a>
##析構函式操作

這裡是一個析構函式操作的範例。這個範例是一個簡單的遊戲，定義了兩種新型別，`Bank`和`Player`。`Bank`結構管理一個虛擬貨幣的流通，在這個流通中`Bank`永遠不可能擁有超過 10,000 的硬幣。在這個遊戲中有且只能有一個`Bank`存在，因此`Bank`由帶有靜態屬性和靜態方法的結構實作，從而儲存和管理其當前的狀態。

```swift
struct Bank {
    static var coinsInBank = 10_000
    static func vendCoins(var numberOfCoinsToVend: Int) -> Int {
          numberOfCoinsToVend = min(numberOfCoinsToVend, coinsInBank)
          coinsInBank -= numberOfCoinsToVend
         return numberOfCoinsToVend
    }
    static func receiveCoins(coins: Int) {
        coinsInBank += coins
    }
}
```

`Bank`根據它的`coinsInBank`屬性來跟蹤當前它擁有的硬幣數量。銀行還提供兩個方法——`vendCoins`和`receiveCoins`——用來處理硬幣的分發和收集。

`vendCoins`方法在 bank 分發硬幣之前檢查是否有足夠的硬幣。如果沒有足夠多的硬幣，`Bank`回傳一個比請求時小的數字(如果沒有硬幣留在 bank 中就回傳 0)。`vendCoins`方法宣告`numberOfCoinsToVend`為一個變數參數，這樣就可以在方法體的內部修改數字，而不需要定義一個新的變數。`vendCoins`方法回傳一個整型值，表明了提供的硬幣的實際數目。

`receiveCoins`方法只是將 bank 的硬幣儲存和接收到的硬幣數目相加，再保存回 bank。

`Player`類別描述了遊戲中的一個玩家。每一個 player 在任何時刻都有一定數量的硬幣儲存在他們的錢包中。這通過 player 的`coinsInPurse`屬性來體現：

```swift
class Player {
    var coinsInPurse: Int
    init(coins: Int) {
        coinsInPurse = Bank.vendCoins(coins)
    }
    func winCoins(coins: Int) {
        coinsInPurse += Bank.vendCoins(coins)
    }
    deinit {
        Bank.receiveCoins(coinsInPurse)
    }
}
```


每個`Player`實例都由一個指定數目硬幣組成的啟動額度初始化，這些硬幣在 bank 初始化的過程中得到。如果沒有足夠的硬幣可用，`Player`實例可能收到比指定數目少的硬幣。

`Player`類別定義了一個`winCoins`方法，該方法從銀行獲取一定數量的硬幣，並把它們添加到玩家的錢包。`Player`類別還實作了一個析構函式，這個析構函式在`Player`實例釋放前一步被呼叫。這裡析構函式只是將玩家的所有硬幣都回傳給銀行：

```swift
var playerOne: Player? = Player(coins: 100)
println("A new player has joined the game with \(playerOne!.coinsInPurse) coins")
// 輸出 "A new player has joined the game with 100  coins"
println("There are now \(Bank.coinsInBank) coins left   in the bank")
// 輸出 "There are now 9900 coins left in the bank"
```

一個新的`Player`實例隨著一個 100 個硬幣（如果有）的請求而被創建。這`個Player`實例儲存在一個名為`playerOne`的可選`Player`變數中。這裡使用一個可選變數，是因為玩家可以隨時離開遊戲。設置為可選使得你可以跟蹤當前是否有玩家在遊戲中。

因為`playerOne`是可選的，所以由一個感嘆號（`!`）來修飾，每當其`winCoins`方法被呼叫時，`coinsInPurse`屬性被存取並列印出它的預設硬幣數目。

```swift
playerOne!.winCoins(2_000)
println("PlayerOne won 2000 coins & now has \ (playerOne!.coinsInPurse) coins")
// 輸出 "PlayerOne won 2000 coins & now has 2100 coins"
println("The bank now only has \(Bank.coinsInBank) coins left")
// 輸出 "The bank now only has 7900 coins left"
```

這裡，player 已經贏得了 2,000 硬幣。player 的錢包現在有 2,100 硬幣，bank 只剩余 7,900 硬幣。

```swift
playerOne = nil
println("PlayerOne has left the game")
// 輸出 "PlayerOne has left the game"
println("The bank now has \(Bank.coinsInBank) coins")
// 輸出 "The bank now has 10000 coins"
```

玩家現在已經離開了遊戲。這表明是要將可選的`playerOne`變數設置為`nil`，意思是「沒有`Player`實例」。當這種情況發生的時候，`playerOne`變數對`Player`實例的參考被破壞了。沒有其它屬性或者變數參考`Player`實例，因此為了清空它占用的內存從而釋放它。在這發生前一步，其析構函式被自動呼叫，其硬幣被回傳到銀行。
