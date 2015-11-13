# 第一步：製作 archlinux 主機 -- 1 到 3 小時

請參考：https://github.com/xsoameix/arch
由於製作 npm 和 gem mirror，根目錄至少 370 G
生成兩個使用者：

1. 自己
2. docker

自己是用來 ssh 進來的身份，docker 是用來開關 container 的使用者

Xorg 不需要裝
Display manager + Desktop environment 不需要裝
Install named 不需要裝

docker 的 systemd service 設定檔不需要調整，檔案位置在：/usr/lib/systemd/system/docker.service，沒事

# 第二步：配置開發平台 -- 15 分鐘

先下載和安裝 dev.rocker 專案

## 下載

從 https://github.com/xsoameix/dev.rocker 下載 zip 檔，解壓縮成 dev 資料夾，

    $ scp -r dev 使用者名稱@主機名稱:~
    $ ssh 使用者名稱
    $ mv dev /tmp
    $ su - # 切換成 root
    $ chown -R docker:docker /tmp/dev
    $ exit
    $ su - docker
    $ mv /tmp/dev ~

## 設定

    $ cd ~/dev

Dockerfile 裡：

    $ groupadd -g 142 dev 改成你的 docker 的 gid
    $ useradd -u 995 -g 142 -m -s /bin/bash dev 改成你的 docker 的 uid 和 gid

執行 https://github.com/xsoameix/dev.rocker README.md 的指令

## 切換目錄

    $ cd

## 製作 image

    $ ~/dev/build

## 進入 container

    $ ~/dev/run

## 設定 git

    $ vim ~/.ssh/id_rsa (按 Ctrl + Shift + V 貼上你的 private key)

你可以 git clone 任何 bitbucket 的專案了

(以下需要用到 git push 時再打)

    $ git config --global user.email 'xxx@xxx.xxx'
    $ git config --global user.name 'your name'

接下來你可以 git push 任何 bitbucket 的專案了

# 第三步：安裝 rocker 引擎 -- 10 分鐘

## 下載

先進入開發平台，執行 git clone git@github.com:xsoameix/rocker.git

## 測試是否已安裝

    $ rocker --help

# 第四步：佈署 containers -- 每個 1 分鐘

## 下載

    $ git clone git@github.com:xsoameix/rock.git

## 察看架構圖

下載 https://github.com/xsoameix/rock/blob/master/arch.dia?raw=true 裡的設計圖 (arch.dia 檔)，並察看

接下來要依照設計圖從底下往上建 containers！

## 啟動 container

    $ rocker setup 專案名稱(eg. rock) 名稱(container name)
    $ rocket restart 專案名稱(eg. rock) 名稱(container name)

## 註解

執行 rocker build ... 之後，\*.tar.xz 是生成 Dockerfile 過程中的產物，不用理他
