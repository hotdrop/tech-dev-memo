# What’s new in Flutter 3.29
- Mediumの記事
- https://medium.com/flutter/whats-new-in-flutter-3-29-f90c380c2317

# 要約
1. Framework（CupertinoとMaterial）の変更
  - たくさんあったので下に列挙しました。
2. Text Selection
  - テキスト選択の状態を`SelectionListener`で取得できるようになった
  - 選択範囲（開始位置・終了位置・選択有無など）の詳細を取得可能になった
  - 親の`SelectionArea`の状態を`SelectableRegionSelectionStatusScope`で確認できるようになった
3. アクセシビリティ改善
  - Form のエラーメッセージは「最初の1つだけ」読み上げるようになった
  - DropdownMenu のラベルをスクリーンリーダーが正しく読み上げるように修正
4. Web
   1. 今までFlutterの`WASM`サポートでは特定のHTTPレスポンスヘッダーを設定する必要があったが、そのヘッダーなしでWASMアプリが動作するようになった。
      1. ただし単一スレッドのみで動作する。複数スレッドで動作させたい場合は、従来どおりHTTPヘッダーの設定が必要。
   2. 画像レンダリング（WebGL バックエンド）の改善
      1. 画像のデコード処理を UI スレッドの外で実行するようになり、カクつきが減少したとのこと
   3. これまでCORS制約によりネットワーク画像の扱いが面倒だったが、`Image.network`がデフォルトでCORS画像を適切に処理するようになった。
5. Engine
   1. 長いので下に書きました。
6. DevTools & IDE アップデート
   1. デフォルトで新しい DevTools インスペクターが有効化
   2. DevTools のウィジェット選択モードを有効にすると、デバイス上のウィジェット選択が「モード終了まで継続」するようになった
   3. DevTools の 「ログ」ツールが大幅に強化された
7. 破壊的変更 & 非推奨機能
   1. Flutter チームがメンテナンスを停止するパッケージ一覧: `ios_platform_images`, `css_colors`, `palette_generator`, `flutter_image`, `flutter_adaptive_scaffold`, `flutter_markdown`
   2. Flutter Gradle プラグインの「スクリプト適用」廃止
      1. 影響を受けるのはFlutter 3.16 以前に作成されたプロジェクトで、もしFlutterビルド時に 「You are applying Flutter’s main Gradle plugin imperatively」 という警告が出たら影響を受けているので公式の移行ガイドを確認して修正しましょうとのこと。
   3. `Flutter Web`の`HTML Renderer`が削除
       1. デフォルトの`WebGL/CanvasKit`レンダラーを使用するように変更
    4. Web 画像のハンドリング改善
    5. Material テーマの正規化

## Cupertinoの変更点列挙
- `CupertinoNavigationBar`/`CupertinoSliverNavigationBar`にbottomウィジェットが追加
  - `CupertinoSliverNavigationBar`のスクロール挙動改善
  - `CupertinoNavigationBar.large`コンストラクタが追加
    - 静的なナビゲーションバーで、大きなタイトルを表示できるようになった
  - `Cupertino`ポップアップの背景ブラーが改善
    - 背景のブラーエフェクトが強化されiOSのネイティブUIとより一貫性が増した。
  - `CupertinoSheetRoute`の追加
    - iOSスタイルのモーダルシート（下からスライドインするシート）を表示できる新しいルートが追加
    - `showCupertinoSheet`関数が追加され簡単にモーダルシートを表示可能
    - ドラッグで閉じる（`drag-to-dismiss`）ジェスチャーをデフォルトでサポート
    - →今までは`showModalBottomSheet`をカスタマイズする必要があったがネイティブのiOSライクな動作を簡単に実装できるようになった
  - `CupertinoAlertDialog`のダークモード改善
  - テキスト選択のハンドル＆マグニファイア（拡大鏡）の改善
    - iOSで「選択ハンドルの向きが逆になる（inverted）」場合に対応
    - テキスト選択の拡大鏡の枠線色が、現在のテーマに適応するようになった

## Materialの変更点列挙
- 新しい`FadeForwardsPageTransitionsBuilder`
  - Material 3（M3）に準拠したページ遷移アニメーションが追加。`ZoomPageTransitionsBuilder`のパフォーマンス問題を解決する目的でもあるようだ
  - 新しいページ遷移の動作
    - 新しいページは「右から左にスライドしながらフェードイン」、古いページは「右から左にスライドしながらフェードアウト」
  - `CircularProgressIndicator` & `LinearProgressIndicator` の更新
     - Material Design 3 の最新仕様に準拠。M3の新デザインをOFFにする方法もある
  - Slider のデザイン更新。新しいトラックとハンドルデザインを採用したとのこと
  - Chip・Tooltip・ReorderableListView に mouseCursor を追加
     - マウスホバー時のカーソルをカスタマイズ可能になった。`ReorderableListView`でのホバー時のUX改善に役立ちそう
  - バグ修正
    - `DropdownMenu.onSelected`がキーボードナビゲーションで正しく発火するように修正
    - `TabBar` のエラスティックアニメーションが改善
    - `RangeSlider`のサム（つまみ）の配置やパディング、角の丸みが調整


## Engineの改善
### `Vulkan`の改善点
1. 古い`Vulkan`デバイスで発生していたフリッカー（画面ちらつき）や視覚的なジッターを修正
2. `Android Hardware Buffer swapchains`を問題が解決されるまで一時的に無効化
3. `MediaTek`/`PowerVR SoC`の`Vulkan`サポートを無効化。ブラックスクリーンやクラッシュの原因になっていたため。これらのデバイスは`OpenGLES`バックエンドを使用するよう変更
4. Androidエミュレータはデフォルトで`OpenGLES`バックエンドを使用

### `Impeller OpenGLES`の導入
1. `Vulkan`が動作しないデバイスはデフォルトで`Impeller OpenGLES`にフォールバック
2. 従来は`Skia`を使っていたが今後は`OpenGLES`ベースの`Impeller`を採用

### `Impeller`がiOSで完全に`Skia`を置き換え
1. iOSバックエンドから`Skia`サポートが完全に削除
2. `FLTEnableImpeller`フラグ（Impeller をオフにするオプション）も廃止
3. 今後のリリースで`Skia`の関連コードをさらに削除しバイナリサイズを縮小予定

### その他新機能
- `BackdropFilter`を複数使う場合`BackdropGroup`を使うとパフォーマンスが向上するようだ
- `Skia`よりも優れたブラーエフェクトの最適化が可能
- 子ウィジェットにカスタムシェーダーを適用できる新しい`ImageFilter`コンストラクタ。従来の `AnimatedSampler`と同様の機能を提供し`BackdropFilter`にも適用可能

### Android/iOSのスレッドモデル変更  
これまで`Dart`コードの実行スレッドはUIスレッド（別スレッド）だったがメインスレッドに変更した。UIスレッドで実行する場合、UIとプラットフォームコードの並列実行が可能で、高スループットが期待できたが、プラットフォームとの相互運用（interop）が面倒で、非同期メッセージのシリアライズが必要だった。

実行がメインスレッドになると、プラットフォームの API 呼び出しが「同期的」に実行可能になり、非同期メッセージのオーバーヘッドが削減できる。また、テキスト入力、アクセシビリティ、プラグインのパフォーマンスが向上するようだ。

懸念点としてメインスレッドでDartコードを実行するため、計算負荷の高い処理がUIのカクつきを引き起こす可能性がある。そのため負荷の高い処理は`Isolates`を活用するのが推奨されている。（コメントにて）

### 疑問点: FlutterはUIスレッドとメインスレッドが別物なのか？
Androidは`Zygote`が`fork`したプロセスの最初のスレッド（アプリを起動するスレッド）がUIスレッドでありメインスレッドだと認識している。UIスレッドは絶対止めてはいけないので負荷の高い処理やビジネスロジックは`WorkerThread`（バックグラウンドスレッド）を使う。以前の`RxJava`や`Kotlin Coroutine`で普通にやっている。

**Flutter のスレッド構成**  
Flutterのスレッドモデルは以下の通りで、メインスレッドとUIスレッドは別物
- `Platform Thread`: プラットフォーム（Android/iOS）のメインスレッド
- `UI Thread`: FlutterのUIを描画する「UIスレッド」。従来はDartコードも実行していた
- `Raster Thread`: レンダリング処理を担当する「Raster スレッド」
- `Dart Worker Threads`: Dart の並列処理をする「Isolate スレッド」

`Flutter 3.29`までは、`Dart`コードはFlutterのUIスレッド（Android のメインスレッドとは別）で実行されていた。	ネイティブとのやり取りには非同期メッセージ（`Platform Channels`）を使う必要があり、`MethodChannel` でネイティブと通信する際にAndroid側とFlutter側でスレッドを跨いでいた。

`Flutter 3.29`からは、`Dart`コードはAndroidのメインスレッド（UI スレッド）で直接実行するようになった。これにより**動機的に実行できる・・つまりUIに関する**ネイティブコードとの連携時に`Platform Channels`を使わず同期的に呼び出せるようになった。パフォーマンス向上 & レスポンス遅延の解消も計れる。
→UIKit（iOS）やView（Android）の操作の部分でそれ以外はこれまで通り`Platform Channels`での処理が必要

最終的にこうなるようだ  
- `メインスレッド（Platform & UI Thread）`: Dartコード実行、UI操作（Android: Activity / iOS: UIKit）、ネイティブAPI呼び出し
- `Raster スレッド（GPU レンダリング）`: `Impeller`によるレンダリング処理（`Skia`はiOSでは削除）
- `Worker スレッド（Isolate Threads）`: 重い計算処理、非同期処理（例: ネットワーク、DB、画像処理）