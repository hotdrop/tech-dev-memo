# Announcing Dart 3.7
- Mediumの記事
- https://medium.com/dartlang/announcing-dart-3-7-bf864a1b195c

# 要約
1. Macrosは開発中止。ただし`build_runner`のパフォーマンス改善やJSONのシリアライズ/デシリアライズの改善につながったとのこと。
2. ローカル変数`_`はワイルドカード変数として仕様変更した。
3. `Dart Formatter`が新しいフォーマットスタイルになった。
4. コードの品質向上や開発効率を上げるために新しいクイックフィックスやアシスト（補助機能）が追加された。
5. DartのWebプラットフォームの進化
6. `pub.dev`にて、どのバージョンがどれくらい使われているか可視化されるようになった
   1. →最新バージョンへの移行率を確認できる
7. `pub.dev`にダークモードが追加
8. 検索時に「IDE風のオートコンプリート」が追加

## 2. _のワイルドカード変数化
変数名の衝突を気にせず複数の_を使えるようになった。以前のように`_.toString()`のようなコードは動かなくなった。設計的にはスッキリする。
```dart
void announceFailure(Future<void> future) {
  future.onError((_, _) { // <--
    print('Error!');
  });
}
```

## 3. Dart Formatterの新しいフォーマットスタイルについて
1. 「トレーリングカンマ（,）」の自動追加・削除
   1. 以前は「カンマがあると改行され、カンマがないと一行に詰める」という仕様だったが、`Dart 3.7`からは引数が複数ある場合、改行時にカンマを追加するようになった。コードの整形をより一貫性のあるものにする改善
2. プロジェクト単位で`page_width`を設定できるようになった。`analysis_options.yaml`に指定可能
3. 特定のコード領域をフォーマット対象外にできるようになった。
4. `dart format --fix` が廃止され`dart fix`を使うようになった

## 4. 新しいクイックフィックスやアシスト機能
- クイックフィックス
  - awaitの付け忘れ修正
  - importで別名をつけるときのミスを検出し、適切な形に修正
  - `cascade_invocations`のLint対応
- アシスト
  - ネストが深くなりすぎないよう、適切に`else`ブロックを`else if`に変換してくれる
  - `Expanded`や`Flexible`でウィジェットを包む(Futter)
- 新しいLintルール
  - `unnecessary_underscores`の追加。`_`を使わなくていい場所で使っていたら指摘してくれる。

## 5. Webプラットフォームの進化
- JavaScriptとの相互運用の標準化
  - 1年前に新しいJavaScriptやブラウザAPIと連携するライブラリを導入したが、`Dart 3.7`では `dart:js_interop`(JavaScriptとの相互運用用)と`package:web`(ブラウザAPI用)を推奨する形になった。
- レガシーWebAPIの廃止
  - `dart:html`, `dart:indexed_db`, `dart:js`, `dart:js_util`, `dart:web_audio`, `dart:web_gl`が2025年末までに完全削除予定。新しいAPIへの移行が必要。
  - →記事には`Seven Dart SDK libraries`と書いてあるが列挙されているものは6つ、リンクの` JavaScript interoperability page`は8つ書いてある。とりあえず自身が利用しているWebAPIが廃止されるか確認して移行するのが良さそう