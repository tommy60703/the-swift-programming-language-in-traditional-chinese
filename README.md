《The Swift Programming Language》in Traditional Chinese
=============================================

繁體中文版 Apple 官方 Swift 教學《The Swift Programming Language》

# 當前階段

簡體中文版已由[numbbbbb]團隊翻譯完成，目前正在進行繁體中文化，包含大陸術語翻譯成台灣術語，歡迎校對，可以隨意提issue和pr。

# 譯者記錄

* 歡迎使用 Swift
   * 關於 Swift ([numbbbbb])
   * Swift 初見 ([numbbbbb])
* Swift 教程
   * 基礎部分 ([numbbbbb], [lyuka], [JaySurplus])
   * 基本操作符 ([xielingwang])
   * 字符串和字符 ([wh1100717])
   * 集合類型 ([zqp])
   * 控制流 ([vclwei], [coverxit], [NicePiao])
   * 函數 ([honghaoz])
   * 閉包 ([wh1100717])
   * 枚舉 ([yankuangshi])
   * 類和結構體 ([JaySurplus])
   * 屬性 ([shinyzhu])
   * 方法 ([pp-prog])
   * 下標 ([siemenliu])
   * 繼承 ([Hawstein])
   * 構造過程 ([lifedim])
   * 析構過程 ([bruce0505])
   * 自動引用計數 ([TimothyYe])
   * 可選鏈 ([Jasonbroker])
   * 類型檢查 ([xiehurricane])
   * 嵌套類型 ([Lin-H])
   * 擴展 ([lyuka])
   * 協議 ([geek5nan])
   * 泛型 ([takalard])
   * 高級操作符 ([xielingwang])
* 語言參考
   * 關於語言參考 ([dabing1022])
   * 詞法結構 ([superkam])
   * 類型 ([lyuka])
   * 表達式 ([sg552] )
   * 語句 ([coverxit])
   * 聲明 ([marsprince])
   * 特性 ([Hawstein])
   * 模式 ([honghaoz])
   * 泛型參數 ([fd5788])
   * 語法總結 ([StanZhai])

# 貢獻力量

如果想做出貢獻的話，你可以：

- 幫忙校對，挑錯別字、語病等等
- 提出修改建議
- 提出術語翻譯建議

# 翻譯建議

如果你願意一起校對的話，請仔細閱讀：

- 使用 markdown 進行翻譯，文件名必須使用英文，因為中文的話 gitbook 編譯的時候會出問題
- 翻譯後的文檔請放到 source 文件夾下的對應章節中，然後 pull request 即可，我會用 gitbook 編譯成網頁
- 工作分支為 gh-pages，用於 GitHub 的 pages 服務
- fork 過去之後新建一個分支進行翻譯，完成後 pull request 這個分支，沒問題的話我會合併到 gh-pages 分支中
- 有其他任何問題都歡迎發 issue，我看到了會盡快回覆

謝謝！

# 關於術語

翻譯術語的時候請參考這個流程：

- 盡量保證與台灣習慣用語和已翻譯的內容一致
- 盡量先搜尋，一般來說程式語言的大部分術語是一樣的，可以參考[這個網站](http://jjhou.boolan.com/terms.htm)
- 如果以上兩條都沒有找到合適的結果，請自己決定一個合適的翻譯或者直接使用英文原文，後期校對的時候會進行統一

# 參考流程

有些朋友可能不太清楚如何幫忙翻譯，我這裡寫一個簡單的流程，大家可以參考一下：

1. 首先 fork 我的項目
2. 把 fork 過去的項目也就是你的項目 clone 到你的本地
3. 在命令行運行 `git branch develop` 來創建一個新分支
4. 運行 `git checkout develop` 來切換到新分支
5. 運行 `git remote add upstream https://github.com/numbbbbb/the-swift-programming-language-in-chinese.git` 把我的庫添加為遠端庫
6. 運行 `git remote update`更新
7. 運行 `git fetch upstream gh-pages` 拉取我的庫的更新到本地
8. 運行 `git rebase upstream/gh-pages` 將我的更新合並到你的分支

這是一個初始化流程，只需要做一遍就行，之後請一直在 develop 分支進行修改。

如果修改過程中我的庫有了更新，請重復 6、7、8 步。

修改之後，首先 push 到你的庫，然後登錄 GitHub，在你的庫的首頁可以看到一個 `pull request` 按鈕，點擊它，填寫一些說明信息，然後提交即可。


# 開源協議
基於[WTFPL](http://en.wikipedia.org/wiki/WTFPL)協議開源。



[numbbbbb]:https://github.com/numbbbbb
[stanzhai]:https://github.com/stanzhai
[coverxit]:https://github.com/coverxit
[wh1100717]:https://github.com/wh1100717
[TimothyYe]:https://github.com/TimothyYe
[honghaoz]:https://github.com/honghaoz
[lyuka]:https://github.com/lyuka
[JaySurplus]:https://github.com/JaySurplus
[Hawstein]:https://github.com/Hawstein
[geek5nan]:https://github.com/geek5nan
[yankuangshi]:https://github.com/yankuangshi
[xielingwang]:https://github.com/xielingwang
[yulingtianxia]:https://github.com/yulingtianxia
[twlkyao]:https://github.com/twlkyao
[dabing1022]:https://github.com/dabing1022
[vclwei]:https://github.com/vclwei
[fd5788]:https://github.com/fd5788
[siemenliu]:https://github.com/siemenliu
[youkugems]:https://github.com/youkugems
[haolloyin]:https://github.com/haolloyin
[wxstars]:https://github.com/wxstars
[IceskYsl]:https://github.com/IceskYsl
[sg552]:https://github.com/sg552
[superkam]:https://github.com/superkam
[zac1st1k]:https://github.com/zac1st1k
[bzsy]:https://github.com/bzsy
[pyanfield]:https://github.com/pyanfield
[ericzyh]:https://github.com/ericzyh
[peiyucn]:https://github.com/peiyucn
[sunfiled]:https://github.com/sunfiled
[lzw120]:https://github.com/lzw120
[viztor]:https://github.com/viztor
[wongzigii]:https://github.com/wongzigii
[umcsdon]:https://github.com/umcsdon
[zq54zquan]:https://github.com/zq54zquan
[xiehurricane]:https://github.com/xiehurricane
[Jasonbroker]:https://github.com/Jasonbroker
[tualatrix]:https://github.com/tualatrix
[pp-prog]:https://github.com/pp-prog
[088haizi]:https://github.com/088haizi
[baocaixiong]:https://github.com/baocaixiong
[yeahdongcn]:https://github.com/yeahdongcn
[shinyzhu]:https://github.com/shinyzhu
[lslxdx]:https://github.com/lslxdx
[Evilcome]:https://github.com/Evilcome
[zqp]:https://github.com/zqp
[NicePiao]:https://github.com/NicePiao
[LunaticM]:https://github.com/LunaticM
[menlongsheng]:https://github.com/menlongsheng
[lifedim]:https://github.com/lifedim
[happyming]:https://github.com/happyming
[bruce0505]:https://github.com/bruce0505
[Lin-H]:https://github.com/Lin-H
[takalard]:https://github.com/takalard
[dabing1022]:https://github.com/dabing1022
[marsprince]:https://github.com/marsprince