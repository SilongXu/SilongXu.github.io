---
title: formData实现多文件上传
abbrlink: 'formDataUpload'
categories: 前端
tags: [前端, formData, 上传, upload]
---
# formData实现单/多文件上传

文件上传阿龙踩坑了，也说不上是多难的东西，但是很重要。

先去官网了解一下FormData是什么以及有关FormData的API：[戳这里](https://developer.mozilla.org/zh-CN/docs/Web/API/FormData)

## FormData的使用方法

### 创建对象

```javascript
const formData = new FormData();
```

### 向FormData中添加数据

更多使用append方法，因为append方法会在有重复值的时候不覆盖原有文件。

```
formData.append(name, value);
formData.append(name, value, filename);

//name:value中包含的数据对应的表单名称
//表单的值。可以是USVString或Blob(很重要！！)
//可选。传给服务器的文件名称 (一个 USVString), 当一个 Blob 或 File 被作为第二个参数的时候， Blob 对象的默认文件名是 "blob"。 File 对象的默认文件名是该文件的名称。
```

### 查看FormData中的数据

想在控制台上查看formData中的数据，`console.log(formData)`是不行的，你只会得到空的一个数组。正确方法是：`console.log(formData.get('name'))` 或者 `console.log(formData.getAll('name'))`，因为`formData`比较特殊，需要调用`get`和`getAll`方法才能得到对应的数据

### 通过el-upload获得上传的文件

el-upload是element-ui封装的上传组件，通过on-change和on-rmove的调用可以得到添加的file和fileList ：

```
<el-form-item label="业务数据" :required="true">
  <el-upload
    action=""
    :auto-upload="false"
    :on-change="onFileChange"
    :on-remove="onFileChange"
    multiple>
    <el-button plain size="small" type="primary">选择文件</el-button>
  </el-upload>
</el-form-item>
```

然后调用onFileChange 赋值给在data中准备好的数组

```
    onFileChange(file, fileList) {
      apiService.getImportPath()
      .then((path) => {
        this.importPath = path;
        this.dir.dirBasePath = path;
        this.dir.dir = fileList;
      }).catch(() => {
      });
    }
```

最后通过append将后端需要的东西通过接口传向后端(需要和后端商量，有时候后端不仅要上传的文件，还可能要文件类型或者一些附加的信息)。

重难点：上传的文件必须是file类型，也就是说如果你append进`formData`中的文件不是file类型，文件就会变成[object object]。

可以通过`console.log`的方法查看在el-upload中的`fileList`，找到里面的file，你会看到这个file本身其实是一个object，并不是file。

那我们要怎么办呢？

仔细观察可以看到，file里面有一个raw属性，它的值是File，我们只需要将file改成`file.raw`即可：

```
importIntData(){
      const formData = new FormData();
      this.dir.dir.forEach(file => {
        formData.append('dir', file.raw);
      })
      formData.append('dirBasePath', this.dir.dirBasePath);
      formData.append('updateDataType', this.dir.updateDataType);
      apiService.importIntData(formData)
      .then((data) => {
        this.importStatus=data.result;
      }).catch(() =>{
      });
    },
```

这时候不知道会不会有人有这样的疑问：你的value只是file的一个raw属性，那是不是说你通过接口传给后端的文件里面只有file的raw属性呢，而不是整个file呢

（反正博主是有这个疑问的）

先上图：![](C:\Users\86139\Desktop\md文档\image\fileRaw.png)

里面的File其实就包含了整个文件的信息，所以只传File就可以了！