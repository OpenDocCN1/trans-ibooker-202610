
パズルの最後のピースであるコマンド・レスポンダは、単語が検出されたことを知らせる出力を生成する。

コマンド・レスポンダは、デバイスのタイプ毎にオーバーライドできるように設計されている。デバイス固有の実装については、この章の後半で説明する。

とりあえず、検出結果をテキストとしてログに記録するだけの、非常にシンプルなリファレンス実装を見てみよう。[`command_responder.cc`](https://oreil.ly/kMjg2) で発見できる：

```
void RespondToCommand(tflite::ErrorReporter* error_reporter,
                      int32_t current_time, const char* found_command,
                      uint8_t score, bool is_new_command) {
  if (is_new_command) {
    error_reporter->Report("Heard %s (%d) @%dms", found_command, score,
                           current_time);
  }
}
```

それだけだ！このファイルが実装している関数は、`RespondToCommand()` の1つだけである。パラメータとして、`error_reporter` 、現在時刻（`current_time` ）、最後に検出されたコマンド（`found_command` ）、そのコマンドが受けたスコア（`score` ）、そしてそのコマンドが新しく聞こえたかどうか（`is_new_command` ）を受け取る。

このプログラムのメインループでは、コマンドが検出されなかった場合でも、推論が実行されるたびにこの関数が呼び出されることに注意することが重要だ。つまり、`is_new_command` をチェックして、何かする必要があるかどうかを判断する必要がある。

[`command_responder_test.cc`](https://oreil.ly/loLZo) にあるこの関数のテストも同様にシンプルだ。正しい出力を生成することをテストする方法がないため、関数を呼び出すだけである：

```
TF_LITE_MICRO_TEST(TestCallability) {
  tflite::MicroErrorReporter micro_error_reporter;
  tflite::ErrorReporter* error_reporter = &micro_error_reporter;

  // This will have external side-effects (like printing to the debug console
  // or lighting an LED) that are hard to observe, so the most we can do is
  // make sure the call doesn't crash.
  RespondToCommand(error_reporter, 0, "foo", 0, true);
}
```

このテストを実行するには、ターミナルに次のように入力する：

```
make -f tensorflow/lite/micro/tools/make/Makefile \
  test_command_responder_test
```

以上だ！アプリケーションの構成要素をすべて見てきた。では、それらがプログラミングの中でどのように組み合わされていくかを見てみよう。

# 目覚めの言葉を聴く

次のコードは[`main_functions.cc`](https://oreil.ly/n2eD1) で発見できる。この関数は、今回のプログラミングの核となる`setup()` と`loop()` 関数を定義している。一緒に読み取ってみよう！

あなたはもうTensorFlow Liteの熟練者なので、このコードの多くは見慣れたものに見えるだろう。それでは、新しい部分に焦点を当ててみよう。

まず、使いたいオペレーションをリストアップする：

```
namespace tflite {
namespace ops {
namespace micro {
TfLiteRegistration* Register_DEPTHWISE_CONV_2D();
TfLiteRegistration* Register_FULLY_CONNECTED();
TfLiteRegistration* Register_SOFTMAX();
}  // namespace micro
}  // namespace ops
}  // namespace tflite
```

次に、グローバル変数をセットアップする：

```
namespace {
tflite::ErrorReporter* error_reporter = nullptr;
const tflite::Model* model = nullptr;
tflite::MicroInterpreter* interpreter = nullptr;
TfLiteTensor* model_input = nullptr;
FeatureProvider* feature_provider = nullptr;
RecognizeCommands* recognizer = nullptr;
int32_t previous_time = 0;

// Create an area of memory to use for input, output, and intermediate arrays.
// The size of this will depend on the model you're using, and may need to be
// determined by experimentation.
constexpr int kTensorArenaSize = 10 * 1024;
uint8_t tensor_arena[kTensorArenaSize];
}  // namespace
```

通常のTensorFlowの容疑者に加えて、`FeatureProvider` と`RecognizeCommands` を宣言していることに注目してほしい。また、`g_previous_time` という変数も宣言している。これは、新しいオーディオサンプルを受信した直近の時刻を記録している。

次に、`setup()` 関数で、モデルをロードし、インタプリタをセットアップし、オペアを追加し、テンソルを割り当てる：

```
void setup() {
  // Set up logging.
  static tflite::MicroErrorReporter micro_error_reporter;
  error_reporter = &micro_error_reporter;

  // Map the model into a usable data structure. This doesn't involve any
  // copying or parsing, it's a very lightweight operation.
  model = tflite::GetModel(g_tiny_conv_micro_features_model_data);
  if (model->version() != TFLITE_SCHEMA_VERSION) {
    error_reporter->Report(
        "Model provided is schema version %d not equal "
        "to supported version %d.",
        model->version(), TFLITE_SCHEMA_VERSION);
    return;
  }

  // Pull in only the operation implementations we need.
  static tflite::MicroMutableOpResolver micro_mutable_op_resolver;
  micro_mutable_op_resolver.AddBuiltin(
      tflite::BuiltinOperator_DEPTHWISE_CONV_2D,
      tflite::ops::micro::Register_DEPTHWISE_CONV_2D());
  micro_mutable_op_resolver.AddBuiltin(
      tflite::BuiltinOperator_FULLY_CONNECTED,
      tflite::ops::micro::Register_FULLY_CONNECTED());
  micro_mutable_op_resolver.AddBuiltin(tflite::BuiltinOperator_SOFTMAX,
                                       tflite::ops::micro::Register_SOFTMAX());

  // Build an interpreter to run the model with.
  static tflite::MicroInterpreter static_interpreter(
      model, micro_mutable_op_resolver, tensor_arena, kTensorArenaSize,
      error_reporter);
  interpreter = &static_interpreter;

  // Allocate memory from the tensor_arena for the model's tensors.
  TfLiteStatus allocate_status = interpreter->AllocateTensors();
  if (allocate_status != kTfLiteOk) {
    error_reporter->Report("AllocateTensors() failed");
    return;
  }
```

テンソルを割り当てた後、入力テンソルが正しい形と型であることをチェックする：

```
  // Get information about the memory area to use for the model's input.
  model_input = interpreter->input(0);
  if ((model_input->dims->size != 4) || (model_input->dims->data[0] != 1) ||
      (model_input->dims->data[1] != kFeatureSliceCount) ||
      (model_input->dims->data[2] != kFeatureSliceSize) ||
      (model_input->type != kTfLiteUInt8)) {
    error_reporter->Report("Bad input tensor parameters in model");
    return;
  }
```

次に面白いことが始まる。まず、`FeatureProvider` をインスタンス化し、それを入力テンソルに向ける：

```
  // Prepare to access the audio spectrograms from a microphone or other source
  // that will provide the inputs to the neural network.
  static FeatureProvider static_feature_provider(kFeatureElementCount,
                                                 model_input->data.uint8);
  feature_provider = &static_feature_provider;
```

次に、`RecognizeCommands` インスタンスを作成し、`previous_time` 変数を初期化する：

```
  static RecognizeCommands static_recognizer(error_reporter);
  recognizer = &static_recognizer;

  previous_time = 0;
}
```

次は、`loop()` 関数の番だ。前の例と同じように、この関数は何度も何度も無限に呼び出される。ループでは、まず特徴プロバイダーを使ってスペクトログラムを作成する：

```
void loop() {
  // Fetch the spectrogram for the current time.
  const int32_t current_time = LatestAudioTimestamp();
  int how_many_new_slices = 0;
  TfLiteStatus feature_status = feature_provider->PopulateFeatureData(
      error_reporter, previous_time, current_time, &how_many_new_slices);
  if (feature_status != kTfLiteOk) {
    error_reporter->Report("Feature generation failed");
    return;
  }
  previous_time = current_time;
  // If no new audio samples have been received since last time, don't bother
  // running the network model.
  if (how_many_new_slices == 0) {
    return;
  }
```

前回の反復から新しいデータがなければ、推論を実行する必要はない。

入力ができたら、あとはインタプリタを呼び出すだけだ：

```
  // Run the model on the spectrogram input and make sure it succeeds.
  TfLiteStatus invoke_status = interpreter->Invoke();
  if (invoke_status != kTfLiteOk) {
    error_reporter->Report("Invoke failed");
    return;
  }
```

モデルの出力テンソルは、各カテゴリーの確率で満たされた。これを解釈するために、`RecognizeCommands` インスタンスを使う。出力テンソルへのポインタを取得し、`ProcessLatestResults()` の出力を受け取るためにいくつかの変数をセットアップする：

```
  // Obtain a pointer to the output tensor
  TfLiteTensor* output = interpreter->output(0);
  // Determine whether a command was recognized based on the output of inference
  const char* found_command = nullptr;
  uint8_t score = 0;
  bool is_new_command = false;
  TfLiteStatus process_status = recognizer->ProcessLatestResults(
      output, current_time, &found_command, &score, &is_new_command);
  if (process_status != kTfLiteOk) {
    error_reporter->Report("RecognizeCommands::ProcessLatestResults() failed");
    return;
  }
```

最後に、単語が検出されたらユーザに通知できるように、コマンドレスポンダの`RespondToCommand()` メソッドを呼び出す：

```
  // Do something based on the recognized command. The default implementation
  // just prints to the error console, but you should replace this with your
  // own function for a real application.
  RespondToCommand(error_reporter, current_time, found_command, score,
                   is_new_command);
}
```

これで終わりだ！`RespondToCommand()` への呼び出しがループの最後だ。特徴生成以降のすべての処理は、音声に既知の単語が含まれていないかチェックし、単語が確認されたら何らかの出力を出す、ということを延々と繰り返すことになる。

`setup()` と`loop()` 関数は、`main.cc` で定義されている`main()` 関数によって呼び出される。この関数は、アプリケーションの起動時にループを開始する：
