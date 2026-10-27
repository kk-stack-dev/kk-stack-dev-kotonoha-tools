# Kotonoha

話した言葉を、その場で入力欄に文字として入れる音声入力アプリです。音声も文字も、すべて自分のコンピューターの中で
処理され、外部には送られません。このリポジトリは、Kotonoha の配布用です。

## インストール

### Mac（macOS 13 以降）

1. 「ターミナル」を開きます（Finder の「アプリケーション」>「ユーティリティ」の中にあります）
2. 次の 1 行を貼り付けて、Return キーを押します

```sh
curl -fsSL https://raw.githubusercontent.com/kk-stack-dev/kk-stack-dev-kotonoha-tools/main/install.sh | sh
```

### Windows（10 / 11、64 ビット）

1. スタートメニューで「PowerShell」と入力して開きます
2. 次の 1 行を貼り付けて、Enter キーを押します

```powershell
irm https://raw.githubusercontent.com/kk-stack-dev/kk-stack-dev-kotonoha-tools/main/install.ps1 | iex
```

インストールが終わると Kotonoha が起動し、セットアップ画面が開きます。案内に沿って進めると、文字起こしに
使うファイル（約 1 GB〜1.7 GB）がダウンロードされます。

新しい版が出たときも、同じ 1 行で更新できます。

### なぜブラウザでダウンロードしないのか

Kotonoha は Apple や Microsoft の有料の署名を付けていません。ブラウザでダウンロードしたアプリは、
「開発元を確認できません」などの警告で止められます。上の 1 行は、ダウンロードしたファイルが公開しているものと
同じか（SHA-256）を確かめてからインストールします。中身は [install.sh](install.sh) と
[install.ps1](install.ps1) で確認できます。

Mac では、更新するとマイクとアクセシビリティの許可が外れることがあります。そのときはセットアップ画面の案内に
沿ってもう一度許可してください。

## アンインストール

- Mac：「アプリケーション」から Kotonoha.app をゴミ箱に入れます。設定とモデルも消すときは、
  `~/Library/Application Support/dev.Kotonoha.Kotonoha` と `~/Library/Logs/Kotonoha` も消します
- Windows：「設定」>「アプリ」から Kotonoha をアンインストールします。設定とモデルも消すときは、
  `%LOCALAPPDATA%\Kotonoha` と `%APPDATA%\Kotonoha` も消します

## 中身

| リリース | 内容 |
|---|---|
| `v*`（最新） | アプリ本体。Mac 用 `Kotonoha-mac.zip`（Apple シリコンと Intel 共通）と Windows 用 `Kotonoha-windows-x64-setup.exe` |
| `tools-v*` | セットアップでダウンロードする whisper.cpp（`whisper-cli`）と llama.cpp（`llama-completion`） |

アプリ本体は Kotonoha のリポジトリの Release ワークフローがビルドして、ここに置きます。

### ツールのビルドを新しくする

whisper.cpp と llama.cpp はソースコードを変更せずにビルドしています。どれも 1 ファイルで動きます。

1. `.github/workflows/build.yml` の `WHISPER_REF` と `LLAMA_REF` を更新して main に push し、ビルドが通ることを確かめる
2. Actions の Build を「Run workflow」から実行し、`release` に `tools-v<番号>` を入れる（同じ名前のタグを push しても同じ）。
   リリースが作られ、実行ファイル、`SHA256SUMS`、各プロジェクトのライセンス、`catalog.rs` が付く
3. `catalog.rs` の中身を Kotonoha の `crates/kotonoha-core/src/tools.rs` の `TOOL_CATALOG` に貼る

Kotonoha は `TOOL_CATALOG` の SHA-256 と一致したファイルだけを使います。公開済みのリリースのファイルは差し替えないでください。

## ライセンス

Kotonoha と、このリポジトリのビルド手順・スクリプトは MIT ライセンスです。whisper.cpp と llama.cpp も MIT ライセンスで、
各ツールのリリースに `LICENSE-whisper.cpp.txt` と `LICENSE-llama.cpp.txt` を付けています。
