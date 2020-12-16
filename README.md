# class_mini_program

一个使用Taro构建的课堂助手小程序

## 技术栈

Taro 2.0 + Taro UI + Redux

## Taro.requst的封装
```
import Taro from '@tarojs/taro';
import {showLoading} from "./common";

const baseUrl = 'XXXX';

export default (method, url, payload)=> {
  const token = Taro.getStorageSync('token');
  return Taro.request({
    url: baseUrl + url,
    data: JSON.stringify(payload),
    header: {
      'Content-Type': 'application/json',
      'token': token || '',
    },
    method: method.toUpperCase(),
    timeout: 10000,
    fail: (res)=>{
      showLoading(false);
      Taro.showToast({
        title: `${res.errMsg}`,
        icon: 'none',
      });
    }
  }).then(res => {
    const {statusCode, data} = res;
    if (statusCode >= 200 && statusCode < 300) {
      data.code !== '0' ?
        Taro.showToast({
          title: '成功！',
          icon: 'success',
        }) :
        Taro.showToast({
          title: data.Exception,
          icon: 'none',
        });
      return data;
    } else {
      showLoading(false);
      throw new Error(`网络请求错误，状态码${statusCode}`);
      return true;
    }
  })
};
```
