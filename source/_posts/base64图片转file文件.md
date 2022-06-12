---
title: base64图片转file文件
date: 2020-05-21 11:48:06
tags: js
---

#### 通过new File()将base64转换成file文件(存在兼容性问题)

```js
//将base64转换为文件
function dataURLtoFile(dataurl, filename) { 
		var arr = dataurl.split(','),
		mime = arr[0].match(/:(.*?);/)[1],
	  bstr = atob(arr[1]),
	  n = bstr.length,
	  u8arr = new Uint8Array(n);
		while (n--) {
			u8arr[n] = bstr.charCodeAt(n);
		}
		return new File([u8arr], filename, { type: mime });
}
//调用
var file = dataURLtoFile(base64Data, imgName);
```

#### 先将base64转换成blob，再将blob转换成file文件

```js
//将base64转换为blob
function dataURLtoBlob(dataurl) { 
	var arr = dataurl.split(','),
			mime = arr[0].match(/:(.*?);/)[1],
  		bstr = atob(arr[1]),
      n = bstr.length,
      u8arr = new Uint8Array(n);
  while (n--) {
      u8arr[n] = bstr.charCodeAt(n);
  }
     	return new Blob([u8arr], { type: mime });
}
//将blob转换为file
function blobToFile(theBlob, fileName){
	theBlob.lastModifiedDate = new Date();
  theBlob.name = fileName;
  return theBlob;
}
//调用
var blob = dataURLtoBlob(base64Data);
var file = blobToFile(blob, imgName);
```

