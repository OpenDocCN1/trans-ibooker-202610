
```
# macOS:
tensorflow/lite/micro/tools/make/gen/osx_x86_64/bin/hello_world

# Linux:
tensorflow/lite/micro/tools/make/gen/linux_x86_64/bin/hello_world

# Windows
tensorflow/lite/micro/tools/make/gen/windows_x86_64/bin/hello_world
```

正しいパスが発見できない場合は、`tensorFlow/lite/micro/tools/make/gen/`にあるディレクトリをリストアップする。

バイナリを実行すると、うまくいけば、以下のような出力がスクロールしていくはずだ：

```
x_value: 1.4137159*2^1, y_value: 1.374213*2^-2

x_value: 1.5707957*2^1, y_value: -1.4249528*2^-5

x_value: 1.7278753*2^1, y_value: -1.4295994*2^-2

x_value: 1.8849551*2^1, y_value: -1.2867725*2^-1

x_value: 1.210171*2^2, y_value: -1.7542461*2^-1
```

とてもエキサイティングだ！これらは`output_handler.cc`の `HandleOutput()` 関数によって書き出されたログである。`x_value`、2πに達するまで徐々にインクリメントされ、その時点で0に戻ってまた始まる。

十分に興奮したら、`Ctrl-C`を押してプログラミングを終了させることができる。

### 注

数値は、`1.4137159*2^1` のように、2のべき乗指数を持つ値として出力されることに気づくだろう。これは、浮動小数点演算をハードウェアでサポートしていないことが多いマイクロコントローラで浮動小数点数を記録する効率的な方法である。

例えば、`1.4137159*2^1` は`2.8274318` と評価される。気になるなら、これらの数値を出力するコードは[`debug_log_numbers.cc`](https://oreil.ly/sb06c) にある。

# まとめ

これで、開発マシン上でプログラミングが動作することが確認できた。次の章では、このプログラムをいくつかのマイクロコントローラーで動かしてみよう！

# 第6章 TinyMLの「Hello World」：マイクロコントローラへのデプロイ

この作品はAIを使って翻訳されている。ご意見、ご感想をお待ちしている：[translation-feedback@oreilly.com](mailto:translation-feedback@oreilly.com)

さて、いよいよ手を動かす時が来た。この章では、3つの異なるデバイスにコードをデプロイする：

* [Arduino Nano 33 BLEセンス](https://oreil.ly/6qlMD)
* [スパークファン・エッジ](https://oreil.ly/-hoL-)
* [STマイクロエレクトロニクス STM32F746Gディスカバリー・キット](https://oreil.ly/cvm4J)

それぞれのビルドとデプロイのプロセスを説明しよう。

### 注

TensorFlow Liteは定期的に新しいデバイスのサポートを追加しているので、使いたいデバイスがここにリストされていない場合は、サンプルの[`README.md`](https://oreil.ly/ez0ef)チェックする価値がある。

この手順で問題が発生した場合は、最新のデプロイ手順を確認することもできる。

各デバイスは、LEDバンクから完全なLCDディスプレイまで、それぞれ独自の出力機能を持っている。そのため、このサンプルには、それぞれ`HandleOutput()` のカスタム実装が含まれている。また、それぞれのロジックがどのように動作するかについても説明する。すべてのデバイスを持っていなくても、このコードを読み通すことは興味深いはずなので、ぜひ見てみることをお勧めする。

# マイクロコントローラーとは何か？

これまでの経験によっては、マイクロコントローラーが他の電子部品とどのように相互作用するのかよく知らないかもしれない。これからハードウェアで遊び始めるのだから、先に進む前にいくつかのアイデアを紹介しておく価値があるだろう。

Arduino、SparkFun Edge、またはSTM32F746G Discoveryキットのようなマイコンボードでは、実際のマイコンは回路基板に取り付けられた多くの電子部品の1つにすぎない。[図6-](#ch06.html#sparkfun_edge_microcontroller)1に、SparkFun Edge上のマイクロコントローラーを示す。

![](assets/timl_0601.png)

マイクロコントローラーは、ピンを使って回路基板に接続されている。典型的なマイクロコントローラーには数十本のピンがあり、あらゆる種類の役割を果たす。マイクロコントローラーに電力を供給するものもあれば、さまざまな重要部品に接続するものもある。一部のピンは、マイクロコントローラー上で実行されるプログラミングによるデジタル信号の入出力用に確保されている。これらは GPIOピンと呼び、一般化入出力の略である。これらのピンは、電圧が印加されているかどうかを判断する入力としても、他のコンポーネントに電力を供給したり通信したりするための電流を供給する出力としても機能する。

GPIOピンはデジタルである。つまり、出力モードでは、完全にオンにもオフにもなるスイッチのようなものだ。入力モードでは、他のコンポーネントから印加された電圧がある閾値以上か以下かを検出することができる。

GPIOに加えて、一部のマイクロコントローラーにはアナログ入力ピンがあり、そこに印加されている電圧レベルを正確に測定することができる。

特殊化関数を呼び出すことで、マイクロコントローラー上で動作するプログラムは、指定したピンが入力モードか出力モードかを制御できる。その他の関数は、出力ピンのオン/オフを切り替えたり、入力ピンの現在の状態を読み取ったりするために使用される。

マイクロコントローラーについて少しわかったところで、最初のデバイスであるArduinoを詳しく見てみよう。

# Arduino

[Arduino](https://www.arduino.cc/)ボードには実に様々な種類があり、どれも異なる機能を持っている。すべてのボードでTensorFlow Lite for Microcontrollersが動作するわけではない。本書で推奨するボードは[Arduino Nano 33 BLE Sense](https://oreil.ly/9g1bJ)だ。TensorFlow Liteと互換性があるだけでなく、マイクと加速度センサー（後の章で使用する）も搭載されている。ヘッダ付きのボードを購入することをお勧めする。ヘッダを使えば、ハンダ付けなしで他のコンポーネントを簡単に接続できる。

ほとんどのArduinoボードにはLEDが内蔵されており、これを使用して正弦波の値を視覚的に出力する。[図6-](#ch06.html#arduino_nano_sense_led)2は、LEDが強調表示されたArduino Nano 33 BLE Senseボードである。

![](assets/timl_0602.png)

## Arduinoで出力を扱う

使えるLEDは1つだけなので、創造的に考える必要がある。一つの選択肢は、直近に予測されたサイン値に基づいてLEDの明るさを変化させることである。値の範囲が-1から1であることを考えると、0を完全に消灯したLEDで、-1と1を完全に点灯したLEDで、中間の値を部分的に減光したLEDで表すことができる。プログラミングがループで推論を実行すると、LEDは点灯と消灯を繰り返す。

`kInferencesPerCycle` 定数を使用することで、正弦波1サイクルで実行する推論の回数を変えることができる。1回の推論にかかる時間は決まっているため、`constants.cc`で定義されている`kInferencesPerCycle` を微調整することで、LEDの減衰速度を調整することができる。

このファイルのArduino専用バージョンが[`hello_world/arduino/constants.cc`](https://oreil.ly/YNsvq)にある。このファイルには`hello_world/constants.cc`と同じ名前付けがされているので、アプリケーションがArduino用にビルドされるときに、オリジナルの実装の代わりに使われる。

内蔵LEDを暗くするには、パルス幅変調（PWM）と呼ばれるテクニックを使うことができる。出力ピンのオンとオフを極端に速く切り替えると、ピンの出力電圧は、オフとオンの状態に費やされる時間の比率の要因になる。ピンが各状態で50％の時間を過ごした場合、出力電圧は最大値の50％になる。オン状態が75％、オフ状態が25％の場合、出力電圧は最大値の75％となる。

PWMは特定のArduinoデバイスの特定のピンでしか利用できないが、使い方はとても簡単で、ピンに希望の出力レベルをセットする関数を呼び出すだけだ。

Arduino用の出力処理を実装したコードは[`hello_world/arduino/output_handler.cc`](https://oreil.ly/OpLMB)あり、元のファイル`hello_world/output_handler.cc`の代わりに使用される。

ソースを見てみよう：

```
#include "tensorflow/lite/micro/examples/hello_world/output_handler.h"
#include "Arduino.h"
#include "tensorflow/lite/micro/examples/hello_world/constants.h"
```

まず、いくつかのヘッダファイルをインクルードする。`output_handler.h`はこのファイルのインタフェースを指定する。`Arduino.h`はArduinoプラットフォームのインタフェースを提供する。ボードの制御にはこれを使う。`kInferencesPerCycle` にアクセスする必要があるので、`constants.h`もインクルードする。

次に、関数を定義し、最初の実行時に何をするかを指示する：

```
// Adjusts brightness of an LED to represent the current y value
void HandleOutput(tflite::ErrorReporter* error_reporter, float x_value,
                  float y_value) {
// Track whether the function has run at least once
static bool is_initialized = false;

// Do this only once
if (!is_initialized) {
  // Set the LED pin to output
  pinMode(LED_BUILTIN, OUTPUT);
  is_initialized = true;
}
```

C++では、関数内で`static` と宣言された変数は、その関数を複数回実行してもその値を保持する。ここでは、`is_initialized` 変数を使用して、次の`if (!is_initialized)` ブロックのコードが以前に実行されたかどうかを追跡する。

初期化ブロックは、Arduinoの 関数を呼び出している。 [`pinMode()`](https://oreil.ly/6Kxep)関数を呼び出す。この関数は、与えられたピンが入力モードか出力モードかをマイクロコントローラーに示す。これは、ピンを使用する前に必要である。この関数は、Arduinoプラットフォームで定義されている2つの定数、`LED_BUILTIN` と`OUTPUT` で呼び出される。`LED_BUILTIN` は、プラットフォームに内蔵されているLEDに接続されているピンを表し、`OUTPUT` は出力モードを表す。

内蔵LEDのピンを出力モードに設定した後、`is_initialized` を`true` に設定し、このブロックコードが再び実行されないようにする。

次に、LEDに必要な明るさを計算する：

```
// Calculate the brightness of the LED such that y=-1 is fully off
// and y=1 is fully on. The LED's brightness can range from 0-255.
int brightness = (int)(127.5f * (y_value + 1));
```

Arduinoでは、PWM出力のレベルを0から255までの数値でセットできる。0は完全消灯、255は完全点灯を意味する。`y_value` は-1から1の間の数値である。先のコードでは、`y_value` を0から255の範囲にマッピングし、`y = -1` のときはLEDが完全に消灯し、`y = 0` のときはLEDが半分点灯し、`y = 1` のときはLEDが完全に点灯するようにしている。

次のステップは、実際にLEDの明るさをセットすることだ：

```
// Set the brightness of the LED. If the specified pin does not support PWM,
// this will result in the LED being on when y > 127, off otherwise.
analogWrite(LED_BUILTIN, brightness);
```

Arduinoプラットフォームの 。 [`analogWrite()`](https://oreil.ly/nNseR)関数は、ピン番号（ここでは`LED_BUILTIN` ）と0から255の間の値を取る。前の行で計算した`brightness` 。この関数が呼び出されると、LEDがそのレベルで点灯する。

### 注

残念ながら、Arduinoボードのモデルによっては、内蔵LEDが接続されているピンがPWMに対応していない。つまり、`analogWrite()` への呼び出しでは明るさを変化させることができない。その代わり、`analogWrite()` に渡された値が127以上ならLEDは点灯し、126以下なら消灯する。つまり、LEDは暗くなったり消えたりするのではなく、点滅するのだ。これほどクールではないが、正弦波の予測は実証されている。

最後に、`ErrorReporter` インスタンスを使って明るさの値を記録する：

```
// Log the current brightness value for display in the Arduino plotter
error_reporter->Report("%d\n", brightness);
```

Arduinoプラットフォームでは、`ErrorReporter` 、シリアルポート経由でデータを記録するようにセットアップされている。シリアルは、マイクロコントローラーがホストコンピューターと通信するための非常に一般的な方法で、デバッグによく使われる。出力ピンをオン・オフすることで、データを1ビットずつ通信する通信プロトコルだ。生のバイナリ・データからテキストや数値まで、あらゆるものの送受信に使用できる。

Arduino IDEには、シリアルポート経由で受信したデータをキャプチャして表示するためのツールが含まれている。ツールの1つであるシリアル・プロッタは、シリアル経由で受信した値のグラフを表示することができる。コードから輝度値のストリームを出力することで、それらをグラフで見ることができる。[図6-3は](#ch06.html#serial_plotter)、その様子を示している。

![](assets/timl_0603.png)

シリアルプロッタの使い方は、このセクションの後半で説明する。

### 注

`ErrorReporter` がどうやってArduinoのシリアル・インタフェース経由でデータを出力できるのか不思議に思うかもしれない。コードの実装は[`micro/arduino/debug_log.cc`](https://oreil.ly/fkF8H)発見できる。これは、[`micro/debug_log.cc`](https://oreil.ly/nxXgJ)のオリジナルの実装を置き換えるものだ。`output_handler.cc`が上書きされるのと同様に、TensorFlow Lite for Microcontrollersの任意のソースファイルをプラットフォーム名のディレクトリに追加することで、プラットフォーム固有の実装を提供することができる。

## 例題を実行する

次の仕事は、Arduino用にプロジェクトを構築し、それをデバイスにデプロイすることだ。

### チップ

この本が書かれた当時とビルド手順が変わっている可能性もあるので、最新の手順は[`README.md`](https://oreil.ly/s2mj1)チェックしてほしい。

これが必要なものすべてだ：

* 対応するArduinoボード（Arduino Nano 33 BLE Senseを推奨する）
* 適切なUSBケーブル
* [Arduino IDE](https://oreil.ly/c-rv6)（続ける前にダウンロードしてインストールする必要がある）
