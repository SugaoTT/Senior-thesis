本項ではネットワーク機器のデータ保存機能について説明する。
時に演習を中断しなければならない場面が考えられる。
その際に、演習状況を保存し次回演習をする時に手早く再開できるようにするための機能としてデータ保存機能がある。

まず初めに動画の前半部分では、以下の画像のようにHost2台とSwitch2台を用意し、左側のSwitch・HostはVLAN10、右側のSwitch・HostはVLAN20といった風にVLANを用いてセグメントを分割している。
![image](https://user-images.githubusercontent.com/98573303/169842435-2ee0e4eb-b044-4f55-b421-231ff2367977.png)

今回はSwitchのデータ保存について説明する。
動画のようなネットワークを構築した場合、Open vSwitchによる仮想bridgeは以下のようになる。
以下の画像のSwitch4がアプリケーション上のSwitch0のことを指しており、Switch5がSwitch1を指している。
画像より、Hostと接続しているポートに関してはVLANタグ10または20が設定されていることがわかる。
また、Switch間を接続しているポートに関しては、トランクリンク上で許可するVLANタグが[1, 10, 20]に設定されていることがわかる。
![image](https://user-images.githubusercontent.com/98573303/169843139-3d699a5e-48be-420b-ab05-194c1165c2dc.png)

本システムでは、データ保存機能を用いてこのような設定を復元する必要がある。
データを保存するためにXMLファイルを用いる。

データ保存機能で用いるXMLファイルは下記の図のようになる。
まず、図の上記の赤枠の部分でネットワークトポロジを復元するためのデータが保存されている。
下記の画像では、Host0のeth0ポートとSwitch0のeth0ポートが接続されていることが確認できる。
図の下部の青枠の部分に関しては、ネットワーク機器の詳細なデータが保存されている。
下記の画像では、Host0のネットワークトポロジ上のx, y座標や、macアドレス、インタフェースeth0に設定されたIPアドレスなどの設定が保存されていることがわかる。
![image](https://user-images.githubusercontent.com/98573303/169844095-4b140ece-f645-4644-b389-be42a4c8b005.png)

本システムでは、動画の0:02:04のように画面左側のSaveボタンを押下し、ファイル名を指定し、OKボタンを押下することでXMLファイルの出力がされる。
データを読み込むときは動画の0:02:29のように画面左側のLoadボタンを押下し、ファイル名を選択しOKボタンを押下することで、ネットワーク機器の復元が開始される。
読み込みが完了したら、Switchの状態が正しく復元されているかを確認する。
動画の0:02:51で、下記の図のように新たにSwitch6とSwitch7が生成されていることが確認できる、これは新しく生成した仮想ネットワーク機器のSwitch0とSwitch1を指しており、
赤枠で囲っている部分から、Switch4とSwitch5のデータが復元されており、データの読み込みが完了したことが確認できる。
![image](https://user-images.githubusercontent.com/98573303/169845634-b0f84eba-5cc7-4a49-bfad-5c7e712e624c.png)

以上のような流れでデータの保存・読み込みが行われる。

参考として動画で保存したネットワークのファイル(XMLファイル)を下記に示す。

```xml:VLAN
<?xml version="1.0" encoding="UTF-8"?>

<vns edition="web" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="">
	<line ifdst="Host0" ifdst_port="eth0" ifsrc="Switch0" ifsrc_port="eth0"></line>
	<line ifdst="Host1" ifdst_port="eth0" ifsrc="Switch1" ifsrc_port="eth0"></line>
	<line ifdst="Switch1" ifdst_port="eth1" ifsrc="Switch0" ifsrc_port="eth1"></line>
	<vm name="Host0" position_x="184" position_y="270">
		<host name="Host0">
			<if mac="56:4e:53:89:26:38" name="eth0" status="------">
				<ip address="192.168.1.1" netmask="255.255.255.0"></ip>
			</if>
		</host>
	</vm>
	<vm name="Host1" position_x="380" position_y="283">
		<host name="Host1">
			<if mac="56:4e:53:15:c7:e5" name="eth0" status="------">
				<ip address="192.168.1.2" netmask="255.255.255.0"></ip>
			</if>
		</host>
	</vm>
	<vm name="Switch0" position_x="167" position_y="120">
		<switch name="Switch0">
			<hostname>Switch0</hostname>
			<if mac="56:4e:53:ba:bf:c6" name="eth0" status="------">
				<ip address=" " netmask=" "></ip>
				<switchport>Access_Port</switchport>
				<vlan vlan_id="10"></vlan>
			</if>
			<if mac="56:4e:53:14:45:da" name="eth1" status="------">
				<ip address=" " netmask=" "></ip>
				<switchport>Trunk_Port</switchport>
				<vlan allowed_vlan="1,10,20" vlan_id="0"></vlan>
			</if>
		</switch>
	</vm>
	<vm name="Switch1" position_x="370" position_y="141">
		<switch name="Switch1">
			<hostname>Switch1</hostname>
			<if mac="56:4e:53:78:50:a6" name="eth0" status="------">
				<ip address=" " netmask=" "></ip>
				<switchport>Access_Port</switchport>
				<vlan vlan_id="20"></vlan>
			</if>
			<if mac="56:4e:53:53:3e:20" name="eth1" status="------">
				<ip address=" " netmask=" "></ip>
				<switchport>Trunk_Port</switchport>
				<vlan allowed_vlan="1,10,20" vlan_id="0"></vlan>
			</if>
		</switch>
	</vm>
</vns>

```
