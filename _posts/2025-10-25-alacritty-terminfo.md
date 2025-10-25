---
layout: post
title: alacrittyをアップデートしたら挙動がおかしい
date: 2025-10-25 21:16 +0900
---
## TL;DR

terminfoが存在しないことがすべての原因だった。

## TERMを変更して修正

 alacrittyを0.16.1にアップデートしたら微妙に挙動がおかしい。
 具体的にはdeleteを押下したら右に移動したり、Neovimから抜けたときにまだ描画が残っていたり。

一応`alacritty.toml`の`TERM = "alacritty"`だったものを`xterm-256color`に変更することで解決できた。

## undercurlが描画されない

しかし、今度はNeovimで設定しているundercurlが正しく描画されない事に気づき(そういえばTERMをalacrittyにしていたのはそのためだった)、
ghosttyに移行しようかなとも思った。(試しにghosstyを使ってみたらundercurlは正しく描画され、不安定な挙動もない)

ちなみに以下のコマンドで波線が表示されるのでalacrittyは対応している状態。

```bash
echo -e "\e[4:3mTEST"
```

## terminfoがない！

そんなこんなで試行錯誤してたらterminfoが存在してないことがわかった。

```bash
$ infocmp alacritty
infocmp: couldn't open terminfo file ...
```

どうやらアップデートに際して`brew`でうまくいかなかったので、公式からdmgをダウンロードしたときにterminfoをなくしてしまったようだ。

alacrittyを`git clone`して以下のコマンドで無事解決できた。

```bash
sudo tic -xe alacritty,alacritty-direct extra/alacritty.info
```

#### 参考

<https://github.com/alacritty/alacritty/blob/master/INSTALL.md#terminfo>
