# kk-stack-dev-kotonoha-tools

[Kotonoha](https://github.com/kk-stack-dev/kotonoha) が初回セットアップでダウンロードする、
[whisper.cpp](https://github.com/ggml-org/whisper.cpp) の `whisper-cli` と
[llama.cpp](https://github.com/ggml-org/llama.cpp) の `llama-completion` のビルドを置くリポジトリです。
ソースコードの変更はしていません。

| ファイル | 対象 |
|---|---|
| `*-mac-arm64` | Apple シリコンの Mac（Metal を使用） |
| `*-mac-x64` | Intel の Mac |
| `*-windows-x64.exe` | 64 ビットの Windows |

どれも 1 ファイルで動きます（ライブラリや Visual C++ ランタイムは不要です）。

## 新しいビルドを出す

1. `.github/workflows/build.yml` の `WHISPER_REF` と `LLAMA_REF` を更新して main に push し、ビルドが通ることを確かめる
2. `tools-v<番号>` のタグを付けて push する。リリースが作られ、実行ファイル、`SHA256SUMS`、各プロジェクトのライセンス、`catalog.rs` が付く
3. `catalog.rs` の中身を Kotonoha の `crates/kotonoha-core/src/tools.rs` の `TOOL_CATALOG` に貼る

Kotonoha は `TOOL_CATALOG` の SHA-256 と一致したファイルだけを使います。公開済みのリリースのファイルは差し替えないでください。

## ライセンス

whisper.cpp と llama.cpp は MIT ライセンスです。各リリースに `LICENSE-whisper.cpp.txt` と
`LICENSE-llama.cpp.txt` を付けています。このリポジトリのビルド手順も MIT ライセンスです。
