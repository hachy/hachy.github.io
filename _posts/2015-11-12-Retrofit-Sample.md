---
layout: post
title: "Retrofit を試してみる"
date: 2015-11-12 17:12:25 +0900
tags: android
---

GitHubのAPIを使って`hachy`というユーザーのリポジトリ一覧をListViewで表示してみます。
`https://api.github.com/users/hachy/repos` というURLでリポジトリの情報を得られます。

AndroidManifest.xml にpermissionを追加

```xml:AndroidManifest.xml
<uses-permission android:name="android.permission.INTERNET"/>
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
```

build.gradle の dependencies に追加

```gradle:build.gradle
compile 'com.squareup.retrofit:retrofit:2.0.0-beta2'
compile 'com.squareup.retrofit:converter-gson:2.0.0-beta2'
```

```java:GitHubService.java
package com.example.retrofitsample;

import java.util.List;

import retrofit.Call;
import retrofit.http.GET;
import retrofit.http.Path;

public interface GitHubService {
    @GET("/users/{user}/repos")
    Call<List<Repo>> listRepos(@Path("user") String user);
}
```

```java:Repo.java
package com.example.retrofitsample;

public class Repo {
    public String name;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}
```

```java:MainActivity.java
package com.example.retrofitsample;

import android.os.Bundle;
import android.support.v7.app.AppCompatActivity;
import android.support.v7.widget.Toolbar;
import android.widget.ArrayAdapter;
import android.widget.ListView;

import java.util.ArrayList;
import java.util.List;

import retrofit.Call;
import retrofit.Callback;
import retrofit.GsonConverterFactory;
import retrofit.Response;
import retrofit.Retrofit;

public class MainActivity extends AppCompatActivity {
    private ArrayList<String> items;
    private ListView listView;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        Toolbar toolbar = (Toolbar) findViewById(R.id.toolbar);
        setSupportActionBar(toolbar);

        listView = (ListView) findViewById(R.id.list_view);

        Retrofit retrofit = new Retrofit.Builder()
                .baseUrl("https://api.github.com")
                .addConverterFactory(GsonConverterFactory.create())
                .build();

        GitHubService service = retrofit.create(GitHubService.class);
        Call<List<Repo>> repos = service.listRepos("hachy");

        repos.enqueue(new Callback<List<Repo>>() {
            @Override
            public void onResponse(Response<List<Repo>> response, Retrofit retrofit) {
                items = new ArrayList<>();
                for (Repo res : response.body()) {
                    items.add(res.getName());
                }
                ArrayAdapter<String> adapter = new ArrayAdapter<>(MainActivity.this, android.R.layout.simple_list_item_1, items);
                listView.setAdapter(adapter);
            }

            @Override
            public void onFailure(Throwable t) {
            }
        });
    }
}
```

リポジトリ一覧が表示された。

![listview](/assets/images/retrofit-sample.png "Retrofit-sample")
