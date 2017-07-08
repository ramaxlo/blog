+++
draft = false
date = "2017-07-08T13:49:38+08:00"
title = "Install VyOS on APU2"

+++

由於之前[把玩 RouterBoard 的經驗](../transfer-to-routerboard)，讓我還想再找找看有沒有其他軟路由的替代方案。原因主要是因為 RouterOS 雖然也是 Linux based 的系統，但只有 base system 是開源軟體，其他 MikroTik
開發的應用程式仍然還是封閉軟體。雖然它的效能也很不錯，設定上也非常有彈性，也很穩定，但對於一個已經用慣
 FOSS 的人來說，心裡還是有些疙瘩。

在網路上尋尋覓覓一陣子，發現蠻多網友使用 [PC Engines](http://http://pcengines.ch/) 的嵌入式 x86
主板 APU2 上跑 [pfSense](https://www.pfsense.org) 這個 BSD based 的防火牆軟體。看一下硬體規格，有三張 intel
Gb 網卡，價格 125 美金，還可以加購外殼，其他配件加起來大約台幣 5000 出頭吧，比起其他廠牌類似規格的要便宜許多，於是就下單訂了一台。

出貨後等了約莫二個禮拜才送到。在這段期間，我開始思考應該裝什麼系統才好。我不太想裝 pfSense，我想找看看有沒有 Linux based 的軟路由可以安裝；[IPFire](http://www.ipfire.org/) 是其中還蠻有名的開源專案，也是透過 Web 介面來設定，不過感覺好像也沒什麼特別的地方；後來找到了另外一個專案 [VyOS](http://vyos.io)，它有個很特別的東西讓我眼睛一亮：它不提供 Web 介面，所有的設定都必須要透過 CLI 完成；它提供了類似於 Cisco ios 的
shell 來設定網路。稍微看了 wiki 文件後，就決定要來安裝它了。

上網搜尋一下才發現，似乎很少人在 APU2 上面安裝 VyOS，大部分不是裝 pfSense 就是裝 Ubuntu；比較有關的只有[這篇文章][1]，但這個也是安裝在前一代的產品上，並非 APU2；心想如果要裝上去，看來得要和機器奮戰一陣子了。

APU2 這塊板子也很特別，很多市面上賣的 x86 路由器都會提供 VGA port，偏偏 APU2 只提供 serial port，這意味著安裝程式必須要支援 serial console，而非僅僅 graphic console。所幸目前看到的軟路由安裝程式皆已考慮到這點，兩者都有支援，VyOS 自然也支援 serial console。不過它是寫死 9600 baud，然而 APU2 似乎只支援
115200，這會造成開機時無法顯示開機選單，於是我花了不少時間在修改 VyOS iso 映像檔內的 baud rate
設定。

另外一個問題是，不知為何，開機時它無法偵測到我的創見 USB 隨身碟，於是改用 SD 卡開機才得以順利進入登入提示符號。我沒試過其他隨身碟，希望只是個特例。

前前後後大概也花了我二個禮拜，終於被我給裝起來了。雖然只是 AMD G-series 的 CPU，不過做為路由器，效能似乎相當夠用，實測簡單的 SNAT 與基本防火牆規則，內網 iperf 測試可直逼 Gb 理論值，外網頻寬直上 300M
毫不費力；重點是軟體都是 FOSS，真的是相當不錯啊…

詳細的安裝方法我記錄在 [VyOS 的 wiki 頁面][2]上，相信對 VyOS 社群的朋友應該會有幫助；至少有人先幫他們採過雷了 XD

[1]: http://blog.jasonantman.com/2011/09/vyatta-networkos-routerfirewall-on-alix-board-compact-flash/
[2]: https://wiki.vyos.net/wiki/Network_appliances#PC_Engines_APU2C4
