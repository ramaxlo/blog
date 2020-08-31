---
title: "入手 ThinkPad T14s AMD Gen1"
date: 2020-08-31T14:12:14+08:00
tags:
- t14s
- lenovo
---

自從入手 T450 使用到現在也已過了五年 (時間真的過得很快)，除了在保固內換過一次硬碟外，幾乎沒再出過什麼問題。看著每年一代又一代的 T 系列推陳出新，原本是打算再戰個
2~3 年再換的，不過由於今年發布的[消息][1]說將會出搭載 AMD Ryzen Pro 4000 系列的 T 系列，心中真是一陣雀躍。接著等到了今年下半年才在官網上架。搭載 Ryzen 7、16G ram、512G SSD 的機型大約 37000 出頭，足足比對應的 intel
版本要便宜個 8~9k，心想這真是香啊，於是索性就下訂了。

等了約 3 週到貨後，立馬安裝 openSUSE Tumbleweed，安裝過程就不多加贅述。雖然是滾動版本，但安裝過程其實相當平順，沒有什麼大問題，檔案系統也從過去習慣的
LVM + ext4 改成全面使用 [Btrfs][2]。安裝完成後，大部分的硬體都可以偵測到並加以驅動。當然畢竟還是相當新的硬體，仍然會有一些小問題發生，在此條列如下：

* 有線網路 (rtl8168) 可以正常驅動，然而在插入網路線的時候，驅動程式似乎無法偵測網路線插入。
  必須要先進休眠狀態後，插入網路線，再喚醒才能偵測。
* 由於目前 Tumbleweed 使用的核心版本為 5.8.0，Virtualbox 6.1.12 無法在該版本上正常運作。詳情見
  [Bugzilla][3] 的討論。
* FnLock 的燈號不會根據目前狀態亮滅
* 無法正常休眠，也就是休眠後，燈號不會轉成呼吸燈，CPU 也還在運轉狀態。後來在[論壇][4]上找到解法，必須先進
  bios 中的 config -> power -> sleep state 設定，將 Windows10 改為 Linux。

最後花了幾天時間將在舊機器的資料搬移過來，就開始做為正式的工作機了。相信上述的小問題會因為驅動程式的陸續更新而慢慢解決。

[1]: https://www.techbang.com/posts/78332-amd-launches-ryzen-4000-series-pro-mobile-processor-lenovo-thinkpad-goes-first
[2]: https://btrfs.wiki.kernel.org/index.php/Main_Page
[3]: https://bugzilla.opensuse.org/show_bug.cgi?id=1175201
[4]: https://forums.lenovo.com/t5/Ubuntu/Lenovo-Ideapad-Slim-7-AMD-Ryzen-7-4700U-suspend-resume-issues/m-p/5019372?page=2
