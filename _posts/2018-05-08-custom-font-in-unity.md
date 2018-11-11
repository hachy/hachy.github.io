---
layout: post
title: Unity のカスタムフォントで数字を表示する
tags: Unity
image: assets/images/myfont.gif
requirements:
- Unity version - 2017.3.1f1
---

こういうやつをつくる

![MyFont gif](/assets/images/myfont.gif "MyFont gif")

やり方としては、まず自前のテクスチャでマテリアルを作成する。

次にそのマテリアルを適用したカスタムフォントを作成して、設定をいじっていくという流れ。

## 自前のテクスチャをマテリアルに適用する

今回は例として以下の `128*64` のテクスチャを用意する

![MyFont](/assets/images/myfont.png "MyFont")

<br />

Unity にインポートして

- Texture Type は Default

- Max Size は128 にして Apply

![MyFont texture](/assets/images/myfont-texture.png "MyFont texture"){:width="500px"}


<br />

ProjectでCreate -> Material でマテリアルを作成

- ShaderはUnlit/Transparent にしてみた

- 上記のテクスチャを適用

![MyFont material](/assets/images/myfont-material.png "MyFont material"){:width="500px"}

## カスタムフォントを作成

Assets -> Create -> Custom Font でカスタムフォントを作成

設定はスクリーンショットでどうぞ。

それぞれの値はもちろん元のテクスチャサイズによって変わります。詳細は公式をチェック！

<div class="info">
💡 Tips としてCharactes Rects の size はいきなり 10 にするのではなく、
最初は 1 にして Element 0 の値を埋めてから Size を変更すると
それ以降の Element の値をよしなに設定してくれるので楽です。
</div>

<ul style="display: flex; justify-content: flex-start; list-style-type: none; margin-left: 0px;">
<li>
<img src="/assets/images/myfont-inspector-1.png" alt="MyFont inspector 1" title="MyFont inspector 1">
</li>
<li style="padding-left: 10px;">
<img src="/assets/images/myfont-inspector-2.png" alt="MyFont inspector 2" title="MyFont inspector 2">
</li>
<li style="padding-left: 10px;">
<img src="/assets/images/myfont-inspector-3.png" alt="MyFont inspector 3" title="MyFont inspector 3">
</li>
</ul>

<br />

以上でカスタムフォントが完成しました。

あとは Hierarchy の Create -> Ui -> Text で Text を作成したら **Font** に自身の作成したカスタムフォントを適用して **Text** に0~9 の値を入れて試してみましょう！


###### 参考

- <https://docs.unity3d.com/ja/current/Manual/class-Font.html> (公式)
- <http://scalar.seesaa.net/article/447727959.html>
