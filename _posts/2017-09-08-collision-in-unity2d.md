---
layout: post
title: Unity2dの衝突判定
tags: Unity2d
image: assets/images/collision2d.png
requirements:
- Unity version - 2017.1.1
- JavaScriptを使用
---

OnCollisionEnter2D を使う場合と OnTriggerEnter2D を使う場合の２つをみていきます。

例として Sphere(球) が重力で落ちてきて Cube(立方体) に当たったときの衝突判定をやってみます。

以下のように Sphere と Cube を用意します。

![collision2d](/assets/images/collision2d.png "collision2d"){:width="100px"}


## OnCollisionEnter2D を使う場合

Sphere が落ちてきて Cube にぶつかったとき衝突判定されます。Sphere は Cubeの上で止まります。

Component を追加していきましょう。

- Sphere はデフォルトでついてるSphere Colliderを削除して **Box Collider 2D** と **Rigidbody 2D** を追加

- Cube はデフォルトでついてるBox Colliderを削除して **Box Collider 2D** を追加

どちらも Inspector の Add Component ボタン → Phisics 2d から選択できます。

さらに Sphere に Add Component ボタン → New Script で新しいスクリプトファイルを作成します。名前は適当につけてください。今回は言語はJavaScriptを使用します。

作成したファイルに以下を記述し再生すれば衝突判定ができているのがわかると思います。
（Console に hit!! と出力される）

``` javascript
function OnCollisionEnter2D(coll: Collision2D) {
  Debug.Log("hit!!");
}
```

## OnTriggerEnter2D を使う場合

OnTriggerEnter2D の使い所は、衝突判定はしつつも物体同士はすり抜けたいときです。

Component の追加は上記の OnCollisionEnter2D と同じですが、物体の少なくともどちらか一方の Box Collider 2D の**Is Trigger にチェックを入れ**てください。

![Is Trigger](/assets/images/is_trigger.png "is_trigger"){:width="300px"}


そして JavaScript は以下のように書き換えます。

ちなみに引数の型が微妙に違うので注意してください。

``` javascript
function OnTriggerEnter2D(other: Collider2D) {
  Debug.Log("hit!!");
}
```

再生すると Sphere は Cube を通り抜けていきますが、衝突判定はされているのがわかると思います。
