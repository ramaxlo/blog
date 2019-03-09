---
title: "使用 APU2 架設家庭路由器"
date: 2019-03-09T22:21:19+08:00
draft: false
tags:
- apu2
- vyos
---

有鑑於家中網路使用的頻寬分享器主要是由 ISP 所提供，其設備老舊，缺乏韌體更新，也缺少一些更進階的設定，讓我覺得該是時候將路由器的角色改由我最近把玩的 APU2 來代替，而原本的設備則單純做為
PPPoE relay 的角色。因此我將家中網路架構做了改變如下：

* 使用 APU2 透過 PPPoE 撥接至 ISP，並做為對外網路的唯一介面
* LAN 端電腦透過 switch 接至 APU2 的 LAN port
* 將原本設備的 virtual server 設定搬移至 APU2
* 架設 [Wireguard][1] VPN，並關閉對外的 web 與 sftp port
* 啟動 DNS forwarder
* 設定 zone based firewall，讓防火牆設定更容易管理

當然在轉換過程中也碰到了一些之前沒遇到的問題：

* 由於 PPPoE header 會佔用 8 byte 的長度，因此 TCP MSS ([Maximum segmemnt size][2]) 必須要減掉
  8 byte，否則封包會傳不出去。詳情可見[這個文章][3]。VyOS 的設定[在此][4]。
* 原本想設定路由器作為 wifi ap，然而測試後發現只能設定為 11n 模式，且速率並不是很理想，另外還有相容性的問題，因此還是決定使用現成的 wifi ap 來架設無線網路。

架設完成後，使用上並不會感覺到網路速度有受到什麼影響，不過現在網路設定更容易維護，且路由軟體也可以很容易地升級
(畢竟是開放源碼系統)，算是一舉數得。


[1]: https://www.wireguard.com/
[2]: https://en.wikipedia.org/wiki/Maximum_segment_size
[3]: https://samuel.kadolph.com/2015/02/mtu-and-tcp-mss-when-using-pppoe-2/
[4]: https://wiki.vyos.net/wiki/PPPoE
