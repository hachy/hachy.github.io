---
layout: post
title: Rails アップグレードの手順
tags: rails
---

Railsのバージョンを上げるときの手順メモ

- テストが通るかどうか確認
- アップグレード用のブランチをきる
- railsのversionをあげるまえにbundle updateして他のgemをアップデートする
- エラーなどなければGemfileのrailsのversionをあげてbundle update
- rails app:update でdiffを確認しながら上書きなどして調整する
- production.rbがかわったらstaging.rbにもコピーする
- テストが通るかどうか確認
- bundle clean
- bin/rails s でエラーがないか確認

などなど
