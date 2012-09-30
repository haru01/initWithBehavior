---
layout: default
---
# 第一章:BDDに必要なものをそろえる

この章では、Kiwiの準備や動作確認、簡単なサンプルを使ったKiwiの記述スタイルを紹介します。
環境はMacOS10.8、Xcode4.5を想定しています。

## 新規プロジェクトの作成
KiwiはSenTestingKitの上で動作するテストフレームワークのため、まずはSenTestingKitを有効にしながらプロジェクトを作成します。
後からターゲットの追加も可能ですが、ここではそれは扱わないため、他のドキュメント等を参考にしてください。

SenTestingKitでのテストを有効にする場合、プロジェクト作成時に "Include Unit Tests" へチェックを入れます。

<img src="../static/images/new_project.png" />

こうすることで、SenTestingKitを動作させる準備が整います。プロジェクトの情報を確認すると "プロジェクト名Tests" という名前でテスト用のターゲットが作成されているはずです。

<img src="../static/images/after_new_project.png" />

そして、「Command+U」でテストが実行されます。プロジェクト作成時にテストが一件作成されるので、それがきちんと動いているかを確認してください。
うまく動作すれば、次はKiwiの準備に入ります。

## Kiwiの準備

Kiwiは公式ではgit-submoduleを使ってプロジェクトにコードを取り込んで利用することを推奨しています。
ここでは、その手順にそってKiwiをのセットアップを行います。
ただし、CocoaPodsに慣れている方は、CocoaPodsにKiwiが存在しますので、そちらを使うことをお勧めします。

まず始めに、Kiwiをgit-submoduleでプロジェクトへ追加します。プロジェクトのルートへ移動した後に、

```
$ git submodule add https://github.com/allending/Kiwi.git Kiwi
```

というコマンドを実行し、Kiwiをローカルに追加します。
次にこのディレクトリをFinderなどで開き "Kiwi.xcodeproj" をプロジェクトへ追加し、ワークスペース化します。

<img src="../static/images/before_add_kiwi.png" />
<img src="../static/images/after_add_kiwi.png" />

追加ができたら "プロジェクト名Tests" のターゲットの "User Header Search Path" へ 

```
${SRCROOT}/Kiwi/Kiwi
```

を追加します。これで、テストのターゲットからKiwiのヘッダーを参照できるようになります。

<img src="../static/images/kiwi_header_search_path.png" />

最後に "Link Binary with Libraries" へ "libKiwi.a" を追加して、Kiwiの準備は終了です。

<img src="../static/images/kiwi_add_library.png" />

## Kiwiを動作させてみる
