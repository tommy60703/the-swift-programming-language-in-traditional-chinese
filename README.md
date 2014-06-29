《正體中文版蘋果 Swift 官方教學》
=============================================

正體中文版蘋果 Swift 官方教學《The Swift Programming Language》

# 當前階段

簡體中文版已由[numbbbbb]團隊翻譯完成，文字部分已經全部正體中文化，而術語我已經依據 translation.json 裡面的對應完成自動替換成台灣習慣的用法。
第二章以後尚未校稿，歡迎校對，可以隨意提 issue。

# 譯者記錄

* [簡體中文] 

* 歡迎使用 Swift
   * 關於 Swift ([tommy60703])
   * Swift 初見 ([tommy60703])
* Swift 教程
   * 基礎部分 ([tommy60703])
   * 基本運算子 ([tommy60703])
   * 字串和字元 ([tommy60703])
   * 集合型別 ([tommy60703])
   * 控制流程 ([tommy60703])
   * 函式 ([tommy60703])
   * 閉包 ([tommy60703])
   * 列舉 ([tommy60703])
   * 類別和結構 ([tommy60703])
   * 屬性 ([tommy60703])
   * 方法 ([tommy60703])
   * 下標腳本 ([tommy60703])
   * 繼承 ([tommy60703])
   * 建構函式 ([tommy60703])
   * 解構函式 ([tommy60703])
   * 自動引用計數 ([tommy60703])
   * Optional Chaining ([tommy60703])
   * 型別檢查 ([tommy60703])
   * 型別嵌套 ([tommy60703])
   * 擴展 ([tommy60703])
   * 協定 ([tommy60703])
   * 泛型 ([tommy60703])
   * 進階運算子 ([tommy60703])
* 語言參考
   * 關於語言參考 ([tommy60703])
   * 詞法結構 ([tommy60703])
   * 型別 ([tommy60703])
   * 表達式 ([tommy60703])
   * 語句 ([tommy60703])
   * 宣告式 ([tommy60703])
   * 屬性 ([tommy60703])
   * 模式 ([tommy60703])
   * 泛型參數 ([tommy60703])
   * 語法總結 ([tommy60703])

# 貢獻力量

如果想做出貢獻的話，你可以：

- 幫忙校對，挑錯別字、語病等等
- 提出修改建議
- 提出術語翻譯建議

# 翻譯建議

如果你願意一起校對的話，請仔細閱讀：

- 使用 markdown 進行翻譯，文件名必須使用英文，因為中文的話 gitbook 編譯的時候會出問題
- 引號請使用「」和『』
- fork 過去之後新建一個分支進行翻譯，完成後 pull request 這個分支，沒問題的話我會合併到 master 分支中
- 有其他任何問題都歡迎發 issue，我看到了會盡快回覆

謝謝！

# 關於術語

翻譯術語的時候請參考這個流程：

- 盡量保證與台灣習慣術語和已翻譯的內容一致
- 盡量先搜尋，一般來說程式語言的大部分術語是一樣的，可以參考[這個網站](http://jjhou.boolan.com/terms.htm)
- 如果以上兩條都沒有找到合適的結果，請自己決定一個合適的翻譯或者直接使用英文原文，後期校對的時候會進行統一
- 校稿時，若有發現沒有被翻譯成台灣術語的大陸術語，可以將它新增到 translation.json 中
- 可以主動提交替換過的文本給我，或是僅提交新增過的 translation.json 也可，我會再進行文本的替換
- 請務必確定提交的翻譯對照組不會造成字串循環替代（ex: 因為「類」->「類別」，造成下次再執行自動翻譯時「類別」又變成「類別別」）

對翻譯有任何意見都歡迎發 issue，我看到了會盡快回覆

# 參考流程

有些朋友可能不太清楚如何幫忙翻譯，我這裡寫一個簡單的流程，大家可以參考一下：

1. 首先 fork 我的項目
2. 把 fork 過去的項目也就是你的項目 clone 到你的本地
3. 在命令行運行 `git branch develop` 來創建一個新分支
4. 運行 `git checkout develop` 來切換到新分支
5. 運行 `git remote add upstream https://github.com/tommy60703/the-swift-programming-language-in-traditional-chinese` 把我的庫添加為遠端庫
6. 運行 `git remote update`更新
7. 運行 `git fetch upstream master` 拉取我的庫的更新到本地
8. 運行 `git rebase upstream/master` 將我的更新合並到你的分支

這是一個初始化流程，只需要做一遍就行，之後請一直在 develop 分支進行修改。

如果修改過程中我的庫有了更新，請重復 6、7、8 步。

修改之後，首先 push 到你的庫，然後登錄 GitHub，在你的 repo 的首頁可以看到一個 `pull request` 按鈕，點擊它，填寫一些說明資訊，然後提交即可。


# 開源協議
基於[WTFPL](http://en.wikipedia.org/wiki/WTFPL)協議開源。


[簡體中文]:https://github.com/numbbbbb/the-swift-programming-language-in-chinese
[tommy60703]:https://github.com/tommy60703