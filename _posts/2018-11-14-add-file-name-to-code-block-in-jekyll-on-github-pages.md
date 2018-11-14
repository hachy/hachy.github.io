---
layout: post
title: GitHub Pagesでコードブロックにファイル名を表示する
tags: jekyll
image: assets/images/filename2codeblock-1.png
requirements:
- jekyll 3.7.4
- highlight.js v9.13.1
---

GitHub PagesとJekyllを使っているという前提ですが、[Qiita](https://qiita.com)のようにMarkdownのコードブロックでファイル名を表示できるようにします。

![file name to code block](/assets/images/filename2codeblock-1.png 'file name to code block')

## JavaScriptとCSSでファイル名を表示

### JavaScript

以下のように`myscript.js`というファイルを作成してみましょう。ここでは`assets/js`フォルダに置いています。

```js:assets/js/myscript.js
document.addEventListener('DOMContentLoaded', () => {
  const code = document.getElementsByTagName('code');

  Array.from(code).forEach(el => {
    if (el.className) {
      const s = el.className.split(':');
      const highlightLang = s[0];
      const filename = s[1];
      if (filename) {
        el.classList.remove(el.className);
        el.classList.add(highlightLang);
        el.parentElement.setAttribute('data-lang', filename);
        el.parentElement.classList.add('code-block-header');
      }
    }
  });
});
```

簡単に説明すると

- `DOMContentLoaded`をしてHTMLの読み込みが完了したら、`<code>`タグを持つ要素を取得します。結果は`HTMLCollection`として返ります。
- そのHTMLCollectionを`forEach`で回して、それぞれの要素がclass名を持っていればそのclass名を`:`文字で分割します。

- もし`:`文字がない場合は分割できないので`highlightLang`にはそのままclass名が入り、`filename`はundefinedになります。

- `if (filename)`の中では一旦もとのclass名を削除して`:hoge.rb`を除いたclass名を再度追加しています。

- それから`<code>`タグの親要素である`<pre>`タグに`data-lang="hoge.rb"`と`code-block-header`というclassを追加します。class名は任意です。

### CSS

`.code-block-header`の中身は以下のようにします。適当なsassファイルに追加して下さい。

```css:scss
.code-block-header {
  position: relative;
  padding-top: 32px;

  &::before {
    content: attr(data-lang);
    background: darken(#f6f8fa, 8%);
    color: darken(#f6f8fa, 50%);
    display: block;
    font-size: 13px;
    font-weight: 700;
    padding: 1px 10px;
    position: absolute;
    top: 0;
    left: 0;
    z-index: 10;
  }
}
```

`data-`属性とCSSの`::before`を使っています。

### JavaScriptを読み込む

最後に作成した`myscript.js`を読み込みましょう。
HTMLの`<head>`タグの中に追加します。

```html
<script src="{{ "/assets/js/myscript.js" | prepend: site.baseurl }}"></script>
```

これで一応コードブロックにファイル名を表示することができました。

![file name to code block but no syntax](/assets/images/filename2codeblock-2.png 'file name to code block but no syntax'){:width="400px"}

しかしシンタックスハイライトが効いていません。

何故ならGitHub Pagesがデフォルトで使うRougeは、例えば`ruby:hoge.rb`なら`language-ruby:hoge.rb`というclass名を使うことになるからです。

## Rougeの代わりにhighlight.jsを使う

DOMが構築された後にJavaScriptでいろいろな操作をしているので、RougeよりもJavaScriptのライブラリのほうが都合が良いです。ここでは[highlight.js][highlight-js]を使います。

### Rougeを無効にする

詳しくは[こちら]({% post_url 2018-11-12-disable-rouge-with-jekyll-on-github-pages %})を参照してください。

```yaml:_config.yml
markdown: kramdown
kramdown:
  syntax_highlighter_opts:
    disable: true
```

### highlight.jsをダウンロード

[公式サイト][highlight-js]から`highlight.pack.js`と好きなテーマのCSSファイルをダウンロードして読み込みます。ここでは`github-gist`というテーマを選びました。

```html
<link rel="stylesheet" href="{{ "/assets/css/github-gist.css" | prepend: site.baseurl }}">
<script src="{{ "/assets/js/highlight.pack.js" | prepend: site.baseurl }}"></script>
<script>hljs.initHighlightingOnLoad();</script>
```

`<pre>`タグと`<code>`タグのbackground-colorが違う場合は修正しましょう。

以上で完了です。

## 補足

`myscript.js`のコードは改良の余地があります。というのも生成されたHTMLを見てみると`<code>`タグには言語毎のclassの他に`hljs`というclassが追加されています。classが２つ以上あると上記のコードではたぶんうまく動かないと思います。

なぜちゃんと動いているかというと、`DOMContentLoaded`の中ではまだ`hljs`classは追加されていなくて、その挙動に依存しているからです。

[highlight-js]: https://highlightjs.org/
