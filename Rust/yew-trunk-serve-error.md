# RustでWebAssemblyをビルド(trunk)した際のエラーと解消方法

## 開発環境

| カテゴリー | ソフトウェア名 | バージョン |
|-----------|-----------|-----------|
| OS | Windows 11 Pro | 22H2 |
| シェル | [PowerShell](https://github.com/PowerShell/PowerShell "PowerShell - PowerShell for every system!") | 7.3.8 |
| 言語 | [Rust](https://github.com/rust-lang/rust "Rust - Empowering everyone to build reliable and efficient software.") | 1.73.0 |
| フレームワーク | [Yew](https://github.com/yewstack/yew "Yew - Rust / Wasm framework for creating reliable and efficient web applications") | 0.21.0 |
| ビルドツール | [Trunk](https://github.com/thedodd/trunk "Trunk - Build, bundle & ship your Rust WASM application to the web.") | 0.17.5 |


## エラー内容

```
❯ trunk serve
2023-10-25T14:35:56.188307Z  INFO 📦 starting build
2023-10-25T14:35:56.189231Z  INFO spawning asset pipelines
2023-10-25T14:35:56.377577Z  INFO building yew-app
error: アクセスが拒否されました。 (os error 5)
2023-10-25T14:35:56.551626Z ERROR ❌ error
error from HTML pipeline

Caused by:
    0: error from asset pipeline
    1: error during cargo build execution
    2: cargo call returned a bad status
2023-10-25T14:35:56.552652Z  INFO 📡 serving static assets at -> /
2023-10-25T14:35:56.553893Z  INFO 📡 server listening at http://127.0.0.1:8080
```


## エラー原因

プロジェクトディレクトリ配下でビルドされたwasm実行ファイルがローカルネットワークにアクセスする場合、Windows OSに標準搭載されているWindows セキュリティアプリケーション(Windows Defender)によってデフォルトでアクセス拒否されてしまうため。


## エラー解消

プロジェクトディレクトリが存在するルートフォルダ(例えば、"project"や"workspace"、"local"など)を除外の設定に追加する。


### 除外するフォルダの設定方法

Windows セキュリティ アプリケーションのバージョン: 1000.25873.0.9001の場合になります。

1.  通知アイコンから「Windows セキュリティ」のアイコンを見つけ、クリック

  ![screenshot_2023-10-26_013128](https://github.com/itjoin1507/memo/assets/149089355/4c4faeb3-e550-4052-bb62-ce58aa6f1c80)


2.  「ウイルスと脅威の防止」から「『ウイルスと脅威の防止の設定』設定の管理」をクリック

  ![screenshot_2023-10-26_013445](https://github.com/itjoin1507/memo/assets/149089355/5c560da4-1778-4db2-bf5e-d259081a34f2)


3. 一番下にスクロールして、「『除外』除外の追加または削除」をクリック

  ![screenshot_2023-10-26_013726](https://github.com/itjoin1507/memo/assets/149089355/b0554e84-a7e5-43b4-88f3-e3b4d57bbd4d)


4. UA制御のポップアップで「はい」を押す


5. 「除外の追加」から除外したいフォルダを選択する

  ![screenshot_2023-10-26_014219](https://github.com/itjoin1507/memo/assets/149089355/b9f36edc-49d7-47c3-a94d-a3b1c6503533)