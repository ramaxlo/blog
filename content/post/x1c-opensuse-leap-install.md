---
title: "ThinkPad X1 Carbon openSUSE Leap 安裝"
date: 2017-10-05T23:59:40+08:00
tags:
- x1c
- opensuse
---

[入手 x1c](../x1c-gen5) 之後，接下來的重要步驟就是要安裝系統。不例外，我們就是要在上面安裝 openSUSE Leap。以下小節會詳述安裝時遇到的一些問題與解決方法。

# 調整磁碟分割表為 msdos 類型
由於 x1c 預裝系統是 Windows 10，我沒打算設定雙系統開機，所以我在安裝前先進入 BIOS，把 secure boot 以及 UEFI 關閉，改用
legacy boot。安裝完後發現 BIOS 無法正確地從 MBR 開機。搞了半天才發現，legacy boot 只支援 msdos 類型的分割表，它看不懂
[GPT][1] 類型的分割表。是故又重新分割了一次磁碟才能正常開機。

# 電源管理不正常
x1c 配備的是 i7-7500U，為第七代的 Core i CPU，openSUSE Leap 42.3 的預設 4.4 版核心似乎不能很好地支援這個較新的 CPU，以致於雖然可以正常休眠，但是從休眠狀態回復後再關機，就會發生關機不完全的現象；雖然電源燈是關閉的，但仍能感覺機身並沒有降溫的傾向；再次按電源鍵要開機，卻發現再也開不起來，結果被迫要用迴紋針去戳機背的 reset 鍵才會回復正常。搜尋了網上論壇發現有網友建議在 x1c 上安裝 Linux，最好核心要升級到 4.12 以上，驅動程式的支援才會比較完整。所以就立馬升級核心到最新的 4.13，果然問題就解決了。

原本正常來說，升級核心需要自行下載核心原始碼來編譯；不過 openSUSE 在 [OBS][2] 上已經有 Factory 套件庫可供安裝最新版本的核心，所以只需加入新套件庫後就可以安裝。

升級步驟如下：
~~~bash
sudo zypper ar https://download.opensuse.org/repositories/Kernel:/stable/standard/ kernel-stable
sudo zypper ref kernel-stable
sudo zypper in kernel-default
~~~

# Trackpad 中鍵無作用
也許是因為 x1c 的觸控板是比較新的硬體，openSUSE Leap 42.3 預設核心是 4.4 版，相對來說比較舊了，以至於安裝完後發現 trackpad
的中鍵沒有作用，不過升級完核心後也可以正常運作了。

# Virtualbox 需重新編譯
由於 Virtualbox 需要安裝驅動程式，然而 42.3 提供的驅動程式只能在 4.4 版核心上運作，因此需要自行安裝原始碼套件
(source rpm) 來編譯出可供 4.13 版核心使用的 RPM 套件。

安裝步驟如下：
~~~bash
sudo zypper in -t srcpackage virtualbox
cd /usr/src/packages/SPECS
# Install dependent packages
# ....
sudo rpmbuild -bb virtualbox.spec
cd /usr/src/packages/RPMS/x86_64
sudo zypper in virtualbox*.rpm
~~~

[1]: https://zh.wikipedia.org/zh-tw/GUID%E7%A3%81%E7%A2%9F%E5%88%86%E5%89%B2%E8%A1%A8
[2]: https://build.opensuse.org/project/show/Kernel:stable
