---
id: 340
title: Building an Android App from Scratch using MVP
date: 2017-09-12T06:00:07+00:00
author: Marcel Jurtz
layout: post
guid: http://blog.mjurtz.com/?p=340
permalink: /2017/09/building-android-app-scratch-using-mvp/
vcv-be-editor:
  - classic
categories:
  - android
tags:
  - android
  - app
  - baseball
  - development
---
In this article, I'll document the process of building an android app from scratch by using the MVP approach of clean architecture. The app itself will represent a minimalistic umpire indicator for tracking baseball games. Please consider using this in the dugout only. Since this will be a longer post, you can use the following links to jump between sections.

  1. [What is MVP?](#what_is_mvp)
  2. [Background on the app to be built](#app_background)
  3. [Constructing the GUI](#gui)
  4. [Managing interfaces](#interfaces)
  5. [Building the model](#model)
  6. [Connecting view and model by adding the presenter](#presenter)

<h3 id="what_is_mvp" style="padding-top: 150px; margin-top: -150px;">
  1. What is MVP?
</h3>

MVP is a design pattern which helps you to build clean, modular code, that can easily be tested. It consists of the model, the view and the presenter. The view just displays your GUI and can be represented by an activity. The model contains your business logic. The presenter connects these parts. This approach has the advantage, that you can build your model using pure Java, without any Android compontents.

Usually, guides on how to implement MVP are based on several frameworks, like dagger2 for dependency injection. I found these guides very hard to get started as a beginner, so I implemented my interpretation of MVP without using any third-party framework.

<h3 id="app_background" style="padding-top: 150px; margin-top: -150px;">
  2. Background on the app to be built
</h3>

For anyone who is not familiar with baseball, this section gives a short summary of the concept of an umpires indicator. A baseball game usually consists of nine innings. Each team has a offensive half of an inning and a defensive half. After scoring three outs, the teams switch places. By scoring three outs, a player receives an out. By scoring four balls, a player receives a walk, which allows him to continue to the first base. The mentioned balls and strikes only apply to the batter, the player who is currently located on the home plate and tries to hit a pitch.

The umpires indicator is used to track outs, balls and strikes (in this case also innings). The tool is usually a small mechanical gadget. The app will contain a simple GUI which covers this functionality. To keep this post simple, the functionality of the app will be quite minimalistic. Balls, Strikes, Outs and Innings will be represented by a number. The value range of each of these will match the pendant in real baseball games. The values will increase by tapping the text, when the value is higher than the maximum, it will reset to the minimum.

The full sourcecode to this project is available on [GitHub](https://github.com/MarcelJurtz/UmpireIndicator).

<h3 id="gui" style="padding-top: 150px; margin-top: -150px;">
  3. Constructing the GUI
</h3>

You can use several tools for GUI-prototyping. I've stumbled across [Pencil on GitHub](https://github.com/prikhi/pencil), which looks pretty neat for this job. This wonderful example also shows, that I'm a talentless designer.

<img src="/assets/2017/android-mvp-gui.png" style="width: 300px">

Like the physical umpire indicators, this one will only be able to increase the counts. Each element will have a maximum value, after which the value will reset to its minimum value. I've decided to add TextViews above the different numbers to be able to differentiate between them.

This layout has been realised in my MainActivity.xml file:

```xaml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context="com.jurtz.marcel.umpireindicator.MainActivity">


    <!-- Outs -->

     <TextView
        android:id="@+id/txtOuts"
        style="@style/ScoreTextView"
        android:layout_centerHorizontal="true"
        android:layout_centerVertical="true"
         android:layout_marginBottom="@dimen/margin_main"
        android:text="O" />

    <!-- Innings -->

    <TextView
        android:id="@+id/txtInnings"
        style="@style/ScoreTextView"
        android:layout_centerHorizontal="true"
        android:layout_alignParentBottom="true"
        android:layout_marginBottom="@dimen/margin_main"
        android:text="I" />

    <!-- Balls -->

    <TextView
        android:id="@+id/txtBalls"
        style="@style/ScoreTextView"
        android:layout_alignParentLeft="true"
        android:layout_alignParentTop="true"
        android:layout_marginTop="@dimen/margin_main"
        android:layout_marginLeft="@dimen/margin_main"
        android:text="B" />

    <!-- Strikes -->

    <TextView
        android:id="@+id/txtStrikes"
        style="@style/ScoreTextView"
        android:layout_alignParentRight="true"
        android:layout_alignParentTop="true"
        android:layout_marginTop="@dimen/margin_main"
        android:layout_marginRight="@dimen/margin_main"
        android:text="S"/>

    <!-- Descriptions -->

    <TextView
        android:id="@+id/lblBallsDescription"
        style="@style/DescriptionTextView"
        android:layout_alignLeft="@+id/txtBalls"
        android:layout_alignStart="@+id/txtBalls"
        android:layout_alignRight="@id/txtBalls"
        android:layout_alignEnd="@id/txtBalls"
        android:layout_alignTop="@+id/txtBalls"
        android:text="Ball" />

    <TextView
        android:id="@+id/lblStrikesDescription"
        style="@style/DescriptionTextView"
        android:layout_alignLeft="@+id/txtStrikes"
        android:layout_alignStart="@+id/txtStrikes"
        android:layout_alignRight="@id/txtStrikes"
        android:layout_alignEnd="@id/txtStrikes"
        android:layout_alignTop="@+id/txtStrikes"
        android:text="Strike" />

    <TextView
        android:id="@+id/lblOutsDescription"
        style="@style/DescriptionTextViewCenter"
        android:layout_alignTop="@+id/txtOuts"
        android:text="Out" />

    <TextView
        android:id="@+id/lblInningsDescription"
        style="@style/DescriptionTextViewCenter"
        android:layout_alignTop="@+id/txtInnings"
        android:text="Inning" />

</RelativeLayout>
```

I use stylings in here for minimizing duplicate entries:

```xaml
<resources>
    <!-- Base application theme. -->
    <style name="AppTheme" parent="Base.Theme.AppCompat.Light.DarkActionBar">
        <!-- Customize your theme here. -->
        <item name="colorPrimary">@color/colorPrimary</item>
        <item name="colorPrimaryDark">@color/colorPrimaryDark</item>
        <item name="colorAccent">@color/colorAccent</item>
    </style>

    <style name="ScoreTextView">
        <item name="android:layout_width">wrap_content</item>
        <item name="android:layout_height">wrap_content</item>
        <item name="android:layout_gravity">center_horizontal</item>
        <item name="android:textSize">64dp</item>
        <item name="android:padding">@dimen/padding_main</item>
    </style>

    <style name="DescriptionTextView">
        <item name="android:layout_width">match_parent</item>
        <item name="android:layout_height">wrap_content</item>
        <item name="android:gravity">center</item>
    </style>

    <style name="DescriptionTextViewCenter">
        <item name="android:layout_width">match_parent</item>
        <item name="android:layout_height">wrap_content</item>
        <item name="android:layout_centerHorizontal">true</item>
        <item name="android:gravity">center</item>
    </style>

</resources>
```

Also, I'm using a _dimen.xml_ file to store size-related values:

```xaml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <dimen name="margin_main">50dp</dimen>
    <dimen name="padding_main">25dp</dimen>
</resources>
```

The next part will be separating the GUI from the logic by creating interfaces.

<h3 id="interfaces" style="padding-top: 150px; margin-top: -150px;">
  4. Managing Interfaces
</h3>

I'll add two interfaces for this project. One for the presenter, and one for the view. The interface that will be implemented by the presenter needs to be able to handle user interactions. Therefore, the following methods will be contained in this interface:

```java
public interface IUmpIndicatorPresenter {
    void onCreate(IUmpIndicatorView view);
    void onPause();
    void onResume();
    void onDestroy();

    void onBallClick();
    void onStrikeClick();
    void onOutClick();
    void onInningClick();
}
```

The first four methods are for general activity lifecycle handling. The _onCreate()_-method receives an instance of the _IUmpIndicatorView_ interface, which will be implemented by the view (represented by the MainActivity). The other four methods are used to handle user interactions with the TextViews.

The view implements the interface _IUmpIndicatorView_, which is shown below:

```java
public interface IUmpIndicatorView  {
    void setBallText(String text);
    void setStrikeText(String text);
    void setOutText(String text);
    void setInningText(String text);
}
```

This interface defines methods of the view, that will be accessed by the presenter. These methods are used to change the values of the TextViews.

<h3 id="model" style="padding-top: 150px; margin-top: -150px;">
  5. Building the model
</h3>

The model is responsible for managing the game itself. Therefore, it has values for the current balls, strikes, outs and inning. It also defines maximum values for each of these, as well as getter-methods to wrap the private properties.

```java
public class Game {
    private int balls;
    private int strikes;
    private int outs;
    private int inning;

    private final int BALLS_MIN = 0;
    private final int BALLS_MAX = 3;

    private final int STRIKES_MIN = 0;
    private final int STRIKES_MAX = 2;

    private final int OUTS_MIN = 0;
    private final int OUTS_MAX = 2;

    private final int INNINGS_MIN = 1;
    private final int INNINGS_MAX = 9;

    public Game() {
        balls = BALLS_MIN;
        strikes = STRIKES_MIN;
        outs = OUTS_MIN;
        inning = INNINGS_MIN;
    }

    //region Getter
    public int getBalls() {
        return balls;
    }

    public int getStrikes() {
        return strikes;
    }

    public int getOuts() {
        return outs;
    }

    public int getInning() {
        return inning;
    }
    //endregion

    //region Increase Values
    public void increaseBalls() {
        balls++;
        if(balls > BALLS_MAX) balls = BALLS_MIN;
    }

    public void increaseStrikes() {
        strikes++;
        if(strikes > STRIKES_MAX) strikes = STRIKES_MIN;
    }

    public void increaseOuts() {
        outs++;
        if(outs > OUTS_MAX) outs = OUTS_MIN;
    }

    public void increaseInnings() {
        inning++;
        if(inning > INNINGS_MAX) inning = INNINGS_MIN;
    }
    //endregion
}
```

<h3 id="presenter" style="padding-top: 150px; margin-top: -150px;">
  6. Connecting view and model by adding the presenter
</h3>

The last part is the combination of both elements. An instance of the presenter-interface needs to be built. This class sets the content of each of the views elements and updates the different properties on user interaction:

```java
public class UmpIndicatorPresenter implements IUmpIndicatorPresenter {

    Game game;
    IUmpIndicatorView view;

    //region Activity Lifecycle Methods
    @Override
    public void onCreate(IUmpIndicatorView view) {
        game = new Game();
        this.view = view;

        view.setBallText(String.valueOf(game.getBalls()));
        view.setStrikeText(String.valueOf(game.getStrikes()));
        view.setOutText(String.valueOf(game.getOuts()));
        view.setInningText(String.valueOf(game.getInning()));
    }

    @Override
    public void onPause() {

    }

    @Override
    public void onResume() {

    }

    @Override
    public void onDestroy() {

    }
    //endregion

    //region View Click Handlind
    @Override
    public void onBallClick() {
        game.increaseBalls();
        view.setBallText(String.valueOf(game.getBalls()));
    }

    @Override
    public void onStrikeClick() {
        game.increaseStrikes();
        view.setStrikeText(String.valueOf(game.getStrikes()));
    }

    @Override
    public void onOutClick() {
        game.increaseOuts();
        view.setOutText(String.valueOf(game.getOuts()));
    }

    @Override
    public void onInningClick() {
        game.increaseInnings();
        view.setInningText(String.valueOf(game.getInning()));
    }
    //endregion
}
```

The view will be covered by the existing MainActivity.java file and just forward user interactions to the presenter:

```java
package com.jurtz.marcel.umpireindicator;

import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.view.View;
import android.widget.TextView;

import com.jurtz.marcel.umpireindicator.Interface.IUmpIndicatorPresenter;
import com.jurtz.marcel.umpireindicator.Interface.IUmpIndicatorView;
import com.jurtz.marcel.umpireindicator.Interface.UmpIndicatorPresenter;

public class MainActivity extends AppCompatActivity implements IUmpIndicatorView {

    TextView txtBalls;
    TextView txtStrikes;
    TextView txtOuts;
    TextView txtInnings;

    IUmpIndicatorPresenter presenter;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        txtBalls = (TextView)findViewById(R.id.txtBalls);
        txtStrikes = (TextView)findViewById(R.id.txtStrikes);
        txtOuts = (TextView)findViewById(R.id.txtOuts);
        txtInnings = (TextView)findViewById(R.id.txtInnings);

        presenter = new UmpIndicatorPresenter();
        presenter.onCreate(this);

        //region TextView onClickListeners
        txtBalls.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                presenter.onBallClick();
            }
        });

        txtStrikes.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                presenter.onStrikeClick();
            }
        });

        txtOuts.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                presenter.onOutClick();
            }
        });

        txtInnings.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                presenter.onInningClick();
            }
        });
        //endregion
    }

    @Override
    protected void onDestroy() {
        presenter.onDestroy();
        super.onDestroy();
    }

    @Override
    protected void onPause() {
        presenter.onPause();
        super.onPause();
    }

    @Override
    protected void onResume() {
        super.onResume();
        presenter.onResume();
    }

    @Override
    public void setBallText(String text) {
        txtBalls.setText(text);
    }

    @Override
    public void setStrikeText(String text) {
        txtStrikes.setText(text);
    }

    @Override
    public void setOutText(String text) {
        txtOuts.setText(text);
    }

    @Override
    public void setInningText(String text) {
        txtInnings.setText(text);
    }
}
```