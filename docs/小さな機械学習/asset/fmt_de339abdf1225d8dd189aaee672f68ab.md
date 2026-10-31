
# テスト

## モデルテスト

私たちの "hello world "アプリケーションでは、モデルに2 \* 1,024バイトしか割り当てていない。`tensor_arena`バイトしか割り当てなかった。我々のスピーチ・モデルはもっと大きく、複雑な入出力を扱うので、より多くのスペース（10 1,024）が必要になる。これは試行錯誤によって決定された。

次に、入力テンソルのサイズをチェックする。しかし、今回は少し違う：

```
// Get information about the memory area to use for the model's input.
TfLiteTensor* input = interpreter.input(0);
// Make sure the input has the properties we expect.
TF_LITE_MICRO_EXPECT_NE(nullptr, input);
TF_LITE_MICRO_EXPECT_EQ(4, input->dims->size);
TF_LITE_MICRO_EXPECT_EQ(1, input->dims->data[0]);
TF_LITE_MICRO_EXPECT_EQ(49, input->dims->data[1]);
TF_LITE_MICRO_EXPECT_EQ(40, input->dims->data[2]);
TF_LITE_MICRO_EXPECT_EQ(1, input->dims->data[3]);
TF_LITE_MICRO_EXPECT_EQ(kTfLiteUInt8, input->type);
```

入力としてスペクトログラムを扱っているため、入力テンソルは合計4つの次元を持つ。最初の次元は、1つの要素を含むラッパーに過ぎない。2番目と3番目はスペクトログラムの「行」と「列」を表す。入力テンソルの一番内側の4番目の次元はサイズ1で、スペクトログラムの個々の「ピクセル」を保持する。スペクトログラムの構造については後で詳しく説明する。

次に、定数`g_yes_micro_f2e59fea_nohash_1_data` に格納されている「yes」のスペクトログラムのサンプルを取得する。この定数は[`micro_features/yes_micro_features_data.cc`](https://oreil.ly/rVn8O)ファイルで定義されている。スペクトログラムは1次元配列として存在し、それを反復して入力テンソルにコピーするだけである：

```
// Copy a spectrogram created from a .wav audio file of someone saying "Yes"
// into the memory area used for the input.
const uint8_t* yes_features_data = g_yes_micro_f2e59fea_nohash_1_data;
for (int i = 0; i < input->bytes; ++i) {
  input->data.uint8[i] = yes_features_data[i];
}
```

入力が代入された後、推論を実行し、出力テンソルのサイズと形状を検査する：

```
// Run the model on this input and make sure it succeeds.
TfLiteStatus invoke_status = interpreter.Invoke();
if (invoke_status != kTfLiteOk) {
  error_reporter->Report("Invoke failed\n");
}
TF_LITE_MICRO_EXPECT_EQ(kTfLiteOk, invoke_status);

// Get the output from the model, and make sure it's the expected size and
// type.
TfLiteTensor* output = interpreter.output(0);
TF_LITE_MICRO_EXPECT_EQ(2, output->dims->size);
TF_LITE_MICRO_EXPECT_EQ(1, output->dims->data[0]);
TF_LITE_MICRO_EXPECT_EQ(4, output->dims->data[1]);
TF_LITE_MICRO_EXPECT_EQ(kTfLiteUInt8, output->type);
```

我々の出力には2つの次元がある。1つ目は単なるラッパーだ。もう1つは4つの要素である。これは、4つのクラス（沈黙、不明、"yes"、"no"）がそれぞれマッチした確率を保持する構造である。

次のコードの塊は、確率が期待通りであったかどうかをチェックする。出力テンソルの与えられた要素は常に特定のクラスを表すので、それぞれについてどのインデックスをチェックすべきかがわかる。順番はトレーニング中に定義される：

```
// There are four possible classes in the output, each with a score.
const int kSilenceIndex = 0;
const int kUnknownIndex = 1;
const int kYesIndex = 2;
const int kNoIndex = 3;

// Make sure that the expected "Yes" score is higher than the other classes.
uint8_t silence_score = output->data.uint8[kSilenceIndex];
uint8_t unknown_score = output->data.uint8[kUnknownIndex];
uint8_t yes_score = output->data.uint8[kYesIndex];
uint8_t no_score = output->data.uint8[kNoIndex];
TF_LITE_MICRO_EXPECT_GT(yes_score, silence_score);
TF_LITE_MICRO_EXPECT_GT(yes_score, unknown_score);
TF_LITE_MICRO_EXPECT_GT(yes_score, no_score);
```

はい」のスペクトログラムを渡したので、変数`yes_score` は、`silence_score` 、`unknown_score` 、`no_score` よりも高い確率で含まれていると予想される。

イエス」で満足したら、「ノー」のスペクトログラムで同じことをする。まず、入力をコピーして推論を実行する：

```
// Now test with a different input, from a recording of "No".
const uint8_t* no_features_data = g_no_micro_f9643d42_nohash_4_data;
for (int i = 0; i < input->bytes; ++i) {
  input->data.uint8[i] = no_features_data[i];
}
// Run the model on this "No" input.
invoke_status = interpreter.Invoke();
if (invoke_status != kTfLiteOk) {
  error_reporter->Report("Invoke failed\n");
}
TF_LITE_MICRO_EXPECT_EQ(kTfLiteOk, invoke_status);
```

推論が終わった後、「いいえ」が最高得点を獲得したことを確認する：

```
// Make sure that the expected "No" score is higher than the other classes.
silence_score = output->data.uint8[kSilenceIndex];
unknown_score = output->data.uint8[kUnknownIndex];
yes_score = output->data.uint8[kYesIndex];
no_score = output->data.uint8[kNoIndex];
TF_LITE_MICRO_EXPECT_GT(no_score, silence_score);
TF_LITE_MICRO_EXPECT_GT(no_score, unknown_score);
TF_LITE_MICRO_EXPECT_GT(no_score, yes_score);
```

それで終わりだ！

このテストを実行するには、TensorFlowリポジトリのルートから以下のコマンドを実行する：

```
make -f tensorflow/lite/micro/tools/make/Makefile \
  test_micro_speech_test
```

次は、すべてのオーディオデータのソースであるオーディオ・プロバイダを見てみよう。

## オーディオ・プロバイダー

オーディオ・プロバイダは、デバイスのマイク・ハードウェアを我々のコードに接続するものだ。すべてのデバイスは、オーディオをキャプチャするための異なるメカニズムを持っている。そのため、[`audio_provider.h`は](https://oreil.ly/89FGG)オーディオデータを要求するためのインタフェースを定義しており、開発者はサポートしたいプラットフォームに合わせて独自の実装を書くことができる。

### チップ

この例には、Arduino、STM32F746G、SparkFun Edge、macOS用のオーディオ・プロバイダの実装が含まれている。このサンプルで新しいデバイスをサポートしたい場合は、既存の実装を読み取り、その方法を学ぶことができる。

オーディオ・プロバイダの核となる部分は、`audio_provider.h`で定義されている`GetAudioSamples()` という関数である。これは次のようになる：

```
TfLiteStatus GetAudioSamples(tflite::ErrorReporter* error_reporter,
                             int start_ms, int duration_ms,
                             int* audio_samples_size, int16_t** audio_samples);
```

`audio_provider.h`に記述されているように、この関数は16ビットのパルス符号変調（PCM）オーディオデータの配列を返すことが期待されている。これはデジタル・オーディオでは非常に一般的な形式である。

この関数は、`ErrorReporter` インスタンス、開始時刻(`start_ms`)、継続時間(`duration_ms`)、2つのポインタを指定して呼び出される。

これらのポインターは、`GetAudioSamples()` 、データを提供するためのメカニズムである。呼び出し側は、適切な型の変数を宣言し、関数呼び出し時にその変数へのポインタを渡す。関数の実装内部では、ポインタがデリファレンスされ、変数の値がセットされる。

最初のポインタ`audio_samples_size` には、オーディオデータの16ビットサンプルの合計数が入る。2番目のポインタ`audio_samples` は、オーディオデータそのものを含む配列を受け取る。

テストを見ることで、これを実際に見ることができる。[`audio_provider_test.cc`には](https://oreil.ly/9XgFg)2つのテストがあるが、オーディオ・プロバイダの使い方を学ぶには、最初のテストだけを見ればいい：

```
TF_LITE_MICRO_TEST(TestAudioProvider) {
  tflite::MicroErrorReporter micro_error_reporter;
  tflite::ErrorReporter* error_reporter = &micro_error_reporter;

  int audio_samples_size = 0;
  int16_t* audio_samples = nullptr;
  TfLiteStatus get_status =
      GetAudioSamples(error_reporter, 0, kFeatureSliceDurationMs,
                      &audio_samples_size, &audio_samples);
  TF_LITE_MICRO_EXPECT_EQ(kTfLiteOk, get_status);
  TF_LITE_MICRO_EXPECT_LE(audio_samples_size, kMaxAudioSampleSize);
  TF_LITE_MICRO_EXPECT_NE(audio_samples, nullptr);

  // Make sure we can read all of the returned memory locations.
  int total = 0;
  for (int i = 0; i < audio_samples_size; ++i) {
    total += audio_samples[i];
  }
}
```

このテストでは、`GetAudioSamples()` がいくつかの値といくつかのポインターでどのように呼び出されるかを示している。このテストでは、関数呼び出し後にポインタが正しく代入されていることを確認している。

### 注

`kFeatureSliceDurationMs` と`kMaxAudioSampleSize` という定数が使われていることに気づくだろう。これらはモデルの学習時に選択された値で、[`micro_features/micro_model_settings.h`で](https://oreil.ly/WLuug)発見できる。

`audio_provider.cc`のデフォルト実装は、空の配列を返すだけだ。それが正しいサイズであることを証明するために、テストは単純に予想されるサンプル数だけループする。

オーディオプロバイダーには、`GetAudioSamples()` に加え、`LatestAudioTimestamp()` という関数呼び出しがある。これは、オーディオデータが最後にキャプチャされた時刻をミリ秒単位で返すものである。この情報は、フィーチャー・プロバイダーがフェッチするオーディオ・データを決定するために必要である。

オーディオ・プロバイダのテストを実行するには、以下のコマンドを使用する：

```
make -f tensorflow/lite/micro/tools/make/Makefile \
  test_audio_provider_test
```

オーディオ・プロバイダーは、フィーチャー・プロバイダーが新鮮なオーディオ・サンプルのソースとして使用する。

## フィーチャー・プロバイダー

特徴プロバイダーは、オーディオプロバイダーから取得した生のオーディオを、モデルに入力できるスペクトログラムに変換する。これはメインループ中に呼び出される。

そのインタフェースは[`feature_provider.h`で](https://oreil.ly/59uTO)定義されており、以下のようになっている：

```
class FeatureProvider {
 public:
  // Create the provider, and bind it to an area of memory. This memory should
  // remain accessible for the lifetime of the provider object, since subsequent
  // calls will fill it with feature data. The provider does no memory
  // management of this data.
  FeatureProvider(int feature_size, uint8_t* feature_data);
  ~FeatureProvider();

  // Fills the feature data with information from audio inputs, and returns how
  // many feature slices were updated.
  TfLiteStatus PopulateFeatureData(tflite::ErrorReporter* error_reporter,
                                   int32_t last_time_in_ms, int32_t time_in_ms,
                                   int* how_many_new_slices);

 private:
  int feature_size_;
  uint8_t* feature_data_;
  // Make sure we don't try to use cached information if this is the first call
  // into the provider.
  bool is_first_run_;
};
```

これがどのように使われているかを見るには、[`feature_provider_mock_test.cc`の](https://oreil.ly/N3YPu)テストを見てみよう。
