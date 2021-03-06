本項では仮想ネットワーク機器であるRouter, Switchを操作するために提供されるCiscoCLIライクなCLIアプリケーションについて説明する。
まず本システムでは、Router, Switchの仮想ネットワーク機器はそれぞれVyOS, Open vSwitchを採用しているがそれらの独自のCLIは利用しない。
理由としては、本システムの開発の背景として、学習者にネットワーク構築演習について学んでもらい、最終目標としてCisco社によって提供されているCCNAの取得を目指してもらうということがあるためである。
そこで、本システムでは、Ciscoネットワーク機器のようなCLIを実現するためのアプリケーションを作成し、学習者に提供することで、学習者はまるでCiscoネットワーク機器を操作しているような感覚で演習を実施可能となる。

作成したCLIアプリケーションについて動画を用いて説明する。
動画ではCLIアプリケーションの大まかな実装内容について説明する。
本アプリケーションはJavaを用いて開発した。

本アプリケーションを起動すると、アプリケーションはコマンドの受付を開始する。
動画の0:00:05のように、学習者は入力コマンドがわからない場合、クエスチョンマーク(?)を入力することで、利用可能なコマンドを確認することができる。
学習者は、コマンドの入力中でも、適時クエスチョンマーク(?)を入力することで、コマンドの候補やパラメータの候補などを確認することが可能である。

また、本アプリケーションを起動したときはユーザはユーザEXECモードという階層にいるが、本アプリケーションではCiscoネットワーク機器と同じく、様々な権限レベルがある。
Router、Switchにおける、各権限レベルの遷移は以下の図のとおりである。


![cli_flow_router](https://user-images.githubusercontent.com/98573303/169831157-c4efb489-789c-4810-bc45-c5ecf352ae72.png)
![cli_flow_switch](https://user-images.githubusercontent.com/98573303/169831175-ef3dfb0c-6bbc-495f-81fa-8d52e2d7630a.png)

ユーザはenable, configure terminal等のコマンドを用いることで適時任意の階層へ遷移し設定を行うことが可能である。
動画では0:00:29からRouterに対して静的ルーティングを設定するためのコマンドであるip routeコマンドの設定を行っている。

動画の0:00:57のようにコマンドの入力が完了すると、コマンドのIDやパラメータ部分を抽出したメッセージが出力され、そのメッセージをもとにサーバ上で、
VyOSやOpen vSwitch用のコマンドに変換し、コマンドの発行が行われる。

例えば、動画の0:00:57の出力メッセージを見ると、Routerのグローバル構成モードのコマンドID: 33が発行されたと判断し、パラメータとして、192.168.4.0, 255.255.255.0, 192.168.1.2が抽出される、そして抽出されたパラメータをもとにコマンドの生成を行う、という流れになる。
