---
title: "轉移到 Gemini capsule"
date: 2021-03-03T23:09:21+08:00
draft: false
---

最近發現到 [Gemini][1] 這個可說是返樸歸真的 web protocol 讓人覺得很有意思。它有幾個特性：

* 不支援 style sheet
* 不支援 scripting
* 僅支援有限的類 markdown 語法
* 強迫使用 TLS
* 伺服器除了回傳狀態值與 MIME 類型在標頭，不允許回傳或要求其他資訊

如此極為限制的 protocol，使得現代 web 中常使用的 javascript、cookie、session
等技術變得不可行。我覺得這非常適合像我這樣偶爾寫寫 blog 的人，不用再擔心排版、或是使用靜態網頁產生器的問題；反正就照著固定的格式寫，render 的部分就讓瀏覽器去傷腦筋吧…

是故最近決定把現在在 github page 上的文章都搬去 Gemini capsule，而這裡就不再更新了。新站台[在此][2]。


[1]: https://en.wikipedia.org/wiki/Gemini_(protocol)
[2]: https://ramax.flounder.online
