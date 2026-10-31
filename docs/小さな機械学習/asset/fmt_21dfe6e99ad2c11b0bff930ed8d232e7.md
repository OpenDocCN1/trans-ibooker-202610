
この警告を記録するために、`error_reporter` の `Report()` メソッドを呼び出す。`error_reporter` もポインタなので、`->` 演算子を使って `Report()` にアクセスする。

`Report()` メソッドは、よく使われるC++のメソッド、`printf()` と同様の動作をするように設計されている。最初のパラメータとして、ログに記録したい文字列を渡す。この文字列には、`%d` フォーマット指定子が2つ含まれており、メッセージが記録されるときに変数が挿入されるプレースホルダーとして機能する。次の2つのパラメータは、モデルのバージョンとTensorFlow Liteスキーマのバージョンだ。これらは、`%d` 文字を置き換えるために、順番に文字列に挿入される。

### 注

`Report()` メソッドは、変数の種類ごとにプレースホルダとして機能するさまざまなフォーマット指定子をサポートしている。`%d` は整数のプレースホルダとして、`%f` は浮動小数点数のプレースホルダとして、`%s` は文字列のプレースホルダとして使用する。

## AllOpsResolverの作成

ここまでは順調だ！コードはエラーを記録できるし、モデルを便利な構造体にロードして、それが互換性のあるバージョンであることをチェックした。C++の概念を復習していることもあり、少しゆっくりではあるが、物事は理解できるようになってきた。次は、`AllOpsResolver` のインスタンスを作成する：

```
// This pulls in all the operation implementations we need
tflite::ops::micro::AllOpsResolver resolver;
```

[`all_ops_resolver.h`](https://oreil.ly/O0qgy) で定義されているこのクラスは、TensorFlow Lite for Microcontrollersインタプリタが演算子にアクセスするためのものである。

[第3章](#ch03.html#chapter_get_up_to_speed)で、機械学習モデルは、入力を出力に変換するために連続して実行されるさまざまな数学演算子で構成されていることを学んだ。`AllOpsResolver` クラスは、TensorFlow Lite for Microcontrollers で利用可能なすべての演算子を知っており、それらをインタプリタに提供することができる。

## テンソルアリーナを定義する

インタプリタを作成するための材料はほぼ揃った。最後に必要なのは、モデルの実行中に必要となるワーキングメモリの領域を確保することだ：

```
// Create an area of memory to use for input, output, and intermediate arrays.
// Finding the minimum value for your model may require some trial and error.
const int tensor_arena_size = 2 × 1024;
uint8_t tensor_arena[tensor_arena_size];
```

コメントにあるように、このメモリ領域はモデルの入力、出力、中間テンソルを格納するために使われる。我々はこれをテンソルアリーナと呼んでいる。我々の場合、2,048バイトの配列を割り当てた。式で指定する。 `2 × 1024`.

では、テンソル・アリーナはどのくらいの大きさにすべきなのか？それはいい質問だ。残念ながら、単純な答えはない。モデルアーキテクチャによって、入力、出力、中間テンソルのサイズや数が異なるので、どれだけのメモリが必要かを知るのは難しい。しかし、マイクロコントローラーのRAMには限りがあるため、プログラムの残りのスペースを確保するために、できるだけ小さなメモリにしておく必要がある。

これは試行錯誤でできる。配列のサイズを `n` `× 1024` と式で表すのはそのためだ。 `n` で表す。正しい配列サイズを発見するためには、かなり大きな数値から始めると、確実に機能する。本書の例で使われている最も大きな数値は `70 × 1024` である。次に、モデルが動かなくなるまで、数を減らしていく。最後に機能した数が正しい数である！

## インタプリタを作成する

`tensor_arena` を宣言したので、インタプリタをセットアップする準備ができた。以下はその様子だ：

```
// Build an interpreter to run the model with
tflite::MicroInterpreter interpreter(model, resolver, tensor_arena,
                                     tensor_arena_size, error_reporter);

// Allocate memory from the tensor_arena for the model's tensors
interpreter.AllocateTensors();
```

まず、`interpreter` という名前の `MicroInterpreter` を宣言する。このクラスはTensorFlow Lite for Microcontrollersの心臓部であり、我々が提供したデータに対してモデルを実行する魔法のようなコードだ。これまでに作成したオブジェクトのほとんどをコンストラクタに渡し、`AllocateTensors()` を呼び出す。

前のセクションでは、`tensor_arena` と呼ばれる配列を定義することで、メモリ領域を確保した。`AllocateTensors()` メソッドは、モデルで定義されたテンソルをすべてウォークスルーし、`tensor_arena` からそれぞれのテンソルにメモリを代入する。`AllocateTensors()` 、推論を実行する前に呼び出すことが重要だ。そうしないと推論が失敗してしまうからだ。

## 入力テンソルを検査する

インタプリタを作成したら、モデルに入力を与える必要がある。そのために、入力データをモデルの入力テンソルに書き込む：

```
// Obtain a pointer to the model's input tensor
TfLiteTensor* input = interpreter.input(0);
```

入力テンソルへのポインタを取得するには、インタプリタの `input()` メソッドを呼び出す。モデルは複数の入力テンソルを持つことができるので、どのテンソルが欲しいかを指定するインデックスを `input()` メソッドに渡す必要がある。このモデルの場合、入力テンソルは1つだけなので、そのインデックスは `0` となる。

TensorFlow Liteでは、テンソルは `TfLiteTensor` 構造体で表現される。この構造体は [`c_api_internal.h`](https://oreil.ly/Qvhre) で定義されている。この構造体は、テンソルを操作したり学習したりするためのAPIを提供する。次のコードでは、この関数を使ってテンソルが正しく見えるかどうかを検証する。テンソルをたくさん使うことになるので、`TfLiteTensor` 構造体の動作に慣れるために、このコードを見ていこう：

```
// Make sure the input has the properties we expect
TF_LITE_MICRO_EXPECT_NE(nullptr, input);
// The property "dims" tells us the tensor's shape. It has one element for
// each dimension. Our input is a 2D tensor containing 1 element, so "dims"
// should have size 2.
TF_LITE_MICRO_EXPECT_EQ(2, input->dims->size);
// The value of each element gives the length of the corresponding tensor.
// We should expect two single element tensors (one is contained within the
// other).
TF_LITE_MICRO_EXPECT_EQ(1, input->dims->data[0]);
TF_LITE_MICRO_EXPECT_EQ(1, input->dims->data[1]);
// The input is a 32 bit floating point value
TF_LITE_MICRO_EXPECT_EQ(kTfLiteFloat32, input->type);
```

最初に気づくのは、`TF_LITE_MICRO_EXPECT_NE` と `TF_LITE_MICRO_EXPECT_EQ` という2つのマクロだ。これらのマクロは、TensorFlow Lite for Microcontrollersテストフレームワークの一部であり、変数の値についてアサーションを行い、変数が特定の期待値を持つことを証明することができる。

例えば、`TF_LITE_MICRO_EXPECT_NE` というマクロは、呼び出された2つの変数が等しくないことを保証するように設計されている（そのため、`_NE` という名前の部分は Not Equal の略である）。変数が等しくなければ、コードは実行され続ける。等しい場合はエラーが記録され、テストは失敗したとマークされる。

最初にチェックするのは、入力テンソルが実際に存在するかどうかである。これは、`nullptr` と等しくないことを保証する。 は、実際にはどのデータも指していないポインタを表すC++の特殊化値である：

```
TF_LITE_MICRO_EXPECT_NE(nullptr, input);
```

次にチェックするのは、入力テンソルの形状である。[第3章](#ch03.html#chapter_get_up_to_speed)で説明したように、すべてのテンソルはその次元を表す形状を持っている。我々のモデルへの入力はスカラー値（単一の数値を意味する）である。しかし、[Kerasのレイヤーが入力を受け入れる方法の](https://oreil.ly/SFiRV)ため、この値は1つの数値を含む2Dテンソルの中で提供されなければならない。入力が0の場合、以下のようになる：

```
[[0]]
```

入力スカラー0が2つのベクトルで包まれていることに注意しよう。

`TfLiteTensor` 構造体は、テンソルの次元を記述する `dims` メンバを含む。このメンバは `TfLiteIntArray` 型の構造体で、[`c_api_internal.h`](https://oreil.ly/Qvhre)で定義されている。`size` メンバはテンソルの次元数を表す。入力テンソルは2次元であるべきなので、`size` の値は `2` であると断言できる：

```
TF_LITE_MICRO_EXPECT_EQ(2, input->dims->size);
```

さらに `dims` 構造体を調べて、テンソルの構造が期待通りであることを確認することができる。`data` 変数は、各次元に1つの要素を持つ配列である。各要素はその次元のサイズを表す整数である。我々は各次元に1つの要素を含む2次元テンソルを期待しているので、両方の次元に1つの要素を含むことを保証できる：

```
TF_LITE_MICRO_EXPECT_EQ(1, input->dims->data[0]);
TF_LITE_MICRO_EXPECT_EQ(1, input->dims->data[1]);
```

これで入力テンソルの形が正しいことを確信できる。最後に、テンソルはさまざまな型のデータ（整数、浮動小数点数、ブール値など）から構成されることがあるので、入力テンソルの型が正しいことを確認しよう。

テンソル構造体の `type` 変数は、テンソルのデータ型を知らせてくれる。ここでは、定数 `kTfLiteFloat32` で表される32ビットの浮動小数点数を提供する：

```
TF_LITE_MICRO_EXPECT_EQ(kTfLiteFloat32, input->type);
```

完璧だ。これで入力テンソルは、入力データ（1つの浮動小数点値）に対して正しいサイズと形状であることが保証された。これで推論を実行する準備が整った！

## 入力に対して推論を実行する

推論を実行するには、入力テンソルに値を追加し、インタプリタにモデルの起動を指示する必要がある。その後、モデルが正常に実行されたかどうかをチェックする。これがその様子だ：

```
// Provide an input value
input->data.f[0] = 0.;

// Run the model on this input and check that it succeeds
TfLiteStatus invoke_status = interpreter.Invoke();
if (invoke_status != kTfLiteOk) {
 error_reporter->Report("Invoke failed\n");
}
TF_LITE_MICRO_EXPECT_EQ(kTfLiteOk, invoke_status);
```

TensorFlow Liteの `TfLiteTensor` 構造体には、`data` 変数があり、これを使用して入力テンソルの内容をセットすることができる。これが使われているのをここで見ることができる：

```
input->data.f[0] = 0.;
```

`data` 変数は `TfLitePtrUnion` 、ユニオンである。ユニオンはC++の特殊データ型で、異なるデータ型をメモリの同じ場所にストアすることができる。与えられたテンソルは多くの異なるデータ型（例えば浮動小数点数、整数、ブール）のいずれかを含むことができるので、ユニオンはそれをストアするのに最適なデータ型である。

`TfLitePtrUnion` ユニオンは [`c_api_internal.h`](https://oreil.ly/v4h7K) で宣言されている：

```
// A union of pointers that points to memory for a given tensor.
typedef union {
  int32_t* i32;
  int64_t* i64;
  float* f;
  TfLiteFloat16* f16;
  char* raw;
  const char* raw_const;
  uint8_t* uint8;
  bool* b;
  int16_t* i16;
  TfLiteComplex64* c64;
  int8_t* int8;
} TfLitePtrUnion;
```

たくさんのメンバがあり、それぞれが特定の型を表しているのがわかるだろう。各メンバーはポインターであり、データがストアされるべきメモリ上の場所を指すことができる。先ほどのように `interpreter.AllocateTensors()` を呼び出すと、適切なポインタがセットされ、テンソルがデータをストアするために割り当てられたメモリ・ブロックを指すようになる。各テンソルは特定のデータ型を持っているので、対応する型のポインタだけがセットされる。

つまり、データをストアするには、`TfLitePtrUnion` の適切なポインタを使用すればよい。例えば、テンソルの型が `kTfLiteFloat32` の場合、`data.f` を使う。

ポインターはメモリーのブロックを指しているので、ポインター名の後に角括弧（`[]` ）を使って、どこにデータをストアするかをプログラミングに指示することができる。この例では次のようにする：

```
input->data.f[0] = 0.;
```

代入する値は `0.` と表記されるが、これは `0.0` の省略形である。小数点を指定することで、この値が整数ではなく浮動小数点数であることをC++コンパイラに明確にしている。

この値を `data.f[0]` に代入しているのがわかるだろう。 これは、割り当てられたメモリ・ブロックの最初の項目として代入していることを意味する。値が1つしかないことを考えると、必要なことはこれだけだ。

入力テンソルをセットアップしたら、推論を実行しよう。これはワンライナーだ：

```
TfLiteStatus invoke_status = interpreter.Invoke();
```

`interpreter` で `Invoke()` を呼び出すと、TensorFlow Liteインタプリタがモデルを実行する。モデルは、入力データを出力に変換するためにインタプリタが実行する数学演算のグラフで構成される。この出力はモデルの出力テンソルに格納されるが、これについては後で詳しく説明する。

`Invoke()` メソッドは `TfLiteStatus` オブジェクトを返し、推論が成功したか問題があったかを知ることができる。その値は `kTfLiteOk` か `kTfLiteError` のどちらかである。エラーの有無をチェックし、エラーがあればそれを報告する：

```
if (invoke_status != kTfLiteOk) {
    error_reporter->Report("Invoke failed\n");
}
```

最後に、テストに合格するためには、ステータスが `kTfLiteOk` ：

```
TF_LITE_MICRO_EXPECT_EQ(kTfLiteOk, invoke_status);
```

これで推論が実行された！次は出力を取得して、見た目が良いことを確認する。

## 出力を読み取り

入力と同様、このモデルの出力は `TfLiteTensor` 、それへのポインタを取得するのも簡単だ：
