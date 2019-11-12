---
layout: post
title: リンクの中にリンク(Nested links)
tags: html
date: 2019-11-12 10:24 +0900
---
リンク(aタグ)の中にリンク(aタグ)を置きたいときがあると思う。

こんな感じで↓

{% include embeded/nested_links.html %}

シンプルに以下のようにすればできそうだがうまくいかないし、そもそも仕様でダメらしい。

```html:html
<a href="#outer">
 Outer link
  <a href="#inner">
    Inner link
  </a>
</a>
```

## やり方

Outer linkはposition: absolute;でリンクの範囲を拡げる

Inner linkはposition: relative;を指定した要素の中に入れる

```css:css
.nested-links {
  position: relative;
  pointer-events: all;
}
.stretched-link::after {
  position: absolute;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  z-index: 1;
  content: '';
}
.inner-links {
  position: relative;
  z-index: 2;
  pointer-events: none;
}
.inner-links a {
  pointer-events: all;
}
```

```html:html
<div class="nested-links">
  <a href="#outer" class="stretched-link">Outer link</a>
  <div class="inner-links">
    <a href="#inner">Inner link</a>
  </div>
</div>
```

###### 参考

- <https://stackoverflow.com/questions/9882916/are-you-allowed-to-nest-a-link-inside-of-a-link>
- <https://getbootstrap.com/docs/4.3/utilities/stretched-link/>
