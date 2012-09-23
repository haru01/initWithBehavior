# テストの手順
このページでは実際にテストを行って行く過程でのその流れを記述しています。
テスト駆動開発における基本的なサイクルは
1. テストの作成
2. 実装
3. リファクタリング
です。

今回はBrowseGithubの機能の一つである、フィードの表示を作った時の一連の流れを例にしています。

## 仕様を考える
まずは仕様を考えましょう、フィードを表示するために必要な要素は

* 表示用のViewController
* 表示用のUITableViewCell
* フィードのモデル
* 複数のフィードを取りまとめるマネージャ
* フィードを取得するクライアント
* 取得したXMLをモデルへ変換するパーサー

くらいでしょうか。Github OAuth など、基本的な処理はすで済んでいる状態と考えています。
これらをクラスに落とし込むと、

* BGFeedViewController
* BGFeedEntryCell
* BGFeedEntry
* BGFeedEntryManager
* BGFeedClient
* BGFeedParser

という具合になり、それぞれの依存関係は以下の図のようになるはずです。

(fig.1)

一番依存するものがなく、単一で動作するBGFeedEntryから順に実装していくことにしましょう。

## BGFeedEntry
BGFeedEntry
