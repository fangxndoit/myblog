---
title: axios
date: 2022-08-04 12:28:52
tags:
---

```js
import axios from 'axios';

const instance = axios.create({
  baseURL: '',
  timeout: 1000
})

export const get = (url, params = {})  => {
  return new Promise((resove, reject)  => {
    instance.get(url, { params }).then(response => {
			resolve(response.data)
    }, err => {
      reject(err)
    })
  })
}

export const post = (url, data = {}) => {
	return new Promise((resolve, reject) => {
    instance.post(url, data, {
      header: {
        'Content-Type': 'application/json'
      }
    }).then(response => {
			resolve(response.data)
    }, err => {
      reject(err)
    })
  })
}

```

