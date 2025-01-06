---
title: "aquaでdocker-composeを管理するときにハマった"
emoji: "🔔"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["aqua", "dockercompose"]
published: true
---
## はじめに

aquaでdocker compose を管理するときにつまったのでメモ。
aquaとは CLI のバージョン管理ツールです。詳しくは [aqua CLI Version Manager 入門(zenn_book)](https://zenn.dev/shunsuke_suzuki/books/aqua-handbook/viewer/index) を読んでください。
自分の用途としては aqua と Renovate を組み合わせて CLI のバージョンを自動で更新してもらっています。

## 内容

[Release v3.13.0 · aquaproj/aqua-registry](https://github.com/aquaproj/aqua-registry/releases/tag/v3.13.0) を読んでもらえば解決しますが流れをメモしておきます。

### 何が起こったか

aqua で docker compose をインストールしたのに下記のように compose コマンドが使えない。

```sh
$ docker compose
docker: 'compose' is not a docker command.
See 'docker --help'
```

### 原因

docker compose などの plugin は `$HOME/.docker/cli-plugins` に配置され、読み込まれる仕様になっている。
一方 aqua でインストールした場合は `$HOME/.local/share/aquaproj-aqua/bin` に docker-cli-plugin-docker-compose が配置される。
`$HOME/.docker/cli-plugins` に docker-compose が配置されていないため、compose なんてコマンドはないよと言われてしまう。

### 解決策

#### 1. aqua で管理されている docker-cli-plugin-docker-compose を呼び出すためのラッパースクリプトを `$HOME/.docker/cli-plugins` に配置する

```sh:~/.docker/cli-plugins/docker-compose
#!/bin/sh
exec aqua exec -- docker-cli-plugin-docker-compose "$@"
```

##### コマンドの説明

- aqua exec は、aqua を使って CLI ツールを実行できる。
- -- 以降に記述された内容はすべて引数として解釈される。
ここでは、aqua に対して「これ以降の引数（＝docker-cli-plugin-docker-compose "$@"）」をそのまま渡すよう指示している。
- $@ はシェルスクリプト内で使われる特殊変数であり、「スクリプトに渡されたすべての引数」を展開する。

#### 2. 実行権限を付与する

```sh
chmod u+x $HOME/.docker/cli-plugins/docker-compose
```

これで docker compose が使えるようになります。
Release Note の内容を記事にしただけですが、参考になれば幸いです。

## 参考

- [docker/compose: Define and run multi-container applications with Docker](https://github.com/docker/compose?tab=readme-ov-file)
- [Docker CLI Plugin とは](https://zenn.dev/miroha/articles/docker-cli-plugin)
- [Issue #2875 · aquaproj/aqua](https://github.com/aquaproj/aqua/issues/2875)
