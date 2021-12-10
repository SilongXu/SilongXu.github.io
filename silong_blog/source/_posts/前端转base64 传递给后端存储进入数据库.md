---
title: 前端图片转base64然后传递给后端
abbrlink: 'turnImgTobase64'
categories: 前端
tags: [vue, 前端, base64]
---
# 前端转base64 传递给后端存储进入数据库

大家应该都知道前端可以通过接收后端图片的url，然后从云端拿到图片。 

但是如果只是一个本地开发的产品，没有云端的情况该怎么办呢。

对于小一些的图片（1M左右），我们可以将它转为base64格式，这样的话传递给后端就是一个字符串，方便后端存储进本地的数据库。

那么接下来就来看看如何做吧。

启蒙网站！： [base64](https://www.cnblogs.com/maggieq8324/p/11577697.html) 

```javascript
<el-upload
         class="avatar-uploader"
         action=""
         ref="uploadAvatar"
         :show-file-list="false"
         :auto-upload="false"
         :on-change="changeFile">
     <img v-if="imageUrl" :src="imageUrl" class="uploadAvatar">
     <i v-else class="el-icon-plus avatar-uploader-icon"></i>
 </el-upload>

data() {
  return {
	imageUrl: '',
	imageBaseUrl: '',
  }
}

 /**
 * 文件框改变事件
 * @param file
 * @param fileList
 */
changeFile(file, fileList) {
    const isJPGORPNG = (file.raw.type === 'image/jpeg' || file.raw.type === 'image/png');
    const isLt1M = file.size / 1024 / 1024 < 1;

    if (!isJPGORPNG) {
        this.$message.info('上传头像图片只能是 JPG 或 PNG 格式!');
        return;
    }
    if (!isLt1M) {
        this.$message.info('上传头像图片大小不能超过 1MB!');
        return;
    }

    var This = this;
    var reader = new FileReader();
    reader.readAsDataURL(file.raw);
    reader.onload = function(e){
        this.result; //base64编码
        This.imageBaseUrl = this.result;
        This.imageUrl = this.result;
    }
},
```



this.result的结果就是base64 编码过后的字符串了！



既然图片转为了base64编码 那么我们该如何base64这个字符串显示出来呢。

其实直接将base64字符串赋值给 :src 就可以了

附上一个分离base64编码的方法：

```javascript
base64ImgtoFile(dataurl, filename = 'file') {
  let arr = dataurl.split(',')
  let mime = arr[0].match(/:(.*?);/)[1]
  let suffix = mime.split('/')[1]
  let bstr = atob(arr[1])
  let n = bstr.length
  let u8arr = new Uint8Array(n)
  while (n--) {
    u8arr[n] = bstr.charCodeAt(n)
  }
  return new File([u8arr], `${filename}.${suffix}`, {
    type: mime
  })
}，

// base64编码的图片
var base64Img = 'data:image/png;base64,XXXXX...';
//转换图片文件
var imgFile = this.base64ImgtoFile(base64Img); 
```

