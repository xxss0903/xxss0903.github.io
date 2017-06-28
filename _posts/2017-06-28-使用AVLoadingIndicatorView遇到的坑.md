
--
layout: blog
title: '使用AVLoadingIndicatorView遇到的坑'
date: 2017-06-20 12:11:34
categories: blog
tags: code
image: ''
lead-text: '使用AVLoadingIndicatorView遇到的坑，动画不动'
---

```
添加了AVLoadingIndicatorView后出现柱子但是没有动画
```

最近使用AVLoadingIndicatorView的时候，使用了他的源码，放入了自己项目中的一个包中，然后在加入的时候<br>
更换了AVLoadingIndicatorView的包名，然后在具体使用的时候就导致加入之后没有动画了，虽然页会绘制出柱子<br>
但是却没有动画效果。

经过反复排查，找到原因是在Indicator类中，此类是继承字Drawable的，通过调用它的invalidateSelf方法来反复<br>
调用draw方法来绘制动画效果，看invalidateSelf的源码

```java
    /**
     * Use the current {@link Callback} implementation to have this Drawable
     * redrawn.  Does nothing if there is no Callback attached to the
     * Drawable.
     *
     * @see Callback#invalidateDrawable
     * @see #getCallback()
     * @see #setCallback(android.graphics.drawable.Drawable.Callback)
     */
    public void invalidateSelf() {
        final Callback callback = getCallback();
        if (callback != null) {
            callback.invalidateDrawable(this);
        }
    }
```

如果没有添加callback那么将不会做任何事，发现是前面的callback对象没有正确添加，所以回去查看<br> **AVLoadingIndicatorView** 的源码
找到设置callback的地方

在 **AVLoadingIndicatorView** 类中找到setIndicator方法，这个方法通过类名来获取到对应indicator<br>
在其中调用了一个重载的setIndicator方法穿入一个indicator对象，在重载方法中会调用setCallback方法来<br>
设置回调
通过debug发现这里捕获了 NotFindClassException，原来是在穿入类名的时候写错了包名导致，所以在<br>
使用类名传递的时候注意你的 **包名**

```java
 /**
     * You should pay attention to pass this parameter with two way:
     * for example:
     * 1. Only class Name,like "SimpleIndicator".(This way would use default package name with
     * "com.wang.avi.indicators")
     * 2. Class name with full package,like "com.my.android.indicators.SimpleIndicator".
     *
     * @param indicatorName the class must be extend Indicator .
     */
    public void setIndicator(String indicatorName) {
        if (TextUtils.isEmpty(indicatorName)) {
            return;
        }
        StringBuilder drawableClassName = new StringBuilder();
        if (!indicatorName.contains(".")) {
            String defaultPackageName = getClass().getPackage().getName();
            drawableClassName.append(defaultPackageName)
                    .append(".");
        }
        drawableClassName.append(indicatorName);
        try {
            Class<?> drawableClass = Class.forName(drawableClassName.toString());
            Indicator indicator = (Indicator) drawableClass.newInstance();
            setIndicator(indicator);
        } catch (ClassNotFoundException e) {
            Log.e(TAG, "Didn't find your class , check the name again !");
        } catch (InstantiationException e) {
            e.printStackTrace();
        } catch (IllegalAccessException e) {
            e.printStackTrace();
        }
    }


    public void setIndicator(Indicator d) {
        if (mIndicator != d) {
            if (mIndicator != null) {
                mIndicator.setCallback(null);
                unscheduleDrawable(mIndicator);
            }

            mIndicator = d;
            //need to set indicator color again if you didn't specified when you update the indicator .
            setIndicatorColor(mIndicatorColor);
            if (d != null) {
                d.setCallback(this);
            }
            postInvalidate();
        }
    }

```
