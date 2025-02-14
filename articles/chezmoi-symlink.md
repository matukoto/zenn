---
title: "chezmoiのシンボリックリンクモードが便利"
emoji: "🔔"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["dotfiles","chezmoi"]
published: true
published_at: 2024-06-20 18:00
---

## はじめに

[chezmoi](https://github.com/twpayne/chezmoi) は dotfiles 管理ツールです。chezmoi の概要についての記事は Zenn にも多くありますので、[chezmoi検索結果@Zenn](https://zenn.dev/search?q=chezmoi) をご覧ください。
dotfiles を管理している人の多くは、シンボリックリンクを使って管理していると思います。
chezmoi でもシンボリックモードを使うことで、シンボリックリンクとして機能させることができます。

## シンボリックリンクモードを設定する

`~/.config/chezmoi/chezmoi.toml` に ↓ を追加することでシンボリックリンクとして機能します。

```toml
mode = "symlink"
```

※ このモードを設定しない場合、 `chezmoi apply` コマンドを実行しなければファイルの変更が環境に適用されません。`.vimrc` など頻繁に変更や読み込みを行いたいファイルは symlink モードにしておくと便利です。

## 注意点

symlink モードにしても以下のファイルはシンボリックリンクとして機能しない点に注意してください：

- テンプレートファイル (hoge.tmpl)
- プライベートファイル (private_hoge)
- 暗号化されたファイル (encrypted_hoge)

などはシンボリックリンクとして機能しません。
特にテンプレートファイルが盲点となりやすいです。自分はコレにハマって 1 時間溶かしました。

## 参考資料

- [Target types - chezmoi](https://www.chezmoi.io/reference/target-types/#symlink-mode)
