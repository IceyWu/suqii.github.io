---
title: "LivePhoto(实况图片)渲染 ✨"
description: "关于在LifePalette-Web实现实况渲染的探索"
uid: 102
ctime: 2024-11-06T06:27:19.911Z
mtime: 2024-11-06T06:27:19.911Z
---

:ArticleToc
:ArticleHeader

## 前言

最近使用 iPhone 拍摄了一些实况照片，发现了一个很有意思的功能，`LivePhoto`，它可以将照片变成一个小视频，如果把这个特性应用到[LifePalette-Web](https://github.com/Life-Palette/LifePalette-Web)项目中，会不会很有趣呢？

## 探索 `LivePhoto`

网上对于 vue 实现`LivePhoto`的文章很少，搜索相关资料发现苹果官方提供了一个`LivePhoto`的`js`库，[LivePhotoKit](https://developer.apple.com/documentation/livephotoskitjs)，它可以帮助我们实现`LivePhoto`的渲染，于是我就尝试使用这个库来实现`LivePhoto`的渲染

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <script src="https://cdn.apple-livephotoskit.com/lpk/1/livephotoskit.js"></script>
  </head>
  <body>
    <div
      data-live-photo
      data-photo-src="http://nest-js.oss-accelerate.aliyuncs.com/nestTest/1/1711965193523.JPEG"
      data-video-src="http://nest-js.oss-accelerate.aliyuncs.com/nestTest/1/1711965199299.MOV"
      style="width: 320px; height: 320px"
    ></div>
  </body>
</html>
```
![image.png](http://nest-js.oss-accelerate.aliyuncs.com/nestTest/1/1730876532790.png)

由官网 demo 发现实况图片其实是由两部分组成的，一部分是图片，一部分是视频，图片和视频是一一对应的，这里我们可以通过`data-photo-src`和`data-video-src`来指定图片和视频的地址，这样就可以实现实况图片的渲染

## 在Vue中使用`LivePhoto`

官方提供了 livephotoskit.js 库，我们可以 npm 安装这个库

```bash
pnpm install livephotoskit
```

实现

```vue
<script setup>
import * as LivePhotosKit from 'livephotoskit'

const livePhotoRef = ref()
const fileInfo = {
    src: 'http://nest-js.oss-accelerate.aliyuncs.com/nestTest/1/1711965193523.JPEG',
    videoSrc: 'http://nest-js.oss-accelerate.aliyuncs.com/nestTest/1/1711965199299.MOV',
}
async function initLivePhoto() {
    await nextTick()
    const player = LivePhotosKit.Player(livePhotoRef.value)
    player.photoSrc = fileInfo.src
    player.videoSrc = fileInfo.videoSrc
}
onMounted(() => {
    initLivePhoto()
})
</script>

<template>
 <div ref="livePhotoRef" class="w-100 h-100" />
</template>

<style lang="less" scoped></style>

```

## 总结

通过这次探索，我们成功的实现了`LivePhoto`的渲染，这个功能可以让我们的项目更加有趣，如果你也对这个功能感兴趣，可以尝试一下

项目地址：[LifePalette-Web](https://github.com/Life-Palette/LifePalette-Web)
预览地址：[LifePalette-Web](http://47.108.192.147:10086/#/)
