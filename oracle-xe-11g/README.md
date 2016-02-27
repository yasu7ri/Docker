# Oracle XE 11g のDockerイメージ

## 準備
Oracle XE 11gをインストールするにはswap領域のサイズが2G以上必要なためswap領域の設定を行う必要がある。
ただし、Dockerコンテナ側でswap領域の設定が（セキュリティ上の理由？）のためホストマシン側のswap領域の設定を行う。  
Dockerホストマシン側にログイン`docker-machine ssh`
```
XXXXXXXX:~ xxxxxxx$ docker-machine ssh
                        ##         .
                  ## ## ##        ==
               ## ## ## ## ##    ===
           /"""""""""""""""""\___/ ===
      ~~~ {~~ ~~~~ ~~~ ~~~~ ~~~ ~ /  ===- ~~~
           \______ o           __/
             \    \         __/
              \____\_______/
 _                 _   ____     _            _
| |__   ___   ___ | |_|___ \ __| | ___   ___| | _____ _ __
| '_ \ / _ \ / _ \| __| __) / _` |/ _ \ / __| |/ / _ \ '__|
| |_) | (_) | (_) | |_ / __/ (_| | (_) | (__|   <  __/ |
|_.__/ \___/ \___/ \__|_____\__,_|\___/ \___|_|\_\___|_|
Boot2Docker version 1.10.1, build master : b03e158 - Thu Feb 11 22:34:01 UTC 2016
Docker version 1.10.1, build 9e83765
docker@default:~$
```
`/var/lib/boot2docker/`へ`bootlocal.sh`を作成し以下のコマンド記述しておく。
```
#!/bin/sh

SWAPFILE=/mnt/sda1/swapfile

dd if=/dev/zero of=$SWAPFILE bs=1024 count=2097152
mkswap $SWAPFILE && chmod 600 $SWAPFILE && swapon $SWAPFILE
```
### 参考
- https://github.com/boot2docker/boot2docker/issues/752
- http://stackoverflow.com/questions/31061248/how-to-increase-the-swap-space-available-in-the-boot2docker-virtual-machine


## Dockerイメージの作成&コンテナの起動
```
docker build -t oracle-xe-11g:step1 .
```
イメージが作成されているか確認する`docker images`
```
XXXXXXXX:~ xxxxxxx$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
oracle-xe-11g       step1               dd7f0404387f        11 days ago         2.9 GB
centos              centos7             61b442687d68        9 weeks ago         196.6 MB
```
Dockerコンテナの起動  
`docker run -d -p 8089:8080 -p 1521:1521 --name oracle-xe-11g  oracle-xe-11g:step1`

## Oracle Web Management Console
Dockerホストマシンのipアドレス確認
```
XXXXXXXX:~ xxxxxxx$ docker-machine ip default
xxx.xxx.xxx.xxx
```
Oracle Web Management Console画面URL:`http://xxx.xxx.xxx.xxx:8089/apex/`  
ログイン情報
```
workspace: INTERNAL
user: ADMIN
password: oracle
```
## Oracle XE 11gへの接続情報
```
hostname: localhost
port: 1521
sid: XE
username: system
password: oracle
```

## 参考
- https://github.com/madhead/docker-oracle-xe
