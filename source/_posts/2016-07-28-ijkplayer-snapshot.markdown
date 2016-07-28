---
layout: post
title: "Ijkplayer Snapshot"
date: 2016-07-28 11:22:32 +0800
comments: true
categories: Android ijkplayer
keywords: ijkplayer snpshot
description: Ijkplaer Snapshot
---
###Issue
在Android使用Ijkplayer播放影片, 預設並沒有支援rtsp以及截圖功能,
因此修改config增加支援rtsp串流播放, 以及增加截圖功能。 

<!--more-->
###Solution

參考原始的<a href=https://github.com/bbcallen/ijkplayer>ijkplayer</a> ,以及<a href=https://github.com/jgfntu/ijkplayer/tree/snapshort>jgfntu</a>增加截圖功能.

如下修改後的projet已經匯整在我的Github: <a href=https://github.com/Tuzr/ijkplayer>ijkplayer</a>

----
詳細方法如下:

下載

從https://github.com/Bilibili/ijkplayer 下載

設定好環境變數 , ANDROID_SDK, ANDROID_NDK等.

#####<font color=#FF0000>*下載之後, 需要修改config改為使用module-default.sh</font>

	cd config
	rm module.sh
	ln -s module-default.sh module.sh
#####<font color=#FF0000>*參照下列底層檔案修改方式修改</font>
* ijkmedia/ijkplayer/Android.mk
{% img /images//2016/07/20160728_1.png %}

* ijkmedia/ijkplayer/android/ijkplayer_jni.c
{% img /images//2016/07/20160728_2.png %}
{% img /images//2016/07/20160728_3.png %}
{% img /images//2016/07/20160728_4.png %}

* ijkmedia/ijkplayer/ff_ffplay.c
{% img /images//2016/07/20160728_5.png %}

依照下列官方方式建置

	git clone https://github.com/Bilibili/ijkplayer.git ijkplayer-android
	cd ijkplayer-android
	git checkout -B latest k0.5.1

	./init-android.sh

	cd android/contrib
	./compile-ffmpeg.sh clean
	./compile-ffmpeg.sh all

	cd ..
	./compile-ijk.sh all

建置完成後, 打開ijkplayer專案 (/android/ijkplayer)
即可看到各個平台的lib以及範例App

#####<font color=#FF0000>**因為底層增加截圖function, 所以Java程式也需要增加對應的function, 否則會在載入lib的時候報錯.</font>

<a href =https://github.com/jgfntu/ijkplayer/commit/fe3a46d2032e6468e490a3c6df927eea59dc7c4e>參考Java修改紀錄</a>

* IMediaPlayer.java 增加getCurrentFrame <br>
{% img /images//2016/07/20160728_6.png %}

* IJKMediaPlayer.java <br>
{% img /images//2016/07/20160728_7.png %}

* MediaPlayerProxy.java <br>
{% img /images//2016/07/20160728_8.png %}
* ijkVideoView.java <br>
{% img /images//2016/07/20160728_9.png %}
* ijkExoMdeiaPlayer.java <br>
{% img /images//2016/07/20160728_10.png %}
* AndroidMediaPlayer <br>
{% img /images//2016/07/20160728_11.png %}

#####<font color=#FF0000>*到目前為止,只是編譯會過, 執行RSTP正常!</font> <br>
#####<font color=#FF0000>*要測試抓圖功能, 可以參考下列Github專案 , Branch:snapshot</font>
<a href=https://github.com/jgfntu/ijkplayer/blob/snapshort/android/ijkmediademo/src/tv/danmaku/ijk/media/demo/VideoPlayerActivity.java>Demo</a> <br>
<a href=https://github.com/jgfntu/ijkplayer/blob/snapshort/android/ijkmediademo/src/tv/danmaku/ijk/media/demo/ImageUtils.java>ImageUtils.java</a>


###參考資料
<a href=https://github.com/jgfntu/ijkplayer/tree/snapshort>選擇Branch: snapshot</a>

<a href=https://github.com/jgfntu/ijkplayer/commit/b1efc14f88cc792ed1c221b9f4e257e27175762a>底層檔案修改紀錄</a>

<a href=https://github.com/jgfntu/ijkplayer/commit/fe3a46d2032e6468e490a3c6df927eea59dc7c4e>Java檔案修改紀錄</a>


