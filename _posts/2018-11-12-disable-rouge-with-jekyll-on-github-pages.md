---
layout: post
title: GitHub PagesのJekyllでRougeを無効にする
tags: jekyll
requirements:
- jekyll 3.7.4
- rouge 2.2.1
---

[GitHub Pages](https://pages.github.com)で[Jekyll](https://jekyllrb.com)を使っている場合、シンタックスハイライトはデフォルトで[Rouge](http://rouge.jneen.net)が使われます。

しかし、何らかの理由でそれを無効にしたいこともあるかもしれません。

たとえばRougeのかわりに[highlight.js](https://highlightjs.org)や[Prism.js](https://prismjs.com)を使いたいとか。

## Rougeを無効にする

`_config.yml`に`highlighter: none`と追加するとローカル環境では動作しますが、GitHub Pagesではうまくいかないようです。

代わりに以下を追加します。

```yaml:_config.yml
markdown: kramdown
kramdown:
  syntax_highlighter_opts:
    disable: true
```

これでGitHub PagesでもRougeが無効になります。

###### 参考

- <https://github.com/github/pages-gem/issues/198>
