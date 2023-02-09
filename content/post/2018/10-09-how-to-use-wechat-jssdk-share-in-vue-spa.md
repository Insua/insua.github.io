---
title: 如何在 Vue Spa 中使用微信jssdk分享接口
date: 2018-10-09 11:44:54
tags: 
- vue
- wechat
---
在Vue Spa项目中，使用了 History 模式，要使用分享接口，只能在第一次访问的时候，就加载jssdk配置，通过Vue router进入其他页面之后再做加载，虽然在调试模式下依然会显示配置正确，但是分享接口是无效的，那么怎么办呢？直接在App.vue下就做jssdk config   
### 实现步骤  

#### 编写后端接口
使用了 overture/wechat
```
public function jssdk(Request $request)
    {
        $this->jssdk->setUrl($request->input('url'));
        return $this->jssdk->buildConfig([
            'onMenuShareAppMessage',
            'onMenuShareWechat',
            'onMenuShareTimeline',
            'getLocation'
        ], false);
    }
```

#### 在App.vue中注入配置
引入wexin-js-sdk
```
const wx = require('weixin-js-sdk')
```
方法
```
async getJssdk () {
      const { data } = await this.$axios.post('/api/wechat-work/jssdk', { url: window.location.href })
      wx.config(data)
    }
```

#### 在分享页面中编写分享方法
引入wexin-js-sdk
```
const wx = require('weixin-js-sdk')
```
在__mounted__里写入分享方法
```
wx.ready(() => {
      const self = this
      wx.onMenuShareTimeline({
        title: self.shareTitle,
        link: window.location.href,
        imgUrl: self.logoPath,
        success: function () {
          self.forward('onMenuShareTimeline')
          self.$toast.success('分享成功')
        },
        cancel: function () {
          self.$toast.fail('取消分享')
        }
      })
   })
```
最后在__destroyed__里重写分享方法，以终止分享接口
```ecmascript 6
wx.ready(() => {
      wx.onMenuShareTimeline({
        success: function () {},
        cancel: function () {}
      )
    })
```
