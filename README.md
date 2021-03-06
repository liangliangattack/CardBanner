# CardBanner

[![](https://jitpack.io/v/fengminchao/CardBanner.svg)](https://jitpack.io/#fengminchao/CardBanner)

卡片式 Banner,支持无限循环滚动,自动滚动播放.

## 效果图

![](./screenshots/cardexample.png)

## 安装

### Gradle
- 在项目的 build.gradle 中添加 JitPack 仓库地址.

```
allprojects {
	repositories {
		...
		maven { url 'https://jitpack.io' }
	}
}
```

- 在需要使用的 module (如 app 文件)中的 build.gradle 添加依赖

```
	dependencies {
	        compile 'com.github.fengminchao:CardBanner:1.1.3'
	}
```

## 使用

### In xml

```xml
   <com.muxistudio.cardbanner.CardBanner
        android:id="@+id/card_banner"
        android:layout_width="match_parent"
        android:layout_height="150dp"
        android:layout_centerVertical="true"
        app:cardMargin="16dp"
        app:baseElevation="4dp"
        app:cardRadius="8dp"
        app:scaleRatio="1.1"
        app:isLoop="false"
        app:sideCardWidth="16dp"/>
```

- cardMargin: 卡片的水平间距
- baseElevation: 卡片的初始高度
- scaleRatio: 中间的卡片放大比率
- isLoop: 是否可无限循环滑动
- sideCardWidth: 两边的卡片所显示的宽度

具体内部卡片的宽高设置如代码内部所示，其实在设置上面几个属性的时候已经影响了卡片的宽高
```java
//这是卡片还没被缩放时的宽高，其实cardMargin在卡片Z轴高度为0，且没有scaleRatio为1时是准的，其他情况下还要自己计算下，根据计算所得来调整出一个适合自己 banner 图片的卡片宽高比，ELEVATION_2_PLANE_RATIO是为了卡片的阴影能展示出来给其留了一定空间。
cardParams.width = (int)((mCardView.getWidth() - cardMargin - mBaseElevation * ELEVATION_2_PLANE_RATIO * mScaleRatio * 2) / mScaleRatio);
cardParams.height = (int)((mCardView.getHeight() - mBaseElevation * mScaleRatio * ELEVATION_2_PLANE_RATIO * 2) / mScaleRatio);
```

### In Java

```java

    private CardBanner mCardBanner;

    private List<ViewHolder<Integer>> mViewHolders = new ArrayList<>();

    private Integer[] resIds = {R.drawable.album,R.drawable.ow,R.drawable.bili};
    private List<Integer> resIdList = new ArrayList<>();
    
 
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        mCardBanner = (CardBanner) findViewById(R.id.card_banner);

        for (int i = 0;i < 3;i ++) {
        //初始化 ViewHolder
            ViewHolder<Integer> viewHolder = new ViewHolder<Integer>() {
            //在 ViewHolder 的 getView方法中返回最终要显示在Banner中的View, 因为考虑到项目中用 Fresco 做图片加载所以没固定写 ImageView.
                @Override
                public View getView(Context context, Integer data) {
                    ImageView imageView = new ImageView(MainActivity.this);
                    imageView.setImageResource(data);
                    imageView.setScaleType(ImageView.ScaleType.CENTER_CROP);
                    return imageView;
                }
            };
            mViewHolders.add(viewHolder);
        }
        
        resIdList = Arrays.asList(resIds);
        //设置自动滑动方向，放置于setViewHolders之前
        mCardBanner.setScrollDirection(CardBanner.DIRECTION_FORWARD);
        //传递 ViewHolders 和 对应的数据 给  CardBanner
        mCardBanner.setViewHolders(mViewHolders,resIdList);
        //开启自动滚动播放
        mCardBanner.setAutoScroll(true);
        //设置滚动间隔时间
        mCardBanner.setScrollDuration(3000);
        //设置滚动动画所需时间
        mCardBanner.setScrollTime(500);
    }
```

## ChangeLogs

### v1.1.3
- 添加设置自动滑动方向的方法

### v1.1.2
- 修复在 fragment 中使用 cardbanner 出现不显示的 bug

### v1.1.1
- 修复 isLoop="true"时的 bug

### v1.1
- 修复一些 bug
- 增加一些可配置的属性

### v1.0
- 初始版本发布

## Thanks
[ViewPagerCards](https://github.com/rubensousa/ViewPagerCards)

[Android-ConvenientBanner](https://github.com/saiwu-bigkoo/Android-ConvenientBanner)

