# Solenoid Stroke Control

ソレコン応募作品の[ネッコノテ](https://www.youtube.com/watch?v=gPOoony_fmU)では、ソレノイドの位置をフィードバック制御しています。これをなるべくお手軽に再現できる情報をまとめます。

## 用意するもの

- タカハの[ロングストロークソレノイドCH12840123](https://www.takaha-shop.com//SHOP/ch1284.html)
- [スライドボリューム(B10kΩ)](https://akizukidenshi.com/catalog/g/g109238/)と接続用の適当なビニール線（3本）
- Groveポートが2個あるマイコン。例えば[M5Stack Core2](https://www.switch-science.com/products/9349)と、[M5Stack Core2用ポート拡張モジュール](https://www.switch-science.com/products/8308)または[M5Core2BaseLite](https://www.switch-science.com/products/6763)
- [SolenoidUnit](https://www.switch-science.com/products/8517)
- [Groveケーブル電源電圧変換モジュール](https://www.switch-science.com/products/9461)
- 12V電源(1A程度以上)。例えば[秋月M120100-A010JP](https://akizukidenshi.com/catalog/g/g117429/])必要に応じてそれ用のDCジャック。[M5Stack BAVG2ソケット](https://www.switch-science.com/products/7234)を使うと便利です
- アクリル板(3mm厚)を加工データ"Attachment.pdf"でレーザーカットしたパーツ4点
- M3x10ネジ（4本）
- M3ナット（3個）
- M2x10ネジ（2本）
- Groveケーブル（3本）
- スライドボリュームをGroveケーブルに接続する部品（[Groveケーブル用ネジ端子基板](https://www.switch-science.com/products/9397)を使うと簡便に固定できます


## 組み立て

### 機構の組み立て

<img src="https://github.com/akita11/SolenoidStrokeControl/blob/main/config1.jpg" width="320px">

- スライドボリュームをM2x10ネジ（2個）でアクリルパーツ(1)に固定、それをロングストロークソレノイドにM3x10ネジ（2個）で固定する
- ロングストロークソレノイドのお尻部分のネジにアクリルパーツ(2)をM3ナットで固定する
- アクリルパーツ(3)(4)にM3x10ネジ+M3ナットをとりつけ、スライドボリュームのツマミ部分をはさみつつ、アクリルパーツ(2)の穴にはさみ（アクリルパーツ(4)が外側）、ネジをしめて固定する


### 制御回路の接続

※以下はM5Stack Core2とPortA/PortBを使う場合の例です

<img src="https://github.com/akita11/SolenoidStrokeControl/blob/main/config2.jpg" width="320px">

- PortAとSolenoidUnitをGroveケーブルで接続。SolenoidUnitのオレンジ色コネクタに、ソレノイド(A+・A-端子）と+12V電源を接続する
- PortBとGroveケーブル電源電圧変換モジュール(5V側)をGroveケーブルで接続。Groveケーブル電源電圧変換モジュール(3.3V側)のG(GND), C（一番端の端子）, V(+3.3V)のそれぞれに、スライドボリュームの1, 2, 3番をビニール線で接続（前述の「Groveケーブル用ネジ端子基板」を使うとネジ端子で固定できるが、ジャンパ線をGroveケーブルのコネクタに直接差し込むなどでもOK）


## ソレノイドの位置制御

※以下はM5Stack Core2を使う場合の例です

<img src="https://github.com/akita11/SolenoidStrokeControl/blob/main/SolenoidStrokeControl.png" width="320px">

[UIFlow用のテストプログラム](SolenoidStrokeControl.m5f)をUIFlowで読み込み、Core2で実行。スライダの位置に応じてソレノイドの位置が動く。表示される数値は、上から順に、ソレノイドの押し出し位置(pos, 0〜100)、スライダで設定する制御目標位置(posT, 0〜100)、PWM制御値（Duty比duty, : 0〜100）。

動作例の動画は"motion.mov"です。

制御アルゴリズムが、パラメータが未調整のP制御のため、PWM比が低いところでソレノイドが動きません。pos, posTから制御量dutyを計算する式を調整してください。



## Author

Junichi Akita (@akita11, akita@ifdl.jp)
