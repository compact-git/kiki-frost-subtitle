# Kiki Frost 字幕(補助exe) v1.0.0

Kiki Frost Web の字幕(動画・配信の **原文** と **訳文**)を、**自分の画面に「枠なし・背景透明・最前面」**で重ねて表示する Windows 用の補助アプリです。
クリックは後ろの窓に通り抜けるので、動画を見ながら・ゲームをしながらでもじゃまになりません。

## 使い方
1. `KikiFrostSubtitle_v1.0.0.exe` をダブルクリックで起動(インストール不要)
   - 初回は Windows の「PC を保護しました」が出ることがあります(署名なしのため)。「詳細情報」→「実行」で起動できます
   - 起動するとタスクトレイ(画面右下)に Kiki Frost のアイコンが出ます
2. Kiki Frost Web の「🪟 字幕」タブ →「🧩 補助exe」で **🔌 接続する** を押す(「補助exeに字幕を送る」は初期値でオン。一度つながれば、次からはアプリを開くだけで自動で接続します)
   - 初回は Chrome が「このデバイス上のアプリへのアクセス」の許可を求めるので「許可」
   - 「🧩 補助exeと接続中(v1.0.0)」と出ればOK
3. 出したい字幕の「出し先」で **🧩 補助exe** にチェック(初期値でオン)

## アプリから起動する
- 補助exeは起動するたびに、自分の保存場所を Windows に「`kikifrost://`」というリンクとして登録します
- 一度ダブルクリックで起動しておけば、Kiki Frost Web の字幕タブの「▶ 補助exeを起動」や、「🚀 アプリを開いたら補助exeも起動する」(アプリを開いて最初にクリックした時)で起動できます
- 初回は Chrome が「Kiki Frost 字幕 を開きますか?」と聞くので、「常に許可」にチェックして「開く」
- exe を別のフォルダに移したり、新しい版に入れ替えた時は、新しい exe を一度ダブルクリックで起動してね(登録し直されます)
- PC の起動時に補助exeも起動したい時は、トレイのアイコン →「**Windows の起動時に起動する**」をオン

## 位置と大きさ
- 初期は**メインのモニター全体**に、アプリの字幕プレビューと同じ配置で表示します(縦横比を保って拡大)
- **位置とサイズを調整する**:トレイのアイコン → 「位置とサイズを調整する」、または **Ctrl+Alt+I**
  - 窓に枠と半透明の背景が出るので、ドラッグで移動・端をつかんでサイズ変更 → 「✔ 調整おわり」
  - 調整した位置と大きさは次回も使います。「画面いっぱいに戻す」で元に戻ります
- **表示するモニター**:トレイ → 「表示するモニター」で切り替え
- **字幕を表示する**:トレイのチェックで一時的に隠せます
- 終了はトレイ → 「終了」
- Yomi Frost 字幕(ポート 50210・Ctrl+Alt+Y)、Kaki Frost 表示(50211・Ctrl+Alt+K)とは別のアプリ・別のポートなので、同時に起動できます

## 安全のための制限
- 受け付けるのは **このPCの中(127.0.0.1)からの接続だけ**です
- さらに、接続してよいページを限定しています(初期値:`https://kiki.rider.tokyo`、`https://*.pages.dev`、`http://localhost:*`、`http://127.0.0.1:*`)。ほかのサイトからは字幕を出せません
- ポート番号(初期値 50212)と許可するページは、トレイ → 「設定ファイルを開く」の `config.json` で変えられます(変えたら補助exeを起動し直してね。ポートを変えたらアプリ側のポートも同じに)

## GitHub で exe を作って公開する
このフォルダの中身を GitHub のリポジトリ(`compact-git/kiki-frost-subtitle`)に置くと、GitHub Actions が Windows 用の exe を自動で作り、Releases に公開します。

1. リポジトリのページで「Add file → Upload files」を開き、**このzipを展開した中身を全部**ドラッグ&ドロップ → 「Commit changes」
   - `.github` フォルダ(中の `workflows/build.yml`)も必ず入れてください。エクスプローラーで見えない時は「表示 → 隠しファイル」をオン
2. 「Actions」タブで `build-exe` が動き始めます(5〜10分)
3. `package.json` の `version` がまだ Releases に無い版なら、ビルドした exe が Releases に「v<版>」として**自動で公開**されます(公開文は `RELEASE_NOTES.md`)
4. Kiki Frost Web の `config.js` の `exeRepo`(初期値 `compact-git/kiki-frost-subtitle`)がこのリポジトリなので、字幕タブの「⬇ 補助exeをダウンロード」が最新リリースの exe を直接ダウンロードします(リポジトリは Public にしてね)
- `subrender.js` は Kiki Frost Web と同じファイルです。アプリ側で字幕の描き方を変えた時は、これも差し替えてビルドし直してください

## ファイル
- `main.js` … 本体(トレイ・透明な窓・WebSocket サーバー)
- `overlay.html` / `overlay.js` / `preload.js` … 字幕を描く窓
- `subrender.js` … 字幕の描き方(Kiki Frost Web と共通)
- `build/icon.*` / `tray*.png` … アイコン
- `.github/workflows/build.yml` … exe を作って公開する GitHub Actions
- `LICENSE` … MIT ライセンス

## アンインストール
1. トレイのアイコン →「Windows の起動時に起動する」をオフにしてから「終了」
2. exe を削除
3. 設定を消す時は `%APPDATA%\Kiki Frost 字幕` フォルダを削除
4. `kikifrost://` の登録を消す時は、レジストリエディターで `HKEY_CURRENT_USER\Software\Classes\kikifrost` を削除(残っていても害はありません)

## Code signing policy
Free code signing provided by [SignPath.io](https://signpath.io/), certificate by [SignPath Foundation](https://signpath.org/).
(Status: application to the SignPath Foundation program is pending. Releases are unsigned until it is approved.)

**Team roles**
- Committers and reviewers: [compact](https://github.com/compact-git)
- Approvers: [compact](https://github.com/compact-git)

**Privacy policy**
This program will not transfer any information to other networked systems unless specifically requested by the user or the person installing or operating it.
It only communicates with Kiki Frost Web on the same PC through a local WebSocket (127.0.0.1).

**Changes to the system**
- On every launch, it registers the `kikifrost://` URL scheme for the current user (`HKCU\Software\Classes\kikifrost`) so that Kiki Frost Web can start it.
- Only when the user turns on "Windows の起動時に起動する" in the tray menu, it registers itself to start at login for the current user. Turning the option off removes it.
- Settings are stored in `%APPDATA%\Kiki Frost 字幕\config.json`.
- See "アンインストール" above for how to remove everything.

## 仕組みとライセンス
このアプリ自体は MIT ライセンスです(`LICENSE` を参照)。Electron(MIT)と ws(MIT)を使っています。

## 更新履歴
- v1.0.0 初版(Kiki Frost Web の原文字幕・訳文字幕を、話者ごとの名前・色つきで表示)
