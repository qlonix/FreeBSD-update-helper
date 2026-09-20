# FreeBSD Update Helper

FreeBSD のホスト環境と Jail 環境に対して、`freebsd-update` および `pkg` コマンドを
まとめて適用できるシェルスクリプトです。

`dialog` ベースのインタラクティブなメニュー UI と、スクリプト化しやすい CLI 引数の両方をサポートしています。

---

## 機能

- **freebsd-update fetch / install** をホスト + 全 Jail または指定 Jail に一括適用
- **pkg update / upgrade** をホスト + 全 Jail または指定 Jail に一括適用
- **メニュー UI**: 引数なしで起動すると `dialog` ベースの ncurses メニューが立ち上がる
- **self-update**: GitHub から最新版に自己更新

---

## 必要条件

| ツール | 用途 |
|---|---|
| `freebsd-update` | FreeBSD ベースシステム更新 |
| `pkg` | パッケージ管理 |
| `jls` / `jexec` | Jail 操作 |
| `dialog` | メニュー UI (オプション) |

`dialog` のインストール:

```sh
pkg install -y dialog
```

---

## インストール

```sh
# リポジトリをクローン
git clone https://github.com/qlonix/FreeBSD-update-helper.git

# スクリプトをシステムパスに配置
install -m 755 FreeBSD-update-helper/freebsd-update-helper /usr/local/sbin/freebsd-update-helper
```

---

## 使い方

### メニューモード (引数なし)

```sh
freebsd-update-helper
```

`dialog` ベースのインタラクティブメニューが起動します。  
操作の種類・対象 (ホスト / 個別 Jail / 全 Jail) をメニューから選択できます。

### CLI モード

#### freebsd-update fetch

```sh
# 全環境 (ホスト + 全 Jail)
freebsd-update-helper fetch all

# 指定した Jail のみ
freebsd-update-helper fetch web db mail
```

#### freebsd-update install

```sh
# 全環境
freebsd-update-helper install all

# 指定した Jail のみ
freebsd-update-helper install web db
```

#### pkg update

```sh
# 全環境
freebsd-update-helper pkg update all

# 指定した Jail のみ
freebsd-update-helper pkg update web
```

#### pkg upgrade

```sh
# 全環境
freebsd-update-helper pkg upgrade all

# 指定した Jail のみ
freebsd-update-helper pkg upgrade web db
```

#### 全操作を一括実行 (fetch → install → pkg update → pkg upgrade)

```sh
# 全環境
freebsd-update-helper all

# 指定した Jail のみ
freebsd-update-helper all web db mail
```

#### self-update (スクリプト自体を GitHub から更新)

```sh
freebsd-update-helper self-update
```

実行前のスクリプトは `.bak` ファイルとしてバックアップされます。

#### バージョン確認

```sh
freebsd-update-helper version
```

---

## 動作の詳細

- **root 権限チェック**: スクリプトは必ず root として実行する必要があります。
- **Jail の検出**: `jls -q name` で実行中の Jail を自動検出します。起動していない Jail はスキップされます。
- **freebsd-update (Jail 向け)**: `freebsd-update -b <jail_path>` を使用します。
- **pkg (Jail 向け)**: `jexec <jail_name> pkg ...` を使用します。
- **エラー処理**: 個別の操作が失敗しても他の操作は継続します (最終的な終了コードはいずれかが失敗すれば非ゼロ)。

---

## ライセンス

BSD 2-Clause License

---

## 貢献

Issue や Pull Request は歓迎します。
