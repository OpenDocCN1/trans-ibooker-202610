
ファイルを保存する。これでパッチは完了だ！

保存文件。至此，补丁已完成！

サンプルを実行するには、ArduinoデバイスをUSB経由で接続する。[図11-6に](#ch11.html#arduino_board_dropdown_3)示すように、ToolsメニューのBoardドロップダウン・リストから正しいデバイス・タイプが選択されていることを確認する。

要运行示例，请通过 USB 连接 Arduino 设备。如[图11-6中](#ch11.html#arduino_board_dropdown_3)所示，请确认 Tools 菜单的 Board 下拉列表中已选择正确的设备类型。

リストにデバイス名が表示されない場合は、サポートパッケージをインストールする必要がある。これを行うには、Boards Managerをクリックし、表示されるウィンドウでお使いのデバイスを検索し、対応するサポートパッケージの最新バージョンをインストールする。

如果列表中未显示设备名称，则需要安装支持包。为此，请单击 Boards Manager，在出现的窗口中搜索您使用的设备，并安装相应支持包的最新版本。

次に、[図11-7に](#ch11.html#arduino_port_dropdown_3)示すように、ToolsメニューのPortドロップダウンでデバイスのポートが選択されていることを確認する。

接下来，如[图11-7中](#ch11.html#arduino_port_dropdown_3)所示，请确认 Tools 菜单的 Port 下拉菜单中已选择设备的端口。

![](assets/timl_0605.png)![](assets/timl_0606.png)

最後に、Arduinoウィンドウのアップロードボタン（[図11-8で](#ch11.html#arduino_upload_button_3)白くハイライトされている）をクリックしてコードをコンパイルし、Arduinoデバイスにアップロードする。

最后，单击 Arduino 窗口的上传按钮（在[图11-8中](#ch11.html#arduino_upload_button_3)中以白色高亮显示），编译代码并上传到 Arduino 设备。

![](assets/timl_0607.png)

アップロードが正常に完了すると、ArduinoボードのLEDが点滅し始めるのが見えるはずだ。

上传成功完成后，您应该可以看到 Arduino 板上的 LED 开始闪烁。

いくつかのジェスチャーを試すには、ツールメニューからシリアルモニターを選択する。初期化すると以下のような出力が表示されるはずだ：

要尝试一些手势，请从工具菜单中选择串行监视器。初始化后应显示如下输出：

```
Magic starts!
```

ジェスチャーをしてみよう。[図11-9に](#ch11.html#gesture_holding_arduino)示すように、部品が上を向き、USBアダプターが左を向くように、片手でボ ードを持ち上げる。

让我们试试手势。如[图11-9中](#ch11.html#gesture_holding_arduino)所示，请用一只手抬起板子，使元件朝上，USB适配器朝左。

![](assets/timl_1109.png)

[図11-10は](#ch11.html#gesture_spells_2)、各ジェスチャーの実行方法を示した図である。このモデルは、ボードを杖に取り付けたときに収集されたデータで学習されたため、動作させるには何度か試す必要があるかもしれない。

[图11-10是](#ch11.html#gesture_spells_2)展示了如何执行每个手势的图。由于该模型是用在将板子安装到魔杖时收集的数据训练的，因此可能需要尝试几次才能使其工作。

![](assets/timl_1103.png)

一番簡単なのは "翼 "だ。ジェスチャーを実行するのに1秒程度かかるくらい、素早く手を動かすこと。成功すれば、次のような出力が表示され、赤いLEDが点灯するはずだ：

最简单的是“翼”。手部动作要快，大约需要1秒钟来执行手势。如果成功，应会显示如下输出，并且红色 LED 会亮起：

```
WING:
*         *         *
 *       * *       *
  *     *   *     *
   *   *     *   *
    * *       * *
     *         *
```

おめでとう。Arduinoを使って初めて魔法の呪文を唱えたのだ！

恭喜你！你用 Arduino 首次施展了魔法咒语！

#### 注

#### 注

この時点で、手から最も遠いマジック・ワンドの先端にボードを取り付けるという独創的な方法を選ぶこともできる。長さ1フィート（30cm）程度の棒や定規など、家庭にあるものなら何でもよい。

在此阶段，您也可以选择一种创造性的方法，将板子安装在距离手最远的魔杖尖端。任何家用物品都可以，例如长度约1英尺（30厘米）的棒或尺子。

デバイスがしっかりと取り付けられていることを確認し、同じ向きで、部品が上を向き、USBアダプターが左を向くようにする。ぐらつきがあると加速度センサーの読み取り値に影響する。

确保设备牢固安装，方向相同，元件朝上，USB适配器朝左。晃动会影响加速度传感器的读数。

次に、手（または杖の先）で時計回りの円をなぞる「リング」のジェスチャーを試してみよう。ここでも、ジェスチャーを行うのに1秒程度を目安にする。まるで魔法のように、次のようなものが現れるはずだ：

接下来，尝试“环”手势，用手（或魔杖尖端）顺时针画一个圆。这里也以大约1秒为执行手势的时间。就像魔法一样，应该会出现如下内容：

```
RING:
          *
       *     *
     *         *
    *           *
     *         *
       *     *
          *
```

最後のジェスチャーは、三角形の角を空中でなぞる。ここで紹介するアスキーアートのデモンストレーションが、このジェスチャーを最もよく表している：

最后的手势是在空中描画三角形的角落。以下介绍的 ASCII 艺术演示最能代表这个手势：

```
SLOPE:
        *
       *
      *
     *
    *
   *
  *
 * * * * * * * *
```

優れた魔法の呪文のように、毎回完璧に実行できるようになるには、少し練習する必要があるかもしれない。[`README.md` で](https://oreil.ly/O1LqD)ジェスチャーのデモンストレーションビデオを見ることができる。

就像优秀的魔法咒语一样，可能需要一些练习才能每次都完美执行。可以在[`README.md` 中](https://oreil.ly/O1LqD)观看手势演示视频。

## 自分で変更を加える

## 自行修改

基本的なアプリケーションをデプロイしたところで、コードを変更して遊んでみよう。Arduino IDEでファイルを編集して保存し、前の手順を繰り返して変更したコードをデバイスにデプロイする。

在部署了基本应用程序后，让我们修改代码并尝试一下。在 Arduino IDE 中编辑并保存文件，然后重复之前的步骤将修改后的代码部署到设备上。

いくつか試してみてほしいことがある：

有一些事情希望你尝试一下：

* [`Arduino/constants.cc` の](https://oreil.ly/H49iS)しきい値を実験して、ジェスチャーをやりやすくしたり、やりにくくしたりする（その代償として偽陽性や偽陰性が増える）。
* 实验[`Arduino/constants.cc` 的](https://oreil.ly/H49iS)阈值，使手势更容易或更难执行（代价是假阳性和假阴性会增加）。
* 身体的なジェスチャーを使ってタスクを実行できるようなプログラミングをコンピュータに書く。
* 编写程序让计算机能够使用身体手势执行任务。
* 検出結果をBluetoothで送信するようにプログラミングを拡張する。[Arduino](https://oreil.ly/xW4SN) IDEからダウンロードできる[ArduinoBLEライブラリに](https://oreil.ly/xW4SN)、その方法を示す例が含まれている。
* 扩展编程以通过 Bluetooth 发送检测结果。[Arduino](https://oreil.ly/xW4SN) IDE 可下载的[ArduinoBLE库中](https://oreil.ly/xW4SN)包含展示此方法的示例。

## スパークファン・エッジ

## SparkFun Edge

SparkFun Edgeは、3軸加速度センサー、バッテリーマウント、ブルートゥースをサポートしている。ワイヤレスで操作できるため、魔法の杖に最適だ。

SparkFun Edge 支持三轴加速度传感器、电池安装和蓝牙。由于可以无线操作，因此非常适合作为魔杖。

### SparkFun Edgeで加速度計のデータを取得する

### 使用 SparkFun Edge 获取加速度计数据

加速度センサーのデータを取得するコードは[`sparkfun_edge/accelerometer_handler.cc` に](https://oreil.ly/yZi0v)ある。その多くはデバイス固有のものだが、実装の詳細は飛ばして、重要なことに集中しよう。

获取加速度传感器数据的代码位于[`sparkfun_edge/accelerometer_handler.cc` 中](https://oreil.ly/yZi0v)。其中大部分是设备特定的，但让我们跳过实现细节，专注于重要内容。

加速度センサーのデータを取り込む最初のステップは、ハードウェアの設定である。`SetupAccelerometer()` 関数は、加速度センサーが必要とする様々な低レベルのパラメータをセットすることで、これを開始する：

获取加速度传感器数据的第一步是硬件设置。`SetupAccelerometer()` 函数通过设置加速度传感器所需的各种低级参数来启动此过程：

```
TfLiteStatus SetupAccelerometer(tflite::ErrorReporter* error_reporter) {
  // Set the clock frequency.
  am_hal_clkgen_control(AM_HAL_CLKGEN_CONTROL_SYSCLK_MAX, 0);

  // Set the default cache configuration
  am_hal_cachectrl_config(&am_hal_cachectrl_defaults);
  am_hal_cachectrl_enable();

  // Configure the board for low power operation.
  am_bsp_low_power_init();

  // Collecting data at 25Hz.
  int accInitRes = initAccelerometer();
```

`initAccelerometer()` という名前の関数呼び出しがあることに気づくだろう。これは[SparkFun Edge BSPの加速度センサー・サンプルで](https://oreil.ly/JC0b6)定義されており、プロジェクトのビルド時に依存関係としてプルダウンされる。この関数は、ボードの加速度センサーをオンにして設定するためのさまざまなタスクを実行する。

你会注意到有一个名为 `initAccelerometer()` 的函数调用。它在[SparkFun Edge BSP 的加速度传感器示例中](https://oreil.ly/JC0b6)定义，并在项目构建时作为依赖项拉取。该函数执行各种任务以打开并配置板载加速度传感器。

加速度センサーが動作した後、[FIFOバッファを](https://oreil.ly/kFEa0)有効にする。これは加速度センサー自体にある特殊化されたメモリー・バッファで、直近の32個のデータポイントを保持する。このバッファを有効にすることで、アプリケーション・コードが推論実行中に忙しくなっても、加速度センサーの測定値を収集し続けることができる。関数の残りは、バッファをセットアップし、何か問題が発生した場合にエラーを記録する：

加速度传感器工作后，启用[FIFO缓冲区](https://oreil.ly/kFEa0)。这是一个加速度传感器本身的专用内存缓冲区，可保存最近的 32 个数据点。通过启用此缓冲区，即使应用程序代码在推理执行期间很忙，加速度传感器也能继续收集测量值。函数的其余部分设置缓冲区并在出现问题时记录错误：

```
  // Enable the accelerometer's FIFO buffer.
  // Note: LIS2DH12 has a FIFO buffer which holds up to 32 data entries. It
  // accumulates data while the CPU is busy. Old data will be overwritten if
  // it's not fetched in time, so we need to make sure that model inference is
  // faster than 1/25Hz * 32 = 1.28s
  if (lis2dh12_fifo_set(&dev_ctx, 1)) {
    error_reporter->Report("Failed to enable FIFO buffer.");
  }

  if (lis2dh12_fifo_mode_set(&dev_ctx, LIS2DH12_BYPASS_MODE)) {
    error_reporter->Report("Failed to clear FIFO buffer.");
    return 0;
  }

  if (lis2dh12_fifo_mode_set(&dev_ctx, LIS2DH12_DYNAMIC_STREAM_MODE)) {
    error_reporter->Report("Failed to set streaming mode.");
    return 0;
  }

  error_reporter->Report("Magic starts!");

  return kTfLiteOk;
}
```

初期化が終わったら、`ReadAccelerometer()` 関数を呼び出して最新のデータを取得することができる。これは推論のたびに行われる。

初始化完成后，可以调用 `ReadAccelerometer()` 函数获取最新数据。这在每次推理时都会进行。

まず、`reset_buffer` 引数が`true` の場合、`ReadAccelerometer()` はデータバッファのリセットを実行する。これは有効なジェスチャーが検出された後に実行され、以降のジェスチャーに対してまっさらな状態を提供するためである。このプロセスの一環として、`am_util_delay_ms()` を使用して、コードを10ミリ秒待たせる。この遅延がないと、新しいデータを読み取るときにコードがハングアップすることが多い（この記事を書いている時点では原因は不明だが、TensorFlowオープンソースプロジェクトでは、より良い修正方法を見つけた場合のプルリクエストを歓迎している）：

首先，如果 `reset_buffer` 参数为 `true`，`ReadAccelerometer()` 会执行数据缓冲区的重置。这是在检测到有效手势后执行的，以便为后续手势提供干净的状态。作为此过程的一部分，使用 `am_util_delay_ms()` 使代码等待 10 毫秒。如果没有此延迟，在读取新数据时代码经常会挂起（截至本文撰写时原因不明，但 TensorFlow 开源项目欢迎在找到更好的修复方法时的拉取请求）：

```
bool ReadAccelerometer(tflite::ErrorReporter* error_reporter, float* input,
                       int length, bool reset_buffer) {
  // Clear the buffer if required, e.g. after a successful prediction
  if (reset_buffer) {
    memset(save_data, 0, 600 * sizeof(float));
    begin_index = 0;
    pending_initial_data = true;
    // Wait 10ms after a reset to avoid hang
    am_util_delay_ms(10);
  }
```

メイン・バッファをリセットした後、`ReadAccelerometer()` 、加速度センサーのFIFOバッファに新しいデータがあるかどうかをチェックする。まだ何もない場合は、関数からリターンする：

在重置主缓冲区后，`ReadAccelerometer()` 检查加速度传感器的 FIFO 缓冲区是否有新数据。如果还没有，则从函数返回：

```
  // Check FIFO buffer for new samples
  lis2dh12_fifo_src_reg_t status;
  if (lis2dh12_fifo_status_get(&dev_ctx, &status)) {
    error_reporter->Report("Failed to get FIFO status.");
    return false;
  }

  int samples = status.fss;
  if (status.ovrn_fifo) {
    samples++;
  }

  // Skip this round if data is not ready yet
  if (samples == 0) {
    return false;
  }
```

つまり、データが利用可能になり次第、このポイントを通過することができる。

也就是说，一旦数据可用，就可以通过此点。

関数の次の部分では、新しいデータをループし、別の大きなバッファにストアする。まず、`axis3bit16_t` 型の特殊化構造体をセットアップし、加速度計データを格納する。次に、`lis2dh12_acceleration_raw_get()` を呼び出し、次に利用可能な測定値で埋める。この関数はエラーになるとゼロを返し、その時点でエラーを表示する：

函数的下一部分循环处理新数据并将其存储到另一个大缓冲区中。首先，设置一个 `axis3bit16_t` 类型的专用结构体来存储加速度计数据。接下来，调用 `lis2dh12_acceleration_raw_get()` 用下一个可用测量值填充它。该函数在出错时返回零，此时会显示错误：

```
  // Load data from FIFO buffer
  axis3bit16_t data_raw_acceleration;
  for (int i = 0; i < samples; i++) {
    // Zero out the struct that holds raw accelerometer data
    memset(data_raw_acceleration.u8bit, 0x00, 3 * sizeof(int16_t));
    // If the return value is non-zero, sensor data was successfully read
    if (lis2dh12_acceleration_raw_get(&dev_ctx, data_raw_acceleration.u8bit)) {
      error_reporter->Report("Failed to get raw data.");
```

測定値が正常に取得された場合、それをモデルが期待する測定単位であるミリGsに変換し、`save_data[]` に書き込む。これは、推論に使用する値を格納するバッファとして使用する配列である。加速度センサーの各軸の値は、連続して格納される：

如果测量值成功获取，则将其转换为模型期望的测量单位毫 Gs，并写入 `save_data[]`。这是一个用于存储用于推理的值的缓冲区数组。加速度传感器的每个轴的值连续存储：

```
    } else {
      // Convert each raw 16-bit value into floating point values representing
      // milli-Gs, a unit of acceleration, and store in the current position of
      // our buffer
      save_data[begin_index++] =
          lis2dh12_from_fs2_hr_to_mg(data_raw_acceleration.i16bit[0]);
      save_data[begin_index++] =
          lis2dh12_from_fs2_hr_to_mg(data_raw_acceleration.i16bit[1]);
      save_data[begin_index++] =
          lis2dh12_from_fs2_hr_to_mg(data_raw_acceleration.i16bit[2]);
      // Start from beginning, imitating loop array.
      if (begin_index >= 600) begin_index = 0;
    }
  }
```

`save_data[]` 配列は3軸の値を200セット格納できるので、`begin_index` カウンタが600に達したら0に戻す。

`save_data[]` 数组可以存储 200 组三轴值，因此当 `begin_index` 计数器达到 600 时将其重置为 0。

これで、すべての新しいデータを`save_data[]` のバッファに取り込むことができた。次に、予測を行うのに十分なデータがあるかどうかをチェックする。モデルをテストする際、バッファサイズの合計の3分の1程度が、信頼できる予測を行うための最低限のデータ量であることがわかった。したがって、少なくともこれだけのデータがあれば、`pending_initial_data` フラグを`false` に設定する（デフォルトは`true` ）：

现在，所有新数据都已摄入到 `save_data[]` 缓冲区中。接下来，检查是否有足够的数据进行预测。在测试模型时，发现缓冲区总大小的三分之一左右是进行可靠预测所需的最低数据量。因此，如果至少有这么多数据，则将 `pending_initial_data` 标志设置为 `false`（默认为 `true`）：

```
  // Check if we are ready for prediction or still pending more initial data
  if (pending_initial_data && begin_index >= 200) {
    pending_initial_data = false;
  }
```

次に、推論を実行するにはまだデータが不十分な場合、`false` を返す：

接下来，如果数据仍不足以执行推理，则返回 `false`：

```
  // Return if we don't have enough data
  if (pending_initial_data) {
    return false;
  }
```

ここまでくれば、推論を実行するのに十分なデータがバッファにあることになる。関数の最後の部分は、要求されたデータをバッファから`input` 、モデルの入力テンソルへのポインタである引数にコピーする：

到此为止，缓冲区中已有足够的数据执行推理。函数的最后一部分将请求的数据从缓冲区复制到 `input` 参数中，该参数是指向模型输入张量的指针：
