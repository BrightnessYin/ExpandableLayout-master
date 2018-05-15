# Expandable Layout

此库带来了可扩展的布局和各种动画。
可以在任何地方包括可选内容和使用。

[![Circle CI](https://circleci.com/gh/AAkira/ExpandableLayout.svg?style=shield&circle-token=cb7464e0215aaecb740936d1d68010ee0ffd4806)](https://circleci.com/gh/AAkira/ExpandableLayout)
[![Platform](http://img.shields.io/badge/platform-android-brightgreen.svg?style=flat)](http://developer.android.com/index.html)
[![Language](http://img.shields.io/badge/language-java-orange.svg?style=flat)](http://www.oracle.com/technetwork/java/javase/downloads/index.html)
[![License](http://img.shields.io/badge/license-apache2.0-lightgrey.svg?style=flat)](http://www.apache.org/licenses/LICENSE-2.0)
[![Android Arsenal](https://img.shields.io/badge/Android%20Arsenal-ExpandableLayout-brightgreen.svg?style=flat)](http://android-arsenal.com/details/1/2456)
[![Android Gems](http://www.android-gems.com/badge/AAkira/ExpandableLayout.svg?branch=master)](http://www.android-gems.com/lib/AAkira/ExpandableLayout)

## Preview

### Normal

![ExpandableRelativeLayout][ExpandableRelativeLayout] ![ExpandableWeightLayout][ExpandableWeightLayout]

### Example

![ExampleRecyclerView][ExampleRecyclerView] ![ExampleSearch][ExampleSearch]
![ExampleReadMore][ExampleReadMore]

## Usage

### ExpandableRelativeLayout

#### Usage

如果子视图改变大小，expandableRelativeLayout就不起作用了。
此时应该使用ExpandableLinearLayout。

#### Code

```java
ExpandableRelativeLayout expandableLayout
 = (ExpandableRelativeLayout) findViewById(R.id.expandableLayout);

// toggle expand, collapse
expandableLayout.toggle();
// expand
expandableLayout.expand();
// collapse
expandableLayout.collapse();

// move position of child view
expandableLayout.moveChild(0);
// move optional position
expandableLayout.move(500);

// set base position which is close position
expandableLayout.setClosePosition(500);
```

#### Layout xml

add `xmlns:app="http://schemas.android.com/apk/res-auto"`

```xml
<com.github.aakira.expandablelayout.ExpandableRelativeLayout
    android:id="@+id/expandableLayout"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    app:ael_expanded="false"
    app:ael_duration="500"
    app:ael_interpolator="bounce"
    app:ael_orientation="vertical">

    <TextView
        android:id="@+id/text"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="sample" />
    <TextView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_below="@id/text"
        android:text="sample2" />
</com.github.aakira.expandablelayout.ExpandableRelativeLayout>
```

### ExpandableLinearLayout

#### Usage

如果子视图可能改变大小，则应该使用ExpandableLinearLayout。
例如，它从服务器获取和设置值。
你应该在回收器的视图中使用这个。

#### Code

```java

// resize expandable layout

ExpandableLinearLayout expandableLayout
 = (ExpandableLinearLayout) findViewById(R.id.expandableLayout);

child.setText("Sets text from a server");
expandableLayout.initLayout(); // Recalculate size of children

// recycler view
// you must set a ViewHolder#setIsRecyclable(false) and ExpandableLinearLayout#setInRecyclerView(true) 

@Override
public void onBindViewHolder(final ViewHolder holder, final int position) {
    holder.setIsRecyclable(false);
    holder.expandableLinearLayout.setInRecyclerView(true);
}
```

### ExpandableWeightLayout

#### Usage

You should use this layout if you want to use weight attributes at expandable layout.

#### Code

```java
ExpandableWeightLayout expandableLayout
 = (ExpandableWeightLayout) findViewById(R.id.expandableLayout);

// toggle expand, collapse
expandableLayout.toggle();
// expand
expandableLayout.expand();
// collapse
expandableLayout.collapse();
```

#### Layout xml

add `xmlns:app="http://schemas.android.com/apk/res-auto"`

```xml
<LinearLayout
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <View
        android:layout_width="match_parent"
        android:layout_height="0dp"
        android:layout_weight="1"/>

    <com.github.aakira.expandablelayout.ExpandableWeightLayout
        android:id="@+id/expandableLayout"
        android:layout_width="match_parent"
        android:layout_height="0dp"
        android:layout_weight="3"
        app:ael_duration="1000"
        app:ael_interpolator="anticipateOvershoot">

        <ImageView
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_centerInParent="true"
            android:src="@drawable/sample" />
    </com.github.aakira.expandablelayout.ExpandableWeightLayout>

    <View
        android:layout_width="match_parent"
        android:layout_height="0dp"
        android:layout_weight="1"/>
</LinearLayout>
```

### Listener

```java

expandableLayout.setListener(new ExpandableLayoutListener() {
    @Override
    public void onAnimationStart() {
    }

    @Override
    public void onAnimationEnd() {
    }

    // You can get notification that your expandable layout is going to open or close.
    // So, you can set the animation synchronized with expanding animation.
    @Override
    public void onPreOpen() {
    }

    @Override
    public void onPreClose() {
    }

    @Override
    public void onOpened() {
    }

    @Override
    public void onClosed() {
    }
});
```
* `ExpandableLayoutListenerAdapter`
 - You can set listeners only you need. 

```java

expandableLayout.setListener(new ExpandableLayoutListenerAdapter() {
    @Override
    public void onPreOpen() {
    }

    @Override
    public void onPreClose() {
    }
});

```

### Attributes

|attribute name|description|
|:-:|:-:|
|ael_duration|The length of the expand or collapse animation|
|ael_expanded|The layout is expanded if you set true|
|ael_defaultChildIndex|The layout is expanded at index of child view. (Only `ExpandableRelativeLayout`)|
|ael_defaultPosition|The layout is expanded at the position. (Only `ExpandableRelativeLayout`)|
|ael_orientation|The orientation of animation(horizontal \| vertical)|
|ael_interpolator|Sets [interpolator](#interpolator)|

### Interpolator

You can use [interpolator](http://developer.android.com/reference/android/view/animation/Interpolator.html).
It helps the layout animates easily.

|Interpolator|value name of attribute |
|:-:|:-:|
|[AccelerateDecelerateInterpolator](http://developer.android.com/reference/android/view/animation/AccelerateDecelerateInterpolator.html)|accelerateDecelerate|
|[AccelerateInterpolator](http://developer.android.com/reference/android/view/animation/AccelerateInterpolator.html)|accelerate|
|[AnticipateInterpolator](http://developer.android.com/reference/android/view/animation/AnticipateInterpolator.html)|anticipate|
|[AnticipateOvershootInterpolator](http://developer.android.com/reference/android/view/animation/AnticipateOvershootInterpolator.html)|anticipateOvershoot|
|[BounceInterpolator](http://developer.android.com/reference/android/view/animation/BounceInterpolator.html)|bounce|
|[DecelerateInterpolator](http://developer.android.com/reference/android/view/animation/DecelerateInterpolator.html)|decelerate|
|[FastOutLinearInInterpolator](http://developer.android.com/reference/android/support/v4/view/animation/FastOutLinearInInterpolator.html)|fastOutLinearIn|
|[FastOutSlowInInterpolator](http://developer.android.com/reference/android/support/v4/view/animation/FastOutSlowInInterpolator.html)|fastOutSlowIn|
|[LinearInterpolator](http://developer.android.com/reference/android/view/animation/LinearInterpolator.html)|linear|
|[LinearOutSlowInInterpolator](http://developer.android.com/reference/android/support/v4/view/animation/LinearOutSlowInInterpolator.html)|linearOutSlowIn|
|[OvershootInterpolator](http://developer.android.com/reference/android/view/animation/OvershootInterpolator.html)|overshoot|

这些都是支持插入器。
但是，可扩展布局扩展到外部的情况是行不通的。
例如AnticipateInterpolator,AnticipateOvershootInterpolator OvershootInterpolator
我建议在可扩展的布局中使用这样的插值器。

* Not support
 - CycleInterpolator
 - PathInterpolator

## Setup

### Gradle

```

* Mail
 - brightnessyin@163.com


## License

```


Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
```

[ExpandableRelativeLayout]: /art/ExpandableRelativeLayout.gif
[ExpandableWeightLayout]: /art/ExpandableWeightLayout.gif
[ExampleSearch]: /art/ExampleSearch.gif
[ExampleRecyclerView]: /art/ExampleRecyclerview_v1.1.gif
[ExampleReadMore]: /art/ExampleReadMore.gif
