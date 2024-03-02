# IoT環境メーター / スマートリモコン

## 概要

ESP32-C3とBoschのBME680を使い、温度、湿度、気圧、二酸化炭素換算値などを測定し、結果を[Ambient](https://ambidata.io/)にアップロードできる環境メーターです。  
表示には0.96inch OLEDを使用し、スイッチで通常表示・詳細表示・簡易表示を切り替えることができます。  
Rev. 2 からは赤外線送受信部を備え、スマートリモコン機能も使うことができます。

Rev. 1 のソフトウェア及び解説記事は「[ESP32-C3とBME680でIoT環境メーターを作る](https://zenn.dev/k_takata/articles/esp32c3-envmeter)」で公開しています。  
Rev. 2 のソフトウェア及び解説記事は今後公開予定です。


## 使用したソフトウェア

KiCad 7.0


## 回路図

[![schema rev.2](https://raw.githubusercontent.com/k-takata/PCB_envmeter_esp32c3/master/images/schema2.png)](https://raw.githubusercontent.com/k-takata/PCB_envmeter_esp32c3/master/images/schema2.pdf)

## 基板パターン図

![PCB pattern rev.2](https://raw.githubusercontent.com/k-takata/PCB_envmeter_esp32c3/master/images/pcb-pattern2.png)

## 部品表

### IoT環境メーター部分

| Reference           |個数|値    | 説明 |
|---------------------|----|------|------|
|C1                   |   1|100μF|電解コンデンサー φ6.3[^1] (100 ~ 470μF)|
|C2                   |   1|  1μF|[積層セラミックコンデンサー](https://akizukidenshi.com/catalog/g/g115940/)、2.54mmピッチ|
|C3 (\*1)             |   1| 10μF|積層セラミックコンデンサー、表面実装 (3225 あるいは 3216 サイズ)|
|C4 (\*1)             |   1| 10μF|[積層セラミックコンデンサー](https://akizukidenshi.com/catalog/g/g103095/)、5mmピッチ|
|D1                   |   1|      |赤色LED φ5|
|F1 (\*2)             |   1|  0.5A|[ポリヒューズ](https://akizukidenshi.com/catalog/g/g115300/)、表面実装|
|F2 (\*2)             |   1|  1.1A|[ポリヒューズ](https://akizukidenshi.com/catalog/g/g100507/)|
|J1 (\*3)             |   1|[5077CR-16-SMC2-BK-TR](https://akizukidenshi.com/catalog/g/g114356/)|USB Type-Cコネクター|
|J2 (\*3)             |   1|      |[USB Type-CコネクターDIP化キット](https://akizukidenshi.com/catalog/g/g115426/)、[同等品](https://akizukidenshi.com/catalog/g/g113080/)でも可|
|J3                   |   1|      |[細ピンソケット](https://akizukidenshi.com/catalog/g/g110073/) 1x4 (あるいは[1列ICソケット](https://akizukidenshi.com/catalog/g/g103470/))|
|J4                   |   1|      |[ピンソケット](https://akizukidenshi.com/catalog/g/g105779/) 1x4 (あるいは[ロープロファイルピンソケット](https://akizukidenshi.com/catalog/g/g100661/))|
|(J3)                 |   1|[AE-BME680](https://akizukidenshi.com/catalog/g/g114469/)|総合環境センサー|
|(J4)                 |   1|      |[0.96インチ 128×64ドット OLED](https://akizukidenshi.com/catalog/g/g112031/)、SSD1306搭載|
|R1, R2               |   2|5.1kΩ|緑茶赤金、小型|
|R3, R4, R5, R6       |   4| 10kΩ|茶黒橙金|
|SW1                  |   1|Reset |[タクトスイッチ(赤)](https://akizukidenshi.com/catalog/g/g103646/)|
|SW2                  |   1|Mode  |[タクトスイッチ(黒)](https://akizukidenshi.com/catalog/g/g103647/)|
|U1                   |   1|[ESP32-C3-WROOM-02-N4](https://akizukidenshi.com/catalog/g/g117493/)|Wi-Fi モジュール|
|U2 (\*4)             |   1|[AP7333-33SAG](https://akizukidenshi.com/catalog/g/g111360/)|低損失三端子レギュレーター 3.3V 300mA|
|U3 (\*4)             |   1|[NJM2845DL1-33](https://akizukidenshi.com/catalog/g/g111299/)|低損失三端子レギュレーター 3.3V 800mA|

[^1]: このサイズならば、まっすぐ足が奥まで挿せる。

注:
* (\*1) C3, C4はどちらかを選択します。基板上にはC3の表示のみあります。表面実装の半田付けが難しい場合はC4を選択してください。
* (\*2) F1, F2はどちらかを選択します。基板上にはF1の表示のみあります。F1がお勧めですが、表面実装の半田付けが難しい場合はF2でも可です。
* (\*3) J1, J2はどちらかを選択します。基板上にはJ1の表示のみあります。表面実装の半田付けが難しい場合はJ2を選択してください。
* (\*4) U2, U3はどちらかを選択します。U2の方が安価ですが300mAしか使えないため、Wi-Fiをフルパワーで使いたい場合は、U3の方がお勧めです。
* AE-BME680を取り外す予定がなければ、ピンソケット(J3)を使わず直接接続してもかまいません。
* OLEDを取り外す予定がなければ、ピンソケット(J4)を使わず直接接続してもかまいません。


### 赤外線受信部

| Reference           |個数|値    | 説明 |
|---------------------|----|------|------|
|U4                   |   1|[OSRB38C9AA](https://akizukidenshi.com/catalog/g/g104659/)|赤外線リモコン受信モジュール|

注: 赤外線リモコン受信機能を使わない場合は組み立て不要です。


### 赤外線送信部

| Reference           |個数|値    | 説明 |
|---------------------|----|------|------|
|D2                   |   1|[OSI5LA5113A](https://akizukidenshi.com/catalog/g/g112612/)|赤外線LED φ5|
|Q1                   |   1|[2SC2001](https://akizukidenshi.com/catalog/g/g113828/)||
|R7                   |   1| 680Ω|青灰茶金|
|R8                   |   1|4.7kΩ|黄紫赤金|
|R9                   |   1| 5.1Ω|緑茶金金|

注: 赤外線リモコン送信機能を使わない場合は組み立て不要です。


## 使い方

### Rev. 1

「[ESP32-C3とBME680でIoT環境メーターを作る](https://zenn.dev/k_takata/articles/esp32c3-envmeter)」を参照してください。

### Rev. 2

T.B.D.

## 完成品

### Rev. 1

通常表示<br>
[![完成品、通常表示](https://raw.githubusercontent.com/k-takata/PCB_envmeter_esp32c3/master/images/envmeter-thumb.jpg)](https://raw.githubusercontent.com/k-takata/PCB_envmeter_esp32c3/master/images/envmeter.jpg)

詳細表示<br>
[![完成品、詳細表示](https://raw.githubusercontent.com/k-takata/PCB_envmeter_esp32c3/master/images/envmeter-detail-thumb.jpg)](https://raw.githubusercontent.com/k-takata/PCB_envmeter_esp32c3/master/images/envmeter-detail.jpg)

簡易表示<br>
[![完成品、簡易表示](https://raw.githubusercontent.com/k-takata/PCB_envmeter_esp32c3/master/images/envmeter-simple-thumb.jpg)](https://raw.githubusercontent.com/k-takata/PCB_envmeter_esp32c3/master/images/envmeter-simple.jpg)

### Rev. 2

T.B.D.

## License

CC0
