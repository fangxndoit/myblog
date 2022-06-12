---
title: 捕获async/await异常
date: 2020-09-21 17:16:26
tags: js
---

```js
//to.js
export default function to(promise){
  return promise.then(data => {
    return [null,data]
  })
  .catch(err => [err])
}
```

#####工具函数

#####使用方法

```js
import to from './to.js';

async function asyncTask() {
     let err, user, savedTask;

     [err, user] = await to(UserModel.findById(1));
     if(!user) throw new CustomerError('No user found');

     [err, savedTask] = await to(TaskModel({userId: user.id, name: 'Demo Task'}));
     if(err) throw new CustomError('Error occurred while saving task');

    if(user.notificationsEnabled) {
       const [err] = await to(NotificationService.sendNotification(user.id, 'Task Created'));  
       if (err) console.error('Just log the error and continue flow');
    }
}
```

