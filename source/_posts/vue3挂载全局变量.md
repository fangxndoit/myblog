---
title: vue3挂载全局变量
date: 2022-09-05 16:11:46
tags:
---

#### 挂载

```js
main.js
//eg
import axios from 'axios'

const app = createApp(app)
//挂载
app.config.globalProperties.$http = axios
```

#### 获取全局变量

```js
const useGetGlobalProperties = function(){
  const { appContext: {app: { config: { globalProperties }}}} = getCurrentInstance() as ComponentInternalInstance; 
}
return { ...globalProperties };
```

