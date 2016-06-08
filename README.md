# Docker

## docker-machineのセットアップ
### homebrewでdocker-machineをインストールする  
`brew install docker-machine`
### Dockerホストマシンの作成  
`docker-machine create -d vmwarefusion default`
```
  XXXXXXXX:~ xxxxxxx$ docker-machine ls
  NAME      ACTIVE   DRIVER         STATE     URL   SWARM   DOCKER    ERRORS
  default   -        vmwarefusion   Stopped                 Unknown
```
### Dockerホストマシンの起動  
`docker-machine start default`
```
XXXXXXXX:~ xxxxxxx$ docker-machine start default
Starting "default"...
Machine "default" was started.
Waiting for SSH to be available...
Detecting the provisioner...
Started machines may have new IP addresses. You may need to re-run the `docker-machine env` command.
XXXXXXXX:~ xxxxxxx$
```
### ローカルターミナルからホストに直接dockerコマンドを実行するために環境変数の設定を行う  
`eval "$(docker-machine env default)"`  
毎回このコンマンドをタイプするのが面倒なので関数を作成しておく
```sh
  function docker-machine-env(){
    if [ $# != 1 ]; then
      eval $(docker-machine env default)
    elif [ $1 = "show" ]; then
      echo $DOCKER_MACHINE_NAME
    else
      eval $(docker-machine env $1)
    fi
  }
```
`docker-machine-env default`
### dockerイメージの取得
```
  docker pull centos:latest
  docker pull centos:centos7
  docker pull centos:centos6
```
### dockerコンテナーの実行  
`docker run`  
主な[オプション]
  - -d：バックグラウンドで実行する。
  - -i：コンテナーの標準入力を開く。/bin/bashなどでコンテナーを操作する際に指定
  - -t：tty（端末デバイス）を確保する。/bin/bashなどでコンテナーを操作する際に指定
  - -p ｛ホストのポート番号｝:｛コンテナーのポート番号｝：Dockerサーバーのホストとポートマッピングを構成
  - --name:｛コンテナ名｝

## 参考
- http://www.bunkei-programmer.net/entry/2015/11/28/232308
- http://qiita.com/takiuchi/items/168c3b2042e467898f91
- https://teratail.com/questions/19477
