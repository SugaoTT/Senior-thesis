本項では、ネットワーク機器の一つであるSwitchの動作について説明していく。
本システムでは仮想Switchを動作させるためにソフトウェアSwitchであるOpen vSwitchを利用している。

また、仮想Switchを動作させるだけでなく、コンテナ間の結線にもOpen vSwitchを使用しており、
例えば、以下のような図を考えた場合、RouterとHost間の結線をするためにOpen vSwitchによる仮想Bridgeが生成され各コンテナのNIC同士が接続される。

![image](https://user-images.githubusercontent.com/98573303/169823456-62519c7f-0ec1-4d2e-a4ad-6495b8467d9e.png)

サーバ上でOpen vSwitchの仮想Bridgeの状態を確認していく、以下がRouterとHostを接続するために生成されたOpen vSwitchの仮想Bridgeとなっており、Bridge名はRouterN(Routerの番号)ToHostN(Hostの番号)となる。
赤枠で囲った部分がコンテナのNICを指しており、上側のポートがRouterのNIC、下側のポートがHostのNICとなっている。
![image](https://user-images.githubusercontent.com/98573303/169823907-4bef4bcc-34aa-4918-a135-b05b5d30b6e3.png)

次に、仮想Switchの生成過程についてみていく。
仮想Switchをネットワークトポロジに追加するために以下の図の赤枠で囲ったSwitchをネットワークトポロジにドラッグ&ドロップすることで仮想Switchが生成される。
![image](https://user-images.githubusercontent.com/98573303/169824512-0004f30d-5b9e-461e-9d4b-c0a18c3f4da9.png)

仮想Switchが生成されるとネットワークトポロジの画面は以下の図のようになる。
![image](https://user-images.githubusercontent.com/98573303/169824822-cc8dd61b-eb03-42f3-bb14-05c9db81c3ee.png)

また、サーバ上でOpen vSwitchの仮想Bridgeの状態を確認すると、Switch0が生成されており、仮想Switchが生成されたことがわかる。

では、次に生成した仮想Switchへのコマンド発行手順についてみていく。
まず、動画の0:00:1からわかるように、予めSwitchとHostを一台ずつ接続し用意した。
今回は例として、SwitchにVLANの設定を施した。
動画の0:00:30からわかるように、Switchのeth0ポートにアクセスポートとしてVLAN10を設定していることがわかる。

次に、サーバ上でOpen vSwitchの仮想Bridgeの状態を確認していく。
Switch0のPort "84187649f5184_l"がHostと接続されているポート(eth0)を指しており、そのポートに対してVLANタグ10が設定されていることが確認できる。

以上のようにして、Switchに対して設定が施される。
