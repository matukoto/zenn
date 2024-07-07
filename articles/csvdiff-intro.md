---
title: "csvdiff という CSV の差分表示ツールが便利"
emoji: "📀"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["csv", "diff", "cli", "go"]
published: true
published_at: 2024-07-08 18:00
---

## はじめに

各環境(開発環境やステージング環境)のテーブルデータの差分を確認したいときがあると思います。
そんなときに便利な Go 製の CLI ツールが [csvdiff](https://github.com/aswinkarthik/csvdiff) です。
※ この記事は README.md の一部を日本語で解説しただけですので、英語が苦でない人は README を読んだ方が手っ取り早いです。

## インストール

Go 言語がインストールされている環境であれば、以下のコマンドでインストールできます。
もちろん Windows でも使えます。

```sh
go install github.com/aswinkarthik/csvdiff@latest
```

## 使い方

### 差分の表示

引数に指定したファイル同士の差分を表示します。
ただ差分を表示するだけでなく、以下のように追加、更新、削除された行を別々に表示します。

```sh
$ csvdiff aaa.csv bbb.csv
# Additions (1)
+ 2,104,1002,2024-07-02,1
# Modifications (3)
- 1,101,1001,2024-07-01,2
+ 1,101,1001,2024-07-01,3
# Deletions (1)
- 2,103,1002,2024-07-02,1
```

### 便利なオプション

#### 主キー(プライマリキー)の指定

主キーを指定することで、指定したカラムを主キーとして比較することができます。
一般的な diff ツールであれば、ソートしなければまともに比較できないですが、csvdiff は主キーを指定することでソートしなくても比較できます。

```sh
# 1列目、2列目を主キーに指定
csvdiff aaa.csv bbb.csv --primary-key 0,1
```

#### 特定のカラムの差分だけチェック

特定のカラムのみを比較できます。

```sh
# 3列目の差分だけ表示
csvdiff aaa.csv bbb.csv --columns 2
```

#### 特定のカラムの差分を無視

特定のカラムの差分を無視することができます。
テーブルのデータには更新日などのカラムが含まれていることが多いですが、そういった場合に便利です。

```sh
# 4列目の差分を無視
csvdiff aaa.csv bbb.csv --ignore-columns 3
```

#### 区切り文字の指定

区切り文字の指定もできるので、TSV などの比較も可能です。

```sh
# タブ区切りのファイルを比較
csvdiff aaa.tsv bbb.tsv --separator "\t"
```

#### 出力フォーマット

出力フォーマットを指定することができます。以下の例では JSON 形式で出力されます。

```sh
# json 形式で出力
csvdiff aaa.csv bbb.csv --format json
```

## まとめ

この記事で解説していないオプションもあるので、気になる人は [csvdiff/README](https://github.com/aswinkarthik/csvdiff/blob/master/README.md) を読んでみてください。
