本項では、課題演習機能について説明する。
本システムには、学習者が自分でネットワーク機器を配置し、ネットワーク構築演習を実施可能な自由演習モードがあるが、学習者のレベルにより学習者が自分で演習を作成し、ネットワーク構築演習を実施することが困難である場合が考えられる。
そこで学習者への負担を軽減するためにあらかじめ作成された演習を実施することができる機能を作成した。
課題演習機能では、指定されたネットワークを学習者が正しく構築できているかの判定や、課題情報を適時確認することが可能である。
本システムで用いる課題は、教師などの教育者が作成する.

動画を用いて説明する。
まず初めに、学習者は動画の0:00:04のように自由演習モードではなく課題演習モードを選択する。
すると、下記の図のように実施可能な演習が表示される。
![image](https://user-images.githubusercontent.com/98573303/169848713-b8e2bc82-a9d5-440f-acdc-540d8144d891.png)

今回は、VLAN構築演習を例に説明する。
学習者が課題を選択すると、選択した課題に適したネットワーク機器が結線の処理まで完了した状態で生成される。
学習者は生成されたネットワーク機器に対して設定を発行することが可能である。

動画の0:00:24で表示している演習情報タブでは、下記の図のように課題の内容を適時確認することが可能である。
![image](https://user-images.githubusercontent.com/98573303/169848887-7bd5da23-37f2-405d-af3c-859b536f884b.png)

動画の0:00:31で表示している採点情報タブでは、下記の図のように学習者がネットワーク機器に対して施した設定が正しいかどうかを判定する。
![image](https://user-images.githubusercontent.com/98573303/169848958-c6ceb061-8911-4a1f-ac35-5de02737e5f7.png)

判定方法としては、予め用意した演習ファイルから生成したネットワーク機器と学習者が設定したネットワーク機器の設定を比較することで正誤判定する。

動画の0:00:37のように学習者はAuto Scoringボタンを押下することで、適時採点情報を更新することが可能である。

例えば、動画の0:00:55ではHost0にIPアドレスを設定しているが、設定が完了した後、Auto Scoringボタンを押下し採点情報が更新されていることがわかる。今回は、正しいIPアドレスを入力したため、×となっていた部分がチェックマークに変わったことが確認できる。

Switchも同様に設定を施し、適時採点情報を確認することが可能であり、最終的にすべての設定が完了した後、Auto Scoringボタンを押下することで、動画の0:02:03のように画面にコンプリートと表示され演習が完了となる。

上記のような流れで学習者は演習を実施可能となる。



参考として、動画で利用した課題演習に利用するファイル(XMLファイル)を下記に示しておく。
```xml:VLANの演習ファイル
<?xml version="1.0" encoding="UTF-8"?>

<vns edition="web" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="">
	<line ifdst="Host0" ifdst_port="eth0" ifsrc="Switch0" ifsrc_port="eth0"></line>
	<line ifdst="Host1" ifdst_port="eth0" ifsrc="Switch1" ifsrc_port="eth0"></line>
	<line ifdst="Switch1" ifdst_port="eth1" ifsrc="Switch0" ifsrc_port="eth1"></line>
	<vm name="Host0" position_x="221" position_y="274">
		<host name="Host0">
			<if mac="56:4e:53:a6:b5:5e" name="eth0" status="------">
				<ip address="192.168.1.1" netmask="255.255.255.0"></ip>
			</if>
		</host>
	</vm>
	<vm name="Host1" position_x="358" position_y="291">
		<host name="Host1">
			<if mac="56:4e:53:b2:91:92" name="eth0" status="------">
				<ip address="192.168.1.2" netmask="255.255.255.0"></ip>
			</if>
		</host>
	</vm>
	<vm name="Switch0" position_x="191" position_y="122">
		<switch name="Switch0">
			<hostname>Switch0</hostname>
			<if mac="56:4e:53:e:44:72" name="eth0" status="------">
				<ip address=" " netmask=" "></ip>
				<switchport>Access_Port</switchport>
				<vlan vlan_id="10"></vlan>
			</if>
			<if mac="56:4e:53:c6:e9:3e" name="eth1" status="------">
				<ip address=" " netmask=" "></ip>
				<switchport>Trunk_Port</switchport>
				<vlan allowed_vlan="1,10,20" vlan_id="0"></vlan>
			</if>
		</switch>
	</vm>
	<vm name="Switch1" position_x="346" position_y="130">
		<switch name="Switch1">
			<hostname>Switch1</hostname>
			<if mac="56:4e:53:35:dd:a5" name="eth0" status="------">
				<ip address=" " netmask=" "></ip>
				<switchport>Access_Port</switchport>
				<vlan vlan_id="20"></vlan>
			</if>
			<if mac="56:4e:53:53:76:e9" name="eth1" status="------">
				<ip address=" " netmask=" "></ip>
				<switchport>Trunk_Port</switchport>
				<vlan allowed_vlan="1,10,20" vlan_id="0"></vlan>
			</if>
		</switch>
	</vm>
	<explain>S2H2 VLAN演習</explain>
	<content> 別のVLAN上に存在するHost0とHost1同士の疎通について学習しよう. 
	VLANの設定をした後にトランクリンクの設定をしよう.
	--アドレステーブル--
	Host0   eth0 192.168.1.1/24
	Host1   eth0 192.168.1.2/24
	Switch0 eth0 --------------
	        eth1 --------------
	Switch1 eth0 --------------
	        eth1 --------------

	--STEP1--
	アドレステーブルに則って各ネットワーク機器にIPアドレスを設定しよう.
	--STEP2--
	Hostとの接続を実施しているSwitchの各ポートにVLANを設定しよう.
	--STEP3--
	Switch同士を接続している各ポートにトランクリンクを設定しよう.
	・許可するVLAN: 1,10,20
	--STEP4--
	・Host0からHost1への疎通をpingコマンドで確認しよう.
	・Host1からHost0への疎通をpingコマンドで確認しよう. </content>
	<number>5</number>
	<check_action>
	<action nodename="Host0" command="ping 192.168.1.2"/>
	<action nodename="Host1" command="ping 192.168.1.1"/>
</check_action>
</vns>

```
