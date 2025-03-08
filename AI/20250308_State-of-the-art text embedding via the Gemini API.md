# State-of-the-art text embedding via the Gemini API
- Google Developers blogの3/7の記事
- https://developers.googleblog.com/ja/gemini-embedding-state-of-the-art-text-embedding-via-the-gemini-api/?_gl=1*1yra8j3*_up*MQ..*_ga*MTI0NTAzNzQwLjE3NDE0MjI4MzY.*_ga_H733Y2BZES*MTc0MTQyMjgzNi4xLjAuMTc0MTQyMjgzNi4wLjAuMA..

# 要約
## Gemini Embeddingモデルの概要
- 新しい実験的モデル: `gemini-embedding-exp-03-07`が登場。Geminiモデル自体をトレーニングに用い、前モデル`text-embedding-004`を上回る性能を発揮しています。
- MTEBリーダーボード: 多言語評価ベンチマークでトップランクを獲得（平均スコア68.32、+5.81の差）し、幅広いタスクで高いパフォーマンスを示しています。

## embeddingの利点と応用
- テキストの意味理解: セマンティックな情報を数値表現に変換することで、類似するデータ同士の近さを実現。
- 主な応用例:
	- 効率的な情報検索: 大規模データベースから関連文書の抽出
	- RAG: 関連情報を取り込んだ生成タスクの品質向上
	- クラスタリング・分類: テキストのグルーピングや自動分類（例: 感情分析、スパム検出）
	- テキスト類似性: 重複コンテンツや類似コンテンツの特定

## 開発者向け実装方法と特徴
- API利用: 既存の`embed_content`エンドポイントで利用可能。Pythonコードのサンプルが示されています。
- 主要な機能・特徴:
	- 入力トークンの上限: 8Kトークンまで対応、より長いテキストやコードも処理可能
	- 出力次元: 3K次元の高次元表現（前モデルの約4倍）
	- `Matryoshka Representation Learning` (MRL): 出力次元を必要に応じてトランケートし、ストレージコストを最適化
	- 言語サポートの拡大: サポート言語数が100以上に増加
	- 統一モデル: 多言語、英語専用、コード専用の各タスクに特化した旧モデルを上回る性能

# 今後の展望
- 実験段階: 現在は実験的なリリースであり、容量は限定されていますが、ユーザーからのフィードバックを受けながら改良が進められる予定です。
- 安定版への移行: 数か月内に広く一般利用可能な安定版のリリースを目指しています。