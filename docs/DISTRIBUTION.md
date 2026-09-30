# PHPLoader 1.0.1 配布ガイド

公開元: [USAPICO](https://usapico.com/)。この文書は配布物の内容と公開作業の確認用です。ホームページへのアップロードは自動で行いません。

## 対象環境と起動

- Windows 11 / x64 で動作確認します。Windows 10、Windows ARM、macOS、Linux は今回の動作確認対象外です。
- ZIP をすべて展開して `PHPLoader.exe` を起動します。実行ファイルと、同じフォルダーの DLL、`resources`、`locales`、文書類をまとめて配布してください。
- PHP 本体は同梱していません。PHP 実行には、利用者が用意した `php.exe` を設定します。PHP がなくてもコード編集、Markdown プレビュー、HTML 用の静的サーバーを利用できます。
- 本版はコード署名を行っていません。Windows の設定やファイルの評判によって、Microsoft Defender SmartScreen や Smart App Control などが警告・ブロックする場合があります。署名済み、認証済み、安全性審査済みとは表示しないでください。
- 利用者には公式配布元と SHA-256 の確認を案内します。保護機能の無効化を案内しないでください。組織の管理するパソコンでは管理者のルールに従います。

## 同梱する文書

| ファイル | 内容 |
| --- | --- |
| `README.md` | 利用方法、PHP の設定、制限事項 |
| `LICENSE.txt` | PHPLoader 本体の利用条件 |
| `docs/PRIVACY.md` | 保存するデータ、通信、データ削除方法 |
| `docs/EXTENSIONS.md` | JSON スニペット拡張の仕様 |
| `docs/THIRD_PARTY_NOTICES.txt` | JavaScript 依存ライブラリと Electron のライセンス全文 |
| `docs/dependency-inventory.json` | ロック済み依存関係のバージョン・配布元・ハッシュ |
| `LICENSE.electron.txt` / `LICENSES.chromium.html` | Electron と同梱ランタイムの通知・ライセンス全文 |

本体の改変・再配布の条件は、第三者ソフトウェアが各ライセンスで認める権利を制限しません。DOMPurify は選択可能なライセンスのうち Apache-2.0 を選択しています。Lucide の通知には Feather 由来のアイコンについての MIT 表示も含めます。

## ランタイムの対応ソース

PHPLoader は Electron に含まれる FFmpeg を使用します。FFmpeg の該当コードには LGPL v2.1 以降が適用されます。**ライセンス文書を添付するだけで、対応ソースの配布に関する条件まで完了するわけではありません。** [FFmpeg の公式配布ガイド](https://ffmpeg.org/legal.html) は、配布するバイナリーに対応するソースとビルド情報を、バイナリーと同じ配布サイトで提供する方法を示しています。

この版のソースの対応関係は次のとおりです。

- Electron: `v44.5.0`
- Electron の `DEPS` が指定する Chromium: `152.0.7977.130`
- Chromium の `DEPS` が指定する FFmpeg: `2b68d2babae73714846961fb0ee47e3b3d2e39a9`
- Electron の標準設定: `ffmpeg_branding = "Chrome"`、`proprietary_codecs = true`
- 上記 FFmpeg の `chromium/config/Chrome/win/x64/config.h`: `CONFIG_GPL = 0`、`CONFIG_NONFREE = 0`、`FFMPEG_LICENSE = "LGPL version 2.1 or later"`

公開時は、対応ソースの配布用アーカイブも同じサイトに置き、アプリのダウンロードページから案内してください。ソースアーカイブに含めた版・変更パッチ・ビルド設定は、その中の `README-SOURCES.txt` と `source-manifest.json` で確認します。アプリ本体を使うだけの利用者は、ソースアーカイブをダウンロードする必要はありません。

Chromium の通知はプラットフォームをまたぐ多数のコンポーネントを含み、FFmpeg 以外にも LGPL / MPL の表示があります。`docs/dependency-inventory.json` は Chromium 内部まで分解した完全な SBOM ではありません。Electron / Chromium の版を変更するときは、対応ソースとライセンス条件も再確認してください。

## 公開前に確認すること

1. 配布用 ZIP、対応ソース、SHA-256 の一覧を同じ版でそろえる。開発用フォルダー全体や `%APPDATA%\PHP Loader` をアップロードしない。
2. アプリ ZIP に、下書き、秘密鍵、認証情報、テストデータ、開発用ソースマップが混ざっていないことを確認する。
3. 新規ユーザーデータで起動し、PHP のある環境とない環境、保存と復元、HTMX、Markdown、拡張機能を確認する。最終 ZIP から展開した内容を対象にする。
4. 本体の利用条件、公開元、対応 OS、PHP 別途必要、未署名であること、第三者通知と対応ソースへのリンクをダウンロードページに載せる。
5. `README.md` に記載した不具合の連絡先または公式サイトの問い合わせ導線が有効であることを確認する。

SHA-256 はダウンロードしたファイルとの一致確認に使います。デジタル署名や発行元の証明の代わりではありません。ハッシュ値は、公開する実際の ZIP が完成した後で計算します。

## 更新する場合

PHP と Electron / Chromium は実行環境です。依存関係のバージョンを固定し、更新時は脆弱性情報、ライセンス通知、対応ソース、パッケージ実機テストを更新します。ライセンス通知は `node scripts/generate-notices.mjs` で再生成し、`--check` で同梱内容と元データの一致を確認できます。脆弱性検査で検出がないことは、未知の問題がないことを保証しません。
