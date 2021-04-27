---
title: 'share and save'
date: 2020-10-11 16:53:04
tags:
- 分享
- 保存图片
categories:
- uniapp
---

&ensp;&ensp;&ensp;&ensp;使用uniapp制作微信小程序、或app（记载回顾）
* 海报（后台生成）分享
* 保存图片到本地
<!--more-->

### 分享
#### 自带分享
&ensp;&ensp;&ensp;&ensp;uniapp自带有分享功能（[uniapp官网分享](https://www.cnblogs.com/xuxiaoxia/p/8405076.html)），官网也比较简洁明了。
#### 手写分享
&ensp;&ensp;&ensp;&ensp;用户自定义生成海报分享，上代码（请求接口返回生成海报图）。此处后台需要接收图片才能返回生成的海报图。
- 接收base64
图片路径转base64，网上查找的方法，整合。
先把图片路径转为arraybuffer格式（二进制），再转为base64格式
```
    async toShare(){
        uni.showLoading({
            title: "加载中"
        })
        uni.request({
    　　　　url: imgUrl, //图片路径
    　　　　method: 'GET',
    　　　　responseType: 'arraybuffer',
    　　　　success: async res => {
    　　　　　　let base64 = wx.arrayBufferToBase64(res.data); //把arraybuffer转成base64
    　　　　　　let toBase64Url = 'data:image/jpeg;base64,' + base64; //不加上这串字符，在页面无法显示
                let data = {
                    img: toBase64Url
                }
                let haibao = await this.$api.imgShare(data)
                // 后台返回base64格式过长，存在换行、空格等
                // 接收海报图
                this.ewmImg = haibao.data.img.replace(/[\r\n]/g, "")
                uni.hideLoading()
    　　　　},
            fail:err=>{
                uni.hideLoading();
                this.$utils.toast('该内容不支持被分享');
            }
    　　});
    },
```
- 接收图片路径
这就简单了，拿到图片路径直接传给后台，接收生成海报就可以了。

### 保存图片至本地
&ensp;&ensp;&ensp;&ensp;图片保存uniapp上也有，这里做一个整合。
&ensp;&ensp;&ensp;&ensp;思路：就两步，首先判断是否授权，再就是保存。
```
    saveEwm() {
        let _self = this;
        //获取相册授权
        uni.getSetting({
            success(res) {
                if (!res.authSetting['scope.writePhotosAlbum']) {
                    uni.authorize({
                        scope: 'scope.writePhotosAlbum',
                        success() {
                            //这里是用户同意授权后的回调
                            _self.saveImgToLocal();
                        },
                        fail() { 
                            //这里是用户拒绝授权后的回调
                        }
                    })
                } else { //用户已经授权过了
                    _self.saveImgToLocal();
                }
            }
        })
    },
    saveImgToLocal(){
        // 指定图片的临时路径
        const path = `${wx.env.USER_DATA_PATH}/share.png`;
        // 获取小程序的文件系统
        const fsm = uni.getFileSystemManager();
        // 把arraybuffer数据写入到临时目录中
        fsm.writeFile({
            filePath: path,
            data: this.ewmImg.replace(/^data:image\/\w+;base64,/, ""),
            encoding: 'base64',
            // encoding: 'binary',
            success: (res)=>{
                // 把临时路径下的图片，保存至相册
                uni.saveImageToPhotosAlbum({
                filePath: path,
                success: res => {
                    uni.showToast({title: '保存成功'});
                },
                fail: err => {uni.showToast({title: err})}
                });
            },
        })
    },

```



