# AGENTS.md

このリポジトリで作業するコーディングエージェント（Claude Code・Codex・Grok Build）への指示。Claude Code は CLAUDE.md の `@AGENTS.md` で、Codex と Grok Build はこのファイルを直接読む。

利用者への回答・質問・報告は、常に日本語で書く（コードのコメントなどの言語は、このファイルのほかの決まりに従う）。

## このリポジトリは何か

自分用の設定と手順書のリポジトリを、サブモジュールとして 1 か所にまとめたもの。中身は各リポジトリにあり、このリポジトリが持つのは `.gitmodules` と、各サブモジュールのコミットの参照だけ。

| パス | リポジトリ | 追うブランチ | 内容 |
| --- | --- | --- | --- |
| `setup-notes` | `ryo-aoki-pc/setup-notes` | `main` | AlmaLinux 10・Windows 11 の構築・設定の手順書と検証記録 |
| `bash` | `ryo-aoki-pc/bash` | `main` | 共通の bash の設定 |
| `kvm-container` | `ryo-aoki-pc/kvm-container` | `main` | qemu-kvm と libvirt を systemd のコンテナに収め、軽いホストで VM を動かす |
| `wezterm` | `ryo-aoki-pc/wezterm` | `main` | WezTerm の設定 |
| `md-table-excel` | `ryo-aoki-pc/md-table-excel` | `main` | GLFM の表を Excel で編集する `mdtable` |
| `LazyVimStarter` | `ryo-aoki-pc/LazyVimStarter` | `custom` | LazyVim をもとにした Neovim の設定 |
| `lazygit` | `ryo-aoki-pc/lazygit` | `custom` | lazygit の設定と Excel の差分の道具 |
| `yazi` | `ryo-aoki-pc/yazi` | `custom` | yazi の設定 |

各サブモジュールには、それぞれの AGENTS.md がある。サブモジュールの中で作業するときは、そちらに従う。

## 変更のしかた

- サブモジュールの中のファイルは、このリポジトリでは変えない。変えるときは、そのリポジトリを clone し、追うブランチへの Pull Request を作る
- このリポジトリでは、サブモジュールの Pull Request が取り込まれた後に、参照を上げるだけにする
- コミットメッセージは日本語で、「〜を含むサブモジュールへ更新する」の形にする（`git log` の過去のコミット）

```sh
git submodule update --init                 # 参照しているコミットを取り出す
git submodule update --remote <パス>        # .gitmodules のブランチの最新に上げる
git submodule status                        # 参照しているコミットを見る
git add <パス> && git commit                # 上げた参照をコミットする
```

- `git submodule update --remote` は、各リポジトリの最新を取り込む。取り込んでよいもの（マージ済みのもの）だけになっていることを、`git -C <パス> log --oneline` で確かめてからコミットする

## 共同作業の規則

このリポジトリでは、Claude Code・Codex・Grok Build が同じ規則で作業する。分担と `main` への取り込みは人が決める。

- 起動された worktree（作業ディレクトリ）の中だけでファイルを変える。ほかの worktree のファイルは変えない
- 今のブランチにだけコミットする。`main` にはコミットも push もしない
- 頼まれた範囲のファイルだけを変える。範囲の外を変えるときは、変える前に理由を書いて確かめる
- 終わったら、テストとリンターを通してから、目的ごとにコミットする。通らなければコミットせずに、結果を報告する
- コミットしたら、今のブランチを push し、`main` への Pull Request を作る（既にあれば足す）。`main` への取り込み（マージ）とブランチの削除は人が行う。今のブランチに `main` を取り込むのは、頼まれたときと、Pull Request が競合したときだけ
- 秘密情報（`.env`・鍵・トークン・パスワード）を読まない・書かない・出力しない
- レビューを頼まれたら、ファイルを変えずに、指摘を「重大度・場所（ファイル:行）・理由・直し方」で挙げる
- ほかの担当の変更は、`git diff main...agent/codex` のように git で読む（ほかの worktree へ移らない）
