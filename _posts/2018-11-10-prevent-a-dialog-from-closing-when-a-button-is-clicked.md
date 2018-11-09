---
layout: post
title: ボタンをタップした時ダイアログが閉じないようにする
tags: android
image: images/dialogfragment.png
---

AndroidのダイアログはEditTextの値が有効か無効かに関わらずボタンを押した時に閉じてしまいます。

しかし、値が無効の場合はエラーメッセージを表示するなどして、ダイアログを閉じないようにしたい時もあるでしょう。

この記事では、ボタンが押された時に勝手にダイアログが閉じないようにするやり方を紹介します。

![Dialog](/images/dialogfragment.png "Dialog")

## とりあえずダイアログを表示する

まずはDialogFragmentを使って簡単なダイアログを表示するところまでやってみます。

ダイアログのレイアウトファイルを用意します。

```xml:res/layout/my_dialog.xml
<android.support.constraint.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <android.support.design.widget.TextInputLayout
        android:id="@+id/text_input_name"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:layout_marginTop="16dp"
        android:layout_marginStart="20dp"
        android:layout_marginEnd="20dp"
        android:paddingTop="8dp"
        app:hintAnimationEnabled="true"
        app:hintEnabled="true"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent">

        <android.support.v7.widget.AppCompatEditText
            android:id="@+id/edit_name"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:hint="@string/name" />
    </android.support.design.widget.TextInputLayout>
</android.support.constraint.ConstraintLayout>
```

support versionのDialogFragmentを使います。


```kotlin:MyDialogFragment.kt
import android.support.v4.app.DialogFragment
// ...

class MyDialogFragment : DialogFragment() {

    override fun onCreateDialog(savedInstanceState: Bundle?): Dialog {
        super.onCreateDialog(savedInstanceState)

        val inflater = activity?.layoutInflater
        val nullParent: ViewGroup? = null
        val view = inflater?.inflate(R.layout.my_dialog, nullParent)

        val builder = AlertDialog.Builder(activity)

        builder.setTitle(R.string.title)
                .setView(view)
                .setPositiveButton(R.string.ok) { dialog, id ->
                    // ここで何かしらの処理をしたあとダイアログは勝手に閉じる
                }
                .setNegativeButton(R.string.cancel) { dialog, id ->
                    getDialog().cancel()
                }
        return builder.create()
    }
}
```

上記の`MyDialogFragment`のインスタンスを作成して`show()`を呼び出すとダイアログが表示されます。

```kotlin
val dialog = MyDialogFragment()
dialog.show(supportFragmentManager, "dialog")
```

ここまででダイアログを実装することができました。

## ダイアログを閉じないようにする

本題はこちらです。

```kotlin:MyDialogFragment.kt
val d = builder.setTitle(R.string.title)
        .setView(view)
        .setPositiveButton(R.string.ok, null)
        .setNegativeButton(R.string.cancel) { dialog, id ->
            getDialog().cancel()
        }
        .create()

d.show()
d.getButton(AlertDialog.BUTTON_POSITIVE).setOnClickListener {
        // ここで何かしらの処理をしたあとdialog.cancel()を呼べばダイアログを閉じることができる
//      dialog.cancel()
}
return d
```

show()した後にボタンをクリックをした時の処理を実装することで、ダイアログが閉じるのを防ぐことができます。

ダイアログを閉じるためには`dialog.cancel()`または`dismiss()`を呼びます。

###### 参考

- <https://developer.android.com/guide/topics/ui/dialogs?hl=en>
- <https://stackoverflow.com/questions/2620444/how-to-prevent-a-dialog-from-closing-when-a-button-is-clicked/15619098#15619098>
