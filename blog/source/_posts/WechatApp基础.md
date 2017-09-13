---
title: WechatApp基础
toc: true
date: 2017-08-24 21:50:30
tags: WechatApp
categories: WechatApp
---

小程序开发框架的目标是通过尽可能简单、高效的方式让开发者可以在微信中开发具有原生 APP 体验的服务。框架提供了自己的视图层描述语言 WXML 和 WXSS，以及基于 JavaScript 的逻辑层框架，并在视图层与逻辑层间提供了数据传输和事件系统，可以让开发者可以方便的聚焦于数据与逻辑上。

<!--more-->

## 从本地相册选择图片或使用相机拍照

**使用到的API是**：`wx.chooseImage(OBJECT)`

html代码：
```bash
<view bindtap="bindViewTap" class="userinfo">
    <image class="userinfo-avatar" src="{{avatarUrl}}" background-size="cover"></image>
    <text class="userinfo-nickname">{{userInfo.nickName}}</text>
</view>
```

css代码：
```wxss
.userinfo { display: flex; flex-direction: column; align-items: center; }
.userinfo-avatar { width: 128rpx; height: 128rpx; margin: 20rpx; border-radius: 50%; }
.userinfo-nickname { color: #aaa; }
```

点击部分事件处理：
```js
data: {
    userInfo: {},
    avatarUrl: null
}, 
bindViewTap: function() {
    var that = this;
    wx.chooseImage({
      count: 1, // 默认9
      sizeType: ['original', 'compressed'], // 可以指定是原图还是压缩图，默认二者都有
      sourceType: ['album', 'camera'], // 可以指定来源是相册还是相机，默认二者都有
      success: function (res) {
        // 返回选定照片的本地文件路径列表，tempFilePath可以作为img标签的src属性显示图片
        var tempFilePaths = res.tempFilePaths
        that.setData({avatarUrl:tempFilePaths[0]})
      }
    })
  }
```

* `wx.chooseImage()` 中`success`方法必填，其他的可选。
* `tempFilePaths` 是图片的本地文件路径列表

## 文件的上传和下载

**使用到的API是**：`wx.uploadFile(OBJECT)`(将本地资源上传到开发者服务器);`wx.downloadFile(OBJECT)`(下载文件资源到本地)。

## 音乐的播放和控制

### 使用后台播放器播放音乐：`wx.playBackgroundAudio(OBJECT)`

在`onReady`中使用:
```js
onReady:function(){
    wx.playBackgroundAudio({
      dataUrl: 'http://ws.stream.qqmusic.qq.com/M500001VfvsJ21xFqb.mp3?guid=ffffffff82def4af4b12b3cd9337d5e7&uin=346897220&vkey=6292F51E1E384E06DCBDC9AB7C49FD713D632D313AC4858BACB8DDD29067D3C601481D36E62053BF8DFEAF74C0A5CCFADD6471160CAF3E6A&fromtag=46',
      success:function(res){
        //success
      },
      fail: function () {
        //fail
      },
      complete: function () {
        //complete
      }
    })
  }
```

### 暂停播放音乐：`wx.pauseBackgroundAudio()`

添加点击事件，点击后暂停音乐：
```js
bindViewTap: function() {
    wx.pauseBackgroundAudio({
      //暂停音乐
    })
}
```

### 获取后台音乐播放状态：`wx.getBackgroundAudioPlayerState(OBJECT)`

```js
bindViewTap: function() {
    wx.pauseBackgroundAudio({
      //暂停音乐
    })
    wx.getBackgroundAudioPlayerState({
      success:function(res){
        console.log(res)
      },
      fail:function(){
        //fail
      },
      complete:function(){
        //complete
      }
    })
  }
```

在控制台打印出的效果如图：
![音乐播放](http://ot4r4qnml.bkt.clouddn.com/music.png)

### 监听音乐播放：`wx.onBackgroundAudioPlay(CALLBACK)`

在`onLoad`中使用：
```js
wx.onBackgroundAudioPlay(function(){
    //用在：播放器写真旋转，当使用该API时，写真开始旋转
})
```

## 数据缓存

本地数据存储的大小限制为10MB

### 本地存储：`wx.setStorage(OBJECT)`

将数据存储在本地缓存中指定的`key`中，会覆盖掉原来该`key`对应的内容，这是一个**异步**接口。

在`onLoad`中使用：
```js
onLoad: function () {
    console.log('onLoad')
    wx.setStorage({
      key: 'testKey',
      data: 'testValue'
    })
  }
```
![存储](http://ot4r4qnml.bkt.clouddn.com/storage.png)

### 异步移除：`wx.removeStorage(OBJECT)`

从本地缓存中异步移除指定`key`。

```js
bindViewTap: function() {
    wx.removeStorage({ //移除
      key: 'testKey',
      data: 'testValue'
    })
}
```

### 拓展知识：同步和异步

* 进程同步：就是在发出一个功能调用时，在没有得到结果之前，该调用就不返回。也就是必须一件一件事做,等前一件做完了才能做下一件事。
* 进程异步：当一个异步过程调用发出后，调用者不能立刻得到结果。实际处理这个调用的部件在完成后，通过状态、通知和回调来通知调用者。
* 同步是阻塞模式，异步是非阻塞模式。
* 举个例子：普通B/S模式（同步）; AJAX技术（异步）
* 同步：提交请求->等待服务器处理->处理完毕返回。这个期间客户端浏览器不能干任何事
* 异步: 请求通过事件触发->服务器处理（这是浏览器仍然可以作其他事情）->处理完毕

## 获取当前位置(地图)

### 获取当前的地理位置：`wx.getLocation(OBJECT)`

在onLoad中使用：
```js
wx.getLocation({
    type: 'wgs84', //默认为wgs84返回gps坐标，gcj02返回可用于wx.openLocation的坐标
    success: function (res) {
      console.log(res) //可以在控制台看到返回的信息
    })
```

### 使用微信内置地图查看位置：`wx.openLocation(OBJECT)`

```js
wx.getLocation({
    type: 'gcj02',
    success: function (res) {
      var latitude = res.latitude
      var longitude = res.longitude
      wx.openLocation({
        latitude: latitude,
        longitude: longitude,
        scale: 28
      })
    }
})
```

![openLocation](http://ot4r4qnml.bkt.clouddn.com/map.png)

### 打开地图选择位置

```js
wx.getLocation({
    type: 'gcj02',
    success: function (res) {
        var latitude = res.latitude
        var longitude = res.longitude
        wx.chooseLocation({
            latitude: latitude,
            longitude: longitude,
            success: function(res){
                console.log(res) //输出当前位置信息：经纬度等
            }
        })
    }
})
```

![chooseLocation](http://ot4r4qnml.bkt.clouddn.com/chooseLocation.png)

## 设备

### 获取网络状态：`wx.getNetworkType(OBJECT)`

在`onLunch`中使用：
```js
wx.getNetworkType({
    success: function(res) {
      // 返回网络类型, 有效值：
      // wifi/2g/3g/4g/unknown(Android下不常见的网络类型)/none(无网络)
      var networkType = res.networkType
    }
})
```

### 获取系统消息：`wx.getSystemInfo(OBJECT)`

success回调参数：手机品牌、手机型号、设备像素比、屏幕宽度、屏幕高度等

```js
wx.getSystemInfo({
    success: function(res) {
      console.log(res)
    }
})
```

### 拨打电话：`wx.makePhoneCall(OBJECT)`

```js
wx.makePhoneCall({
  phoneNumber: '1340000' //仅为示例，并非真实的电话号码
})
```

## 动画

在`.wxml`中使用`animation`
```html
<view  bindtap="bindViewTap" class="userinfo">
    <image animation="{{animationData}}" class="userinfo-avatar" src="{{userInfo.avatarUrl}}"></image>
    <text class="userinfo-nickname">{{userInfo.nickName}}</text>
</view>
```

```js
Page({
  data: {
    userInfo: {},
    animationData: []
  },
  //事件处理函数
  bindViewTap: function() {
    var animation = wx.createAnimation({
      transformOrigin: '50% 50% 0',
      duration: 3000,
      timingFunction: "ease",
      delay: 0
    })

    animation.rotate(-180).scale(2).translate(50).step()

    this.setData({
      animationData: animation.export()
    })
}
```
