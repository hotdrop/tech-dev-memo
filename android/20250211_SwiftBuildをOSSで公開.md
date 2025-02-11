# Apple、Swift Build をオープンソースで公開
AndroidDagashiの2月9日の記事より
- URL: https://www.swift.org/blog/the-next-chapter-in-swift-build-technologies/

# SwiftPMとは？
`SwiftPM`がよくわかっていなかったので調査メモ。
`CocoaPods`のように外部ライブラリの管理をするので依存関係の解決/ライブラリダウンロードができるが、それ以外にもビルドシステムも組み込まれていて`swift build`コマンドでSwiftプロジェクトをビルドできる。そのため、Xcodeなしでターミナルからコンパイルや実行ができる。`CocoaPods`と違ってApple公式サポート。
ただ、大きな問題があって`SwiftPM`は独自の簡易ビルドシステムになっていてXcodeのビルドシステムとは別物だったため、それぞれが異なった動きをすることがあったようだ。Xcodeではビルドが通るが`SwiftPM`では通らないなど・・これは辛い。

# 要約
統一ビルドシステムの導入
- これまで`Xcode`と`Swift Package Manager(SwiftPM)`のビルド方式が異なり動作がズレることがあった。
- 今回の変更で`Xcode`の強力なビルドエンジンが`SwiftPM`にも組み込まれることで統一的なビルド環境が実現する
- これにより`SwiftPM`を使う開発者（特に Linux & Windows ユーザー）も`Xcode`レベルのビルド機能を利用できるようになる。

クロスプラットフォーム開発が加速
- Mac だけでなく Windows や Linux でも`SwiftPM`を通じてビルドが簡単にできるようになる
- Mac環境に依存しなくて済むのでCI/CDの構築が容易になる可能性がある
- 他のIDE、例えば`VSCode`などでもSwift開発がしやすくなる可能性がある

コミュニティとの協力
- GitHubで`Swift Build`のリポジトリが公開 されている（PRやIssueの投稿が可能）
- Swiftフォーラムでのディスカッションを推奨
- AppleがSwiftの開発をよりオープンソースコミュニティと連携する姿勢を見せている
