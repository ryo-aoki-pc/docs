# 構築・設定手順書

OS の初期設定とツールの導入、共通のシェル設定、個人用のアプリ設定、KVM の運用手順をまとめて探すためのリポジトリ。各文書の原本は、下のサブモジュールで管理する。

## 最初に読む

| 作業 | 開始点 |
|---|---|
| AlmaLinux 10 を入れた直後 | [AlmaLinux の初期設定](setup-notes/docs/almalinux-setup.md) |
| Windows 11 を入れた直後 | [Windows の初期設定](setup-notes/docs/windows-setup.md) |
| Windows と AlmaLinux をデュアルブートにする | [Windows のインストール](setup-notes/docs/windows-dual-boot.md) |
| ツールを順に導入する | [OS 別の導入順](setup-notes/docs/getting-started.md) |
| 必要な手順書を用途から探す | [setup-notes の文書一覧](setup-notes/docs/README.md) |
| 導入元やツールを比較する | [手順書とツールの比較](setup-notes/docs/catalog.md)・[導入元一覧](setup-notes/docs/tool-catalog.md) |

## リポジトリごとの入口

ツール本体の導入と OS 共通の前提は `setup-notes`、個人設定の導入・操作・保守は各設定リポジトリで扱う。

| リポジトリ | 担当するもの | 手順書の入口 |
|---|---|---|
| [setup-notes](setup-notes/README.md) | OS、ツール本体、リモート接続、VPN、共有・同期、コンテナ | [文書一覧](setup-notes/docs/README.md) |
| [bash](bash/README.md) | 複数ホストで共有する bash 設定と既存設定の移行 | [文書一覧](bash/docs/README.md) |
| [wezterm](wezterm/README.md) | WezTerm の個人設定、キー操作、シェル統合 | [文書一覧](wezterm/docs/README.md) |
| [LazyVimStarter](LazyVimStarter/README.md) | Neovim の日本語入力・検索と Markdown 執筆用設定 | [文書一覧](LazyVimStarter/docs/README.md) |
| [lazygit](lazygit/README.md) | lazygit の個人設定、delta の表示、Excel の差分表示 | [文書一覧](lazygit/docs/README.md) |
| [yazi](yazi/README.md) | yazi の個人設定、連携コマンド、上流への追従 | [文書一覧](yazi/docs/README.md) |
| [kvm-container](kvm-container/README.md) | Podman コンテナによる KVM と VM の作成・運用 | [文書一覧](kvm-container/docs/README.md) |

## 文書の読み分け

- 実行する操作・前提・注意・完了条件は、導入・操作・運用の手順書で確認する
- 背景・採用理由・設定の仕様は、各リポジトリの `docs/reference/` で確認する
- 実施日・対象版・環境・結果・未確認事項は、各リポジトリの `docs/verification/` で確認する

[setup-notes の参考資料一覧](setup-notes/docs/reference/README.md)と[検証記録一覧](setup-notes/docs/verification/README.md)からも探せる。アプリ設定と KVM の資料・記録は、それぞれの文書一覧から開く。

## 手順書を手元で読む

初回はサブモジュールを含めて取得する。

```bash
git clone --recurse-submodules https://github.com/ryo-aoki-pc/docs.git
```

既に取得したリポジトリでは、`docs` のディレクトリで実行する。

```bash
git pull --ff-only
git submodule update --init --recursive
```

この操作は `docs` が記録した各サブモジュールの版を取得する。アプリの設定を有効にする場合は、それぞれの導入手順に従って設定用の場所へ配置する。

文書を追加・移動する場合は [配置と更新のガイド](CONTRIBUTING.md)を参照する。
