---
title: "miseでnpmパッケージを管理する"
emoji: "🔔"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["mise", "npm", "go"]
published: true
---
## 対象読者

対象読者は既に [mise](https://github.com/jdx/mise) を使っている人です。そのため mise の基本的な機能の説明は割愛します。
mise を一言で説明すると、「複数の開発言語やツールのバージョンを一元管理できる cli ツール」です。[^1]

## npm パッケージを管理する

`npm install -g zenn-cli` でグローバルにインストールしていたパッケージを mise を使って管理することができます。
npm製のパッケージを管理する場合、以下のように設定します。

```toml:~/.config/mise/config.toml
[tools]
node = "22.9.0"
# 常に最新バージョンを使用
"npm:zenn-cli" = "latest"
# 特定のバージョンを使用
"npm:vite" = "5.4.8"
```

上記のように記述したあと、

```
mise install
```

でインストールできます。

## おまけ

まだ experimental なのですが、Go 製のツールも管理できます。
実験的機能なので、設定ファイルに以下のように設定する必要があります。

```toml:~/.config/mise/config.toml
[tools]
go = "1.23.1"
# Vim, Neovim の起動時間を計測できる便利なツール
"go:github.com/rhysd/vim-startuptime" = "latest"

[settings]
# experimental な機能を有効にする
experimental = true
```

その他にも以下のようなバックエンドが使えます。

- Cargo
- Pipx
- SMP
- Ubi
- Vfox

詳細な設定方法や他のバックエンドツールのサポート状況については、以下で確認できます。
[Backends | mise-en-place](https://mise.jdx.dev/dev-tools/backends/#backends)

## 参考資料

- [npm Backend | mise-en-place](https://mise.jdx.dev/dev-tools/backends/npm.html)
- [Go Backend | mise-en-place](https://mise.jdx.dev/dev-tools/backends/go.html)

[^1]: もちろん言語のバージョン管理だけでなく、便利な機能がたくさんあります。
