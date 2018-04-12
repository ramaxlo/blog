---
title: "openSUSE Leap 42.3 AMI 上架"
date: 2018-04-13T00:36:25+08:00
---

自從 openSUSE 將 42.2 的 AMI 放上 [AWS Marketplace][1] 後，就很久沒在更新了，害我想用 42.3 都還要自己用
[Kiwi][2] 建立映象檔再上傳到 AWS 上。不過最近終於[把 42.3 給放上來了][3]，趁著最近要開新機器，就來試用一下。

要啟動執行 openSUSE 的 instance 其實就跟執行 Ubuntu 的 instance 沒什麼兩樣，差別在於選擇 AMI
時要在 *Community AMI* 中選擇 openSUSE AMI。另外的差別就是預設帳號是 ec2-user，而不是大家習慣的
ubuntu。

實際使用起來並沒有什麼問題，這個 AMI 還很貼心地幫你把 [Cloud:Tools][4]
套件庫預先設定好，一些常用的工具如
aws-cli、google-cloud-sdk、azure-cli 等都可以直接安裝，不需要自行加入套件庫或是下載個別套件再安裝了，相當方便。想跑
docker？套件庫中已經直接提供 v17.09.1 了，不是最新的，但也算很新的了…

[1]: https://news.opensuse.org/2017/02/01/opensuse-cloud-images-are-ripe-for-users/
[2]: https://github.com/SUSE/kiwi
[3]: https://aws.amazon.com/marketplace/pp/B01N4R3GJI?qid=1523547208339&sr=0-1&ref_=srh_res_product_title
[4]: https://build.opensuse.org/project/show/Cloud:Tools
