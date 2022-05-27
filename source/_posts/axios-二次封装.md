---
title: axios 二次封装
top: false
cover: false
toc: true
mathjax: true
date: 2021-09-17 10:59:04
password:
summary:
tags: axios 
categories: 网络
---
axios库二次封装
1、新建utils文件夹，在文件夹里面新建reques.js

```
import axios from 'axios';
import { Message, Loading } from 'element-ui'
import qs from 'qs'; //参数序列化，把数据格式转为 x-www-form-urlencoded

let BASE_URL = '', loadingInstance;
let HOST = process.env.NODE_ENV;
switch (HOST) {
    case 'development':
        BASE_URL = 'http://localhost:888';
        break;
    case 'test':
        BASE_URL = 'http://192.168.101.21/foxcubeCrm/';
        break;
    default:
        BASE_URL = 'http://192.168.101.21/foxcubeCrm/';
}
axios.create({
    //crossDomain: true,//设置cross跨域
    withCredentials: false,  //跨域请求是否允许携带cookie资源凭证
    baseurl: BASE_URL,
    time: 1000               //请求超时时间
    // responseType:"arraybuffer"  返回的数据格式


})


//  request请求拦截器
axios.interceptors.request.use(config => {

    // let token=localstorage.getItem('token');
    // token && (config.headers.Authorization=token);//请求携带token
    // config.headers = {
    //     'Content-Type': 'application/x-www-form-urlencoded'  //转换数据格式
    // }
    loadingInstance = Loading.service({
        lock: true,
        text: '飞速加载中……',
    });
    return config;
}, error => {
    return Promise.reject(error);
})

// 成功状态 有3的接口一般是资源重定向
// service.defaults.validateStatus=status=>{
//     return /^(2|3)\d{2}$/.test(status);
// };


// response响应拦截器
axios.interceptors.response.use(response => {
    setTimeout(() => {
        loadingInstance.close();
    }, 300)
    return response;

}, error => {
    setTimeout(() => {
        loadingInstance.close();
    }, 300)
    let { response } = error;
    if (response) {
        //服务器有返回内容
        var errormsg = '';
        switch (response.status) {
            case 400:
                errormsg = '错误请求'
                break;
            case 401:
                errormsg = '未登录,请重新登录'
                break;
            case 403:
                errormsg = '决绝访问'
                break;
            case 404:
                errormsg = '请求错误，未找到该资源'
                break;
            case 405:
                errormsg = '请求方法未允许'
                break;
            case 408:
                errormsg = '请求超时'
                break;
            case 500:
                errormsg = '服务器出错'
                break;
            case 501:
                errormsg = '网络未实现'
                break;
            case 502:
                errormsg = '网络错误'
                break;
            case 503:
                errormsg = '服务不可用'
                break;
            case 504:
                errormsg = '网络超时'
                break;
            case 505:
                errormsg = 'http版本不支持该请求'
                break;
            default:
                errormsg = '连接错误'
        }
        Message({ type: 'warning', message: errormsg });
        return false;
    } else {
        //服务器连结果都没有返回  有可能是断网或者服务器奔溃
        if (!window.navigator.online) {
            //断网处理
            Message('网络中断');
            return;
        } else {
            //服务器奔了
            Message('服务器奔了');
            return Promise.reject(error);
        }
    }
})


/*
*get 方法，对应get请求
*@param {String} url [请求的url地址]
*@param {Object} params[请求携带的参数]]
*/


export function get(url, params) {
    return new Promise((resolve, reject) => {
        axios.get(url, {
            params: params
        }).then(res => {
            resolve(res.data);
        }).catch(err => {
            reject(err.data);
        })
    });
}



/*
*post 方法，对应post请求
*@param {String} url [请求的url地址]
*@param {Object} params[请求携带的参数]]
*/


export function post(url, params) {
    return new Promise((resolve, reject) => {
        axios.post(url, params)
            .then(res => {
                resolve(res.data);
            }).catch(err => {
                reject(err.data);
            })
    });
}


/*
*封装patch请求
 *@param url
 * @param params
 * @returns {Promise}
*/


export function patch(url, params) {
    return new Promise((resolve, reject) => {
        axios.patch(url, params)
            .then(res => {
                resolve(res.data);
            }, err => {
                reject(err);
            })
    })
}


/*
*put 请求
*@param url
* @param params
*/


export function put(url, params) {
    return new Promise((resolve, reject) => {
        axios.put(url, params)
            .then(res => {
                resolve(res.data);
            }, err => {
                reject(err);
            })
    })
}

2.新建api文件夹，在api文件夹里创建对应模块的js，写接口调用的方法

import {post,get,put,patch} from '@/utils/request'
export const login = params =>post('/api/login',params)

3.在login.vue直接引入该方法就可以直接使用

import { login } from "@/api/login";
login(data).then(res=>{

})

4.也可以在main.js引入直接到挂载到原型prototype上

import { post, get, put, patch } from './utils/request'
// 挂载到原型上
Vue.prototype.$post = post;
Vue.prototype.$get = get;

在需要使用请求数据的地方直接使用就ok
　　this.$post('/api/login',data).then(res=>{
         console.log(res)
   })
```
