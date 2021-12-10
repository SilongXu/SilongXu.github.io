---
title: axios封装http请求
abbrlink: 'axiosHTTP'
categories: 前端
tags: [vue, 前端, axios, http]
---
# vue使用axios封装http请求

## 安装axios

npm命令：

**npm install axios --save**



找一个合适的目录位置 建立一个http.js

在其中导入axios

```javascript
import axios from 'axios';
```

## 配置axios

1.创建一个axios对象，取名http，并且在内部进行axios配置:

```
const http = axios.create({
  baseURL: BASE_URL, //请求base url ，一般由前后端商量接口确定base url，将自动加在url前面,除非url是个绝对url
  url: ,//用于请求的服务器url，只有这一项是必需的
  method: ,//创建请求时使用的方法，默认是get
  headers: {
    'Content-Type': 'application/json;charset=UTF-8',//设置请求时的header信息
  },
  timeout: ,//设置请求超时时间
  withCredentials: ,//如果需要携带cookie的时候这一项为true
});
```

**详细参数配置查看这个网页**：[axios配置](https://blog.csdn.net/moxiaoya1314/article/details/73650751) 

2.然后配置拦截器用来拦截request请求和response应答：

```
http.interceptors.request.use((data) => {
  return data;
});

http.interceptors.response.use((response) => {
  return response.data;
}, (error) => {
  return Promise.reject(error);
});
```

3.接下来在需要的模块里创建一个module.service.js文件，用来配置http的get ，post，以及其他的请求，举例：

```
import http from '../shared/services/http'

const prefix = 'catalog';
const MenuService = {
  getMenuNodeByParentId(parentId) {
    return http.get(`/${prefix}/system/directory/${parentId}`);
  },
  createMenuNode(catalogId, metaEnumFieldDTO, nodeCode) {
    return http.put(`/${prefix}/system/directory`, { catalogId, metaEnumFieldDTO, nodeCode });
  },
  deleteMenuNode(ids) {
    return http.post(`/${prefix}/system/directory/delete`, ids);
  },
  getMetaField(id) {
    return http.get(`/${prefix}/system/directory/metafield/${id}`);
  },
  enumfield(fieldCode, id) {
    return http.get(`/${prefix}/system/directory/enumfield/${id}/${fieldCode}`);
  },
};
export default MenuService;
```

4.最后在需要接口的地方引用即可，举例：

```
    fetchMenuSelectData(classValue) {
      if(this.node) {
        apiService.enumfield(classValue, this.node.id)
        .then((selects) => {
          this.menuSelect = selects;
          this.menuSelect.items.forEach(item => {
            this.checkList.push(item.itemCode);
          });
        }).catch(() => {});
      }
    },
```

