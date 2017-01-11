+++
title = "Add M.2 SSD For T450"
draft = false
date = "2017-01-11T23:58:08+08:00"

+++

2016 年的最後一天，大家都要去跨年，但是我沒去。在這一年將要結束的這一天，我決定要幫我的 Lenovo T450
擴充硬碟空間，並且要在安裝後還能夠恢復原來的工作環境。這是一個大工程，值得我用一天的時間好好地搞一搞。

根據 Lenovo 的[硬體維護手冊](https://download.lenovo.com/pccbbs/mobiles_pdf/t450_hmm_en_sp40a27225.pdf)，T450
有二個 M.2 插槽；其中一個已被 Wifi 網卡使用，另一個則原本是供 WWAN 使用，但若未購買搭 WWAN
的機器，就會多出一個插槽可供利用。另外需要注意的是，M.2 介面的 SSD 有二種通訊協定規格，一個是
SATA，另一個是 PCIe，並不是所有的筆電都支援這二種協定，故購買前一定要先上網查清楚，以免白花錢。又還有一點要注意，就算你確定了通訊協定規格，也不是所有廠商的 SSD 都可以和你的筆電相容，多查查網上的討論串，儘可能買有成功案例的廠牌會比較保險些。總之 M.2 SSD 就是這麼麻煩的東西。

有興趣的人可以參考這個[連結](http://www.tpuser.idv.tw/wp/?p=2647)了解更多關於 M.2 SSD 的細節。

好啦，買好了 SSD，也順利地安裝上去（硬體安裝步驟略過不提），打開筆電開關，BIOS 也順利抓到了硬體，接下來才是工程的主要部分：搬移系統檔案，設定以 SSD 為開機硬碟。以下將大略走一下執行步驟。

# 準備工作
*   下載 [openSUSE 開機映象檔](https://software.opensuse.org)，寫入到 USB 隨身碟中：

    ~~~bash
    sudo dd if=openSUSE-Leap-42.2-DVD-x86_64.iso of=/dev/sdX bs=1M
    ~~~

    當然你也可以用其他的 Linux 開機映象檔，只要有具備救援模式功能即可。

*   備份你的系統。BJ4

# 開始搬移
首先用 USB 隨身碟開機，選擇救援模式，進入到命令列模式，然後分割 SSD 並且格式化：

~~~bash
fdisk /dev/sdX
mkfs.ext4 /dev/sdX1
~~~

接著將原系統磁區掛載到 `/tmp/root`, SSD 磁區掛載到 `/tmp/root2`：

~~~bash
mkdir /tmp/root
mkdir /tmp/root2
mount /dev/sdY1 /tmp/root
mount /dev/sdX1 /tmp/root2
~~~

開始複製檔案：

~~~bash
rsync -auv --exclude="lost+found" /tmp/root/ /tmp/root2
~~~

複製的時間可能會很久，端看你的資料量而定。

# 更新系統設定檔
最重要的檔案就是 `/etc/fstab`，這個檔案控制了使用那一個磁區來掛載 root 檔案系統，故當你搬移完系統檔案後，一定要記得更新這個檔案。

原來的檔案內容如下：

~~~bash
UUID=dedaf79c-24e4-468f-a03c-7616cdfa8385 swap          swap       defaults              0 0
UUID=e9912870-b350-4040-9ed3-386a4a5f849e /             ext4       acl,user_xattr        1 1
UUID=77ec7af0-cc2c-44a5-8bc3-6c7913273ab6 /home         ext4       acl,user_xattr        1 2
~~~

更新為：

~~~bash
UUID=dedaf79c-24e4-468f-a03c-7616cdfa8385 swap          swap       defaults              0 0
UUID=cc3aa415-2c1f-43bd-a810-e0cf3d1614be /             ext4       acl,user_xattr,discard 1 1
UUID=77ec7af0-cc2c-44a5-8bc3-6c7913273ab6 /home         ext4       acl,user_xattr        1 2
none                                      /tmp          tmpfs      defaults              0 0
~~~

注意我這邊使用的是檔案系統的 UUID 來掛載磁區，`/` 的掛載磁區換成了 SSD，並且加上了 `discard`
參數；另外使用 tmpfs 來掛載 `/tmp` 目錄，以避免大量的暫存檔案存在 SSD，降低 SSD 的壽命。

檔案系統的 UUID 可用指令 `ls -l /dev/disk/by-uuid` 查詢。

# 安裝開機啟動程式 (bootloader)
首先要 chroot 到新的系統目錄：

~~~bash
mount --bind /dev /tmp/root2/dev
mount --bind /sys /tmp/root2/sys
mount --bind /proc /tmp/root2/proc
chroot /tmp/root2 /bin/bash
~~~

更新 GRUB 設定檔：

~~~bash
grub2-mkconfig -o /boot/grub2/grub.cfg
~~~

安裝開機啟動程式到 SSD 的開機啟動磁區 (MBR)：

~~~bash
grub2-install /dev/sdX
~~~

退出 chroot 環境：

~~~bash
exit
~~~

卸載目錄：

~~~bash
umount /tmp/root2/dev
umount /tmp/root2/sys
umount /tmp/root2/proc
umount /tmp/root2
umount /tmp/root
~~~

重新啟動電腦：

~~~bash
reboot
~~~

然後進入 BIOS，設定使用 SSD 做為開機硬碟，打完收工。
