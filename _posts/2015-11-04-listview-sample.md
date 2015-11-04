---
layout: post
title: "とりあえずListViewを使う"
date: 2015-11-04 10:31:58 +0900
tags: android
---
`layout/activity_main.xml`

{% highlight xml %}
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical" >

    <ListView
        android:id="@+id/list_view"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />
</LinearLayout>
{% endhighlight %}

`MainActivity.java` ではデータをArrayAdapter経由でListViewに表示します。

{% gist 84ff01073eebdd4f7092 %}

`android.R.layout.simple_list_item_1` はAndroid側で用意されているレイアウトファイルで以下のように定義されている。ちなみにAndroidStudioでは`Shift`を２回押すと検索することができる。

`simple_list_item_1.xml`
{% highlight xml %}
<?xml version="1.0" encoding="utf-8"?>
<TextView xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@android:id/text1"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:textAppearance="?android:attr/textAppearanceListItemSmall"
    android:gravity="center_vertical"
    android:paddingStart="?android:attr/listPreferredItemPaddingStart"
    android:paddingEnd="?android:attr/listPreferredItemPaddingEnd"
    android:minHeight="?android:attr/listPreferredItemHeightSmall" />
{% endhighlight %}

結果はこんな感じ。

![listview](/images/2015/11/listview1.png "listview")
