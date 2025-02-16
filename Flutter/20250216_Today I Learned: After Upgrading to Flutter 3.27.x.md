# Today I Learned: After Upgrading to Flutter 3.27.x
- Mediumの記事
- https://benamorn.medium.com/today-i-learned-after-upgrading-to-flutter-3-27-x-d7a267557dd1

# 所感
Flutter 3.27.Xの破壊的変更がまとめられていて素晴らしい。以下要約

1. Deep Linking
   1. Flutterがデフォルトで`Deep Linking`を処理するようになったので`uni_link`などサードパーティ製のライブラリを使っている場合は挙動が変わる可能性があり注意
2. Android 15 Edge-to-Edge UI
   1. SafeAreaが機能していれば問題なさそうだが、ネイティブビューを使っている場合に注意が必要。
3. Gradle Warning
   1. Gradleの旧設定が今後非推奨になるので、移行の準備が必要。
4. Impeller on Android（Opt-in by Default）
   1. まだ不安定なので、無効化しておくのが無難。
5. OppoのPowerVR GPUの問題
   1. Oppo端末の`PowerVR GPU`で問題がある。気になる場合はFlutterのバグトラッカー（Issue #162303）をウォッチしておくのが良さそう
6. Flutter 3.24 がiOS 18.2のWebViewバグ修正に必須
   1. iOS 18.2 で WebView を使う場合、Flutter 3.24 以上が必要になる。Flutter Issue #160208 で詳細を追うべき

# DeepLinkについて
[Flutter DocsのDeep links flag change](https://docs.flutter.dev/release/breaking-changes/deep-links-flag-change?source=post_page-----d7a267557dd1---------------------------------------)には`flutter_deeplinking_enabled`がデフォルトでtrueになったとある。
もともとSDKにDeepLinkの機能があった？`app_links`など使う必要がなかった？→これは違う

→GPT4oの回答
> Flutterは元々、Navigator 2.0（2020年導入）からWebやモバイルでのDeep Linkingに対応する仕組みを持っていた。  
具体的には`onGenerateRoute`や`go_router`を使用してURLベースのナビゲーションができるようになっていた。  
ただし、ネイティブの`Deep Linking`（Androidのintent-filterやiOSのURL Schemes, Universal Links）とは別物だったため、多くの開発者はサードパーティのライブラリを使って`Deep Link`を処理していた。
（例: uni_links, app_links, firebase_dynamic_links）

確かにIssue #52350「Set deep linking flag to true by default」を読む限り、Navigator2.0の問題点を解決するためにtrueにしたようだ。

1. pushRouteInformation の挙動の問題
   1. `pushRouteInformation`という関数は新しいルート（URL）をFlutterのナビゲーションシステムに送る役割がある。
   2. しかしこれが「成功したかどうか」を正しく判定するのが難しかった。特に、iOSの`UIApplication.openURL`は非同期APIになっている。
   3. その結果「Deep Linkingの処理を開始したけど、実際に遷移したかどうかわからない」といった状況が発生していた。
2. iOSエンジン側でのナビゲーション情報管理の改善
   1. `Flutter Web`では`routeInformationUpdated`というメッセージを`navigationChannel`に送る仕組みがあるが、iOSではこれが適用されていなかった。
   2. そのため「Deep LinkでURLを受け取ったが、実際にナビゲーションが更新されたのか？」を確認する方法が存在しなかった。
3. FlutterのDeep Linkingを有効化することで、ナビゲーションの一貫性を保ちやすくなる
   1. 今までは`flutter_deeplinking_enabled = false`だったため、デフォルトではDeep LinkがFlutterのナビゲーションシステムに統合されていなかった。
   2. それをtrueにすることでDeep LinkがFlutterのナビゲーションシステムに統合され「Flutterのナビゲーションとして適切に処理される」ことを保証しやすくなる。
