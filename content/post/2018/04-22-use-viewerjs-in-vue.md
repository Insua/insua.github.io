---
title: 在vue中使用viewerjs
date: 2018-04-22 08:49:17
tags:
- vue
---

在 vue中使用viewerjs 必须执行 viewer.update
```
const viewer = new Viewer(this.$refs.image)

 this.$nextTick().then(() => {
    viewer.update()
 })
 viewer.view(0)
```

当然最后别忘了引入css
```scss
@import "~viewerjs/dist/viewer.css";
```
