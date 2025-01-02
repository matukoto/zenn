---
title: "chezmoi導入メモ"
emoji: "📀"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["dotfiles", "vim"]
published: false
---
## はじめに

chezmoi をしばらく使ってみて、使い方が安定したので備忘録がてら書きました。

## よく使うコマンド

### chezmoi apply

### chezmoi update

dotfiles リポジトリで変更があった場合、このコマンドで変更を取り込んで環境への適用まで実行してくれます。
仕事とプライベートで異なる PC を使っている場合などに便利です。
内部的には `git pull --autostash --rebase` してから、 `chezmoi apply` してくれているようです。

- [update - chezmoi公式Doc](https://www.chezmoi.io/reference/commands/update/)

### chezmoi cd

chezmoi は `~/.local/share/chezmoi` で dotfiles を管理しており、このコマンドで `~/.local/share/chezmoi` に移動できます。
また exit で元のディレクトリに戻れます。コーディング途中に「ちょっとだけ dotfiles を修正したい」と思ったときにサクッと修正して戻ることができます。

## 便利な設定

### symlink モード

config ファイルに以下のように追加することでシンボリックリンクのように機能します。
※ このモードを設定しない場合、 `chezmoi apply` コマンドを実行しなければファイルの変更が環境に適用されません。`.vimrc` などの読み込み&変更を頻繁にしたい場合は symlink モードにしたほうが楽です。

```config.toml
mode = "symlink"
```

注意点として symlink モードにしても、

- template ファイル (hoge.tmpl)
- private ファイル (private_hoge)
- 暗号化されたファイル (encrypted_hoge)
- 実行可能ファイル (executable_hoge)
などはシンボリックリンクとして機能しません。
特にテンプレートファイルが盲点でした。Windows と Linux で分けたいので `.vimrc` をテンプレート対応させていましたが、symlink モードが適用されず 2 時間くらい溶かしました。。。結局 `.vimrc` はテンプレート対応は諦めました。

## 参考

- [既存の dotfiles を chezmoi で管理する](https://zenn.dev/johnmanjiro13/articles/d14825f4ef3184)
- [chezmoiを使って管理しているdotfileのファイルタイプをNeovimにうまく認識させる | Atusy's blog](https://blog.atusy.net/2023/01/11/neovim-filetype-matching-with-chezmoi/)
- [dotfile manager の chezmoi に移行してみる | /var/log/life](https://blog.yamano.dev/posts/25a8f07bc0e3913807cdf2c3fd48b3cc/)
- [chezmoi を サブマシンにも導入する](https://zenn.dev/yukionodera/articles/setup-second-machine)
- [chezmoi で dotfiles を管理するときに便利な機能についてまとめる](https://zenn.dev/ganariya/articles/useful-features-of-chezmoi)
- [chezmoi で同じ内容のファイルを環境に応じて違うパスに配置する | monolithic kernel](https://blog.mono0x.net/2023/04/16/chezmoi-different-locations-with-the-same-contents/)
