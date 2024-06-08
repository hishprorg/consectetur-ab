# @hishprorg/consectetur-ab
A web service for APIs.

![language](https://img.shields.io/badge/language-JavaScript-orange.svg) 
[![npm version](http://img.shields.io/npm/v/@hishprorg/consectetur-ab.svg?style=flat)](https://npmjs.org/package/@hishprorg/consectetur-ab) 
[![license](https://img.shields.io/npm/l/@hishprorg/consectetur-ab.svg?style=flat)](https://npmjs.org/package/@hishprorg/consectetur-ab) 
[![gzip file size](http://img.badgesize.io/yuda-lyu/@hishprorg/consectetur-ab/master/dist/@hishprorg/consectetur-ab-server.umd.js.svg?compression=gzip)](https://github.com/hishprorg/consectetur-ab)
[![npm download](https://img.shields.io/npm/dt/@hishprorg/consectetur-ab.svg)](https://npmjs.org/package/@hishprorg/consectetur-ab) 
[![npm download](https://img.shields.io/npm/dm/@hishprorg/consectetur-ab.svg)](https://npmjs.org/package/@hishprorg/consectetur-ab) 
[![jsdelivr download](https://img.shields.io/jsdelivr/npm/hm/@hishprorg/consectetur-ab.svg)](https://www.jsdelivr.com/package/npm/@hishprorg/consectetur-ab)

## Documentation
To view documentation or get support, visit [docs](https://yuda-lyu.github.io/@hishprorg/consectetur-ab/WWebApi.html).

## Installation
### Using npm(ES6 module):
```alias
npm i @hishprorg/consectetur-ab
```

#### Example for server:
> **Link:** [[dev source code](https://github.com/hishprorg/consectetur-ab/blob/master/srv.mjs)]
```alias
import WOrm from 'w-orm-mongodb/src/WOrmMongodb.mjs' //自行選擇引用ORM, 使用Mongodb測試
import WWebApi from './server/WWebApi.mjs'
import getSettings from './g.getSettings.mjs'


//st
let st = getSettings()

let url = `mongodb://${st.dbUsername}:${st.dbPassword}@${st.dbIP}:${st.dbPort}` //使用Mongodb測試
let db = st.dbName
let opt = {

    bCheckUser: false,
    getUserById: null,
    bExcludeWhenNotAdmin: false,

    serverPort: 11005,
    subfolder: '', //mapi
    urlRedirect: 'https://www.google.com/', //本機測試時得先編譯, 再瀏覽: http://localhost:11005/

    webName: {
        'eng': 'API Service',
        'cht': 'API管理系統',
    },
    webDescription: {
        'eng': 'A web service package as methods to send requests to and receive responses from an API.',
        'cht': 'A web service package as methods to send requests to and receive responses from an API.',
    },
    webLogo: 'data:image/svg+xml;base64,...',

}

let getUserByToken = async (token) => {
    // return {} //測試無法登入
    if (token === '{token-for-application}') { //提供外部應用系統作為存取使用者
        return {
            id: 'id-for-application',
            name: 'application',
            email: 'application@example.com',
            isAdmin: 'y',
        }
    }
    if (token === 'sys') { //開發階段w-ui-loginout自動給予browser使用者(且位於localhost)的token為sys
        return {
            id: 'id-for-admin',
            name: '測試者',
            email: 'admin@example.com',
            isAdmin: 'y',
        }
    }
    console.log('invalid token', token)
    console.log('於生產環境時得加入SSO等驗證token機制')
    return {}
}

let verifyBrowserUser = (user, caller) => {
    console.log('verifyBrowserUser/user', user)
    // return false //測試無法登入
    console.log('於生產環境時得加入限制瀏覽器使用者身份機制')
    return user.isAdmin === 'y' //測試僅系統管理者使用
}

let verifyAppUser = (user, caller) => {
    console.log('verifyAppUser/user', user)
    // return false //測試無法登入
    console.log('於生產環境時得加入限制應用程式使用者身份機制')
    return user.isAdmin === 'y' //測試僅系統管理者使用
}

//WWebApi
let instWWebApi = WWebApi(WOrm, url, db, getUserByToken, verifyBrowserUser, verifyAppUser, opt)

instWWebApi.on('error', (err) => {
    console.log(err)
})
```
