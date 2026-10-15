# AGENTS.md

このリポジトリで作業するコーディングエージェント（Claude Code・Codex・Grok Build・Antigravity CLI）への指示。Claude Code は CLAUDE.md の `@AGENTS.md` で、Codex・Grok Build・Antigravity CLI はこのファイルを直接読む。

利用者への回答・質問・報告は、常に日本語で書く（コードのコメントなどの言語は、このファイルのほかの決まりに従う）。

## このリポジトリは何か

自分用の設定と手順書のリポジトリを、サブモジュールとして 1 か所にまとめたもの。中身は各リポジトリにあり、このリポジトリが持つのは `.gitmodules` と、各サブモジュールのコミットの参照だけ。

| パス | リポジトリ | 追うブランチ | 内容 |
| --- | --- | --- | --- |
| `setup-notes` | `ryo-aoki-pc/setup-notes` | `main` | AlmaLinux 10・Windows 11 の構築・設定の手順書 |
| `setup-notes-verification` | `ryo-aoki-pc/setup-notes-verification`（非公開） | `main` | setup-notes の手順書の検証記録 |
| `bash` | `ryo-aoki-pc/bash` | `main` | 共通の bash の設定 |
| `kvm-container` | `ryo-aoki-pc/kvm-container` | `main` | qemu-kvm と libvirt を systemd のコンテナに収め、軽いホストで VM を動かす |
| `wezterm` | `ryo-aoki-pc/wezterm` | `main` | WezTerm の設定 |
| `md-table-excel` | `ryo-aoki-pc/md-table-excel` | `main` | GLFM の表を Excel で編集する `mdtable` |
| `scene-browser` | `ryo-aoki-pc/scene-browser`（非公開） | `main` | 手元の動画から場面を切り出し、ブラウザでサムネイルから再生する |
| `LazyVimStarter` | `ryo-aoki-pc/LazyVimStarter` | `custom` | LazyVim をもとにした Neovim の設定 |
| `lazygit` | `ryo-aoki-pc/lazygit` | `custom` | lazygit の設定と Excel の差分の道具 |
| `yazi` | `ryo-aoki-pc/yazi` | `custom` | yazi の設定 |

各サブモジュールには、それぞれの AGENTS.md（scene-browser は CLAUDE.md）がある。サブモジュールの中で作業するときは、そちらに従う。

`setup-notes-verification` と `scene-browser` は非公開で、URL は SSH（`git@github.com:`）にしてある。GitHub に SSH の鍵を登録していない環境では、この 2 つだけ取得に失敗する（ほかの 8 つは取り出せる）。

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

このリポジトリでは、Claude Code・Codex・Grok Build・Antigravity CLI が同じ規則で作業する。分担と `main` への取り込みは人が決める。

- 起動された worktree（作業ディレクトリ）の中だけでファイルを変える。ほかの worktree のファイルは変えない
- 今のブランチにだけコミットする。`main` にはコミットも push もしない
- 頼まれた範囲のファイルだけを変える。範囲の外を変えるときは、変える前に理由を書いて確かめる
- 終わったら、テストとリンターを通してから、目的ごとにコミットする。通らなければコミットせずに、結果を報告する
- コミットしたら、今のブランチを push し、`main` への Pull Request を作る（既にあれば足す）。`main` への取り込み（マージ）とブランチの削除は人が行う。今のブランチに `main` を取り込むのは、頼まれたときと、Pull Request が競合したときだけ
- 秘密情報（`.env`・鍵・トークン・パスワード）を読まない・書かない・出力しない
- レビューを頼まれたら、ファイルを変えずに、指摘を「重大度・場所（ファイル:行）・理由・直し方」で挙げる
- ほかの担当の変更は、`git diff main...agent/codex` のように git で読む（ほかの worktree へ移らない）

## 既定の分担

利用者が Claude Code に、分担を指定せずにファイルを変える作業を頼んだときは、Claude Code が取りまとめて次の 4 つで分担する（2026-10-15、利用者の決定）。「共同作業の規則」の「分担は人が決める」で、人が分担を決めていないときの既定にする。利用者がほかのエージェントに直接頼んだときは、この節は使わず「共同作業の規則」に従う。

- Claude Code だけで行うもの: 利用者の質問への答え、ファイルを変えない調べもの、1 つのファイルの数行で振る舞いを変えない直し（誤字・リンク・値の直しなど）
- Claude Code: 計画・実機での確かめ・ほかの 3 つへの依頼・指摘の確かめ・統合・コミットと Pull Request
- Codex: 実装と下書き。Claude Code が用意した別の clone か worktree の中だけで書く。この分担で頼まれたときは、「共同作業の規則」のコミット・push・Pull Request を行わず、変更を残して報告する（Claude Code が確かめ、自分の worktree に移してからコミットする）
- Grok Build: 差分の読み取り専用のレビュー。ファイルを変えない（Windows 11 では、Claude Code は `--always-approve` を付けずに頼む）
- Antigravity CLI（Windows 11 だけ）: 差分の読み取り専用のレビューと、その作業のための調べもの。ファイルを変えない（Claude Code は、入力を写した使い捨てのフォルダーで 1 回だけ実行し、`--dangerously-skip-permissions`・`--mode` を付けない）
- Claude Code から頼まれたエージェントは、上の自分の役割の範囲で作業する
- レビューの指摘は、実物で確かめてから採る。採らなかった指摘は、理由を付けて報告する
- 使えないエージェント（入っていない・ログインが切れた・AlmaLinux 10 の Antigravity CLI）は飛ばし、そのことを報告する
- 頼み方は、setup-notes（https://github.com/ryo-aoki-pc/setup-notes）の `docs/ai-agents.md` の「コーディングエージェントの共同作業: 既定で分担する」と、`docs/reference/coding-agents.md` の「既定の分担」
