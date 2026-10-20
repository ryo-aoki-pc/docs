# AGENTS.md

このリポジトリで作業するコーディングエージェント（Claude Code・Codex・Grok Build・Antigravity CLI）への指示。Claude Code は CLAUDE.md の `@AGENTS.md` で、Codex・Grok Build・Antigravity CLI はこのファイルを直接読む。

利用者への回答・質問・報告は、常に日本語で書く。作業の途中の短い報告、作業の最後のまとめ（完了の報告）、確認の問い、画面の欄に出す要約も含め、英語で書かない。英語の指示・資料・ツールの出力を読んだ後や、会話が長くなった後も同じ（コードのコメントなどの言語は、このファイルのほかの決まりに従う）。

長い作業の最後のまとめは、英語になりやすい。送る前に必ず読み返し、英語の文が 1 つでもあれば、日本語に書き直してから送る。そのまま残してよいのは、コード・コマンド・パス・識別子と、引用したエラーの文だけ。

## このリポジトリは何か

自分用の設定と手順書のリポジトリを、サブモジュールとして 1 か所にまとめたもの。中身は各リポジトリにあり、このリポジトリが持つのは `.gitmodules` と、各サブモジュールのコミットの参照だけ。

| パス | リポジトリ | 追うブランチ | 内容 |
| --- | --- | --- | --- |
| `setup-notes` | `ryo-aoki-pc/setup-notes` | `main` | AlmaLinux 10・Windows 11 の構築・設定の手順書 |
| `setup-notes-verification` | `ryo-aoki-pc/setup-notes-verification`（非公開） | `main` | setup-notes の手順書と、ほかの公開リポジトリ（`repos/<名前>/`）の検証記録 |
| `bash` | `ryo-aoki-pc/bash` | `main` | 共通の bash の設定 |
| `kvm-container` | `ryo-aoki-pc/kvm-container` | `main` | qemu-kvm と libvirt を systemd のコンテナに収め、軽いホストで VM を動かす |
| `wezterm` | `ryo-aoki-pc/wezterm` | `main` | WezTerm の設定 |
| `md-table-excel` | `ryo-aoki-pc/md-table-excel` | `main` | GLFM の表を Excel で編集する `mdtable` |
| `md-format` | `ryo-aoki-pc/md-format` | `main` | GFM と GLFM の Markdown を、描画を変えずに整形する `mdfmt`（Neovim の保存の整形も使う） |
| `md-table-sticky` | `ryo-aoki-pc/md-table-sticky` | `main` | GitLab の Markdown の表で、先頭行・先頭列を固定してスクロールできるようにするブックマークレット |
| `excel-diff` | `ryo-aoki-pc/excel-diff` | `main` | Excel のブックと Word の文書を、セル・段落の単位で比べる道具（Git の差分表示・部分ステージ、2 つのファイルの取り込み） |
| `LazyVimStarter` | `ryo-aoki-pc/LazyVimStarter` | `custom` | LazyVim をもとにした Neovim の設定 |
| `lazygit` | `ryo-aoki-pc/lazygit` | `custom` | lazygit の設定 |
| `yazi` | `ryo-aoki-pc/yazi` | `custom` | yazi の設定 |

各サブモジュールには、それぞれの AGENTS.md がある。サブモジュールの中で作業するときは、そちらに従う。

`setup-notes-verification` は非公開で、URL は SSH（`git@github.com:`）にしてある。GitHub に SSH の鍵を登録していない環境では、これだけ取得に失敗する（ほかの 11 個は取り出せる）。

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

このリポジトリでは、Claude Code・Codex・Grok Build・Antigravity CLI が同じ規則で作業する。分担は人が決める。

- 起動された worktree（作業ディレクトリ）の中だけでファイルを変える。ほかの worktree のファイルは変えない
- 今のブランチにだけコミットする。`main` にはコミットも push もしない
- 頼まれた範囲のファイルだけを変える。範囲の外を変えるときは、変える前に理由を書いて確かめる
- 終わったら、テストとリンターを通してから、目的ごとにコミットする。通らなければコミットせずに、結果を報告する
- コミットしたら、今のブランチを push し、`main` への Pull Request を作る（既にあれば足す）。今のブランチに `main` を取り込むのは、頼まれたときと、Pull Request が競合したときだけ
- `main` への取り込み（マージ）は、エージェントが行ってよい。テストとリンターが通り、Pull Request が競合していなければ、`gh pr merge --squash` で Pull Request を取り込む（手元の `main` にはコミットも push もしない）。利用者が取り込まないよう頼んだときは取り込まず、そのことを報告する。取り込んだブランチには、その後コミットも push もせず、次の作業は新しいブランチで始める
  - リモートのブランチ: 取り込んだ Pull Request のブランチは、GitHub が自動で消す（リポジトリの Automatically delete head branches）。取り込む前に `gh api 'repos/{owner}/{repo}' --jq .delete_branch_on_merge` で確かめ、`false` なら `gh api -X PATCH 'repos/{owner}/{repo}' -F delete_branch_on_merge=true` で有効にする（`{owner}/{repo}` は gh が今のリポジトリで埋めるので、そのまま打つ）。取り込んだ後もリモートに残った自分のブランチは、`git push origin --delete <ブランチ>` で消す
  - 手元のブランチ: 自分が作ったブランチは、Pull Request が取り込まれたこと（`gh pr view <番号> --json state` が `MERGED`）と、どの worktree でも使っていないこと（`git worktree list`）を確かめてから、`git branch -D <ブランチ>` で消す（squash で取り込むので `git branch -d` は通らない）。今いるブランチは、新しいブランチへ移ってから消す。ほかの担当のブランチは消さない
  - Claude Code の worktree とブランチ: セッションの worktree は、Pull Request が閉じるとアプリがセッションをアーカイブして消す（Claude Code の設定の Auto-archive sessions when their pull request closes を有効にしておく。消えることはアプリの設定の説明による。コミットしていない変更がある worktree は残る）。残った `claude/` のブランチは、ほかの担当のブランチを消さない決まりの例外とする。Claude Code は、ほかのセッションが作ったものも、Pull Request が取り込まれたこと（`gh pr list --head <ブランチ> --state merged` に出る）と、どの worktree でも使っていないこと（`git worktree list`）を確かめてから、手元は `git branch -D <ブランチ>`、リモートに残っていれば `git push origin --delete <ブランチ>` で消す
- 秘密情報（`.env`・鍵・トークン・パスワード）を読まない・書かない・出力しない
- レビューを頼まれたら、ファイルを変えずに、指摘を「重大度・場所（ファイル:行）・理由・直し方」で挙げる
- ほかの担当の変更は、`git diff main...agent/codex` のように git で読む（ほかの worktree へ移らない）

## 既定の分担

利用者が Claude Code に、分担を指定せずにファイルを変える作業を頼んだときは、Claude Code が取りまとめて次の 4 つで分担する。「共同作業の規則」の「分担は人が決める」で、人が分担を決めていないときの既定にする。利用者がほかのエージェントに直接頼んだときは、この節は使わず「共同作業の規則」に従う。

- Claude Code だけで行うもの: 利用者の質問への答え、ファイルを変えない調べもの、1 つのファイルの数行で振る舞いを変えない直し（誤字・リンク・値の直しなど）
- Claude Code: 計画・実機での確かめ・ほかの 3 つへの依頼・指摘の確かめ・統合・コミットと Pull Request・取り込み
- Codex: 実装と下書き。Claude Code が用意した別の clone か worktree の中だけで書く。この分担で頼まれたときは、「共同作業の規則」のコミット・push・Pull Request・取り込みを行わず、変更を残して報告する（Claude Code が確かめ、自分の worktree に移してからコミットする）
- Grok Build: 差分の読み取り専用のレビュー。ファイルを変えない（Windows 11 では、Claude Code は `--always-approve` を付けずに頼む）
- Antigravity CLI: 差分の読み取り専用のレビューと、その作業のための調べもの。ファイルを変えない（Claude Code は、入力を写した使い捨てのフォルダーで 1 回だけ実行し、`--dangerously-skip-permissions`・`--mode` を付けない）
- Claude Code から頼まれたエージェントは、上の自分の役割の範囲で作業する
- レビューの指摘は、実物で確かめてから採る。採らなかった指摘は、理由を付けて報告する
- 使えないエージェント（入っていない・ログインが切れた）は飛ばし、そのことを報告する
- 頼み方は、[setup-notes](https://github.com/ryo-aoki-pc/setup-notes) の `docs/ai-agents.md` の「コーディングエージェントの共同作業: Windows 11 で既定で分担する」と、`docs/reference/coding-agents.md` の「既定の分担」
