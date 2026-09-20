# 開発ルール

## プロジェクト概要
FreeBSD のホスト環境と Jail 環境に対して、`freebsd-update` および `pkg` コマンドを
まとめて適用できるシェルスクリプト `freebsd-update-helper`。

## 言語・環境
- **シェル**: `/bin/sh` (FreeBSD 標準の POSIX sh)
- **対象OS**: FreeBSD (Jail を持つホスト)
- **UI**: `dialog` コマンド (ncurses ベース) を使ったメニュー UI

## ファイル構成
```
freebsd-update-helper   # メインスクリプト
README.md               # ドキュメント
.agent/RULES.md         # 本ファイル（開発ルール）
```

## コーディング規約
- コード内コメントは **日本語** で記述すること
- POSIX sh 互換を保つこと (bashism 禁止)
- `set -e` を使用してエラー時に即時終了
- 関数名は `snake_case`
- 定数・環境変数は `UPPER_SNAKE_CASE`
- インデント: タブ文字

## コミット規約
- コミットメッセージは日本語で記述
- プレフィックス記法: `feat:`, `fix:`, `docs:`, `style:`, `refactor:`, `test:`, `chore:`
- 例: `feat: freebsd-update-helper の初期実装`

## セキュリティ
- root 権限チェックを必ず行うこと
- パスワードを平文で保存しないこと
- Jail 名のバリデーションを行うこと (ディレクトリトラバーサル対策)

## GitHub
- リポジトリ名: `FreeBSD-update-helper`
- self-update コマンドは GitHub の raw URL から取得する
