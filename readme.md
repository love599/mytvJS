
供测试的JS订阅源：
https://raw.githubusercontent.com/mytv-android/mytvJS/refs/heads/main/js_github.m3u 

或   https://gitee.com/mytv-android/mytvJS/raw/main/js.gitee.m3u

凤凰卫视：javascript://https://gitee.com/mytv-android/mytvJS/raw/main/js/fh.js?type=cn&phone=凤凰秀手机号&pwd=凤凰秀密码 ，

type三种取值：cn 凤凰中文 info 凤凰资讯 hk 凤凰香港。 




支持QuickJS基础功能，集成了CryptoJS库（见下方模板）。

你需要实现main函数，paramstr为截断的 http://xxx/xxx/xxx.js?aaaa ?后的所有内容(aaaa)。

返回为json格式的字符串，url代表实际播放地址，headers为请求头。

# Template
```
function generateAuthorization(videoPath) {
    const AUTHORIZATION_TV_KEY = "key";
    const currentTimeMillis = Date.now();
    const md5Part = CryptoJS.MD5(AUTHORIZATION_TV_KEY + currentTimeMillis).toString();
    const uuidArray = [];
    for (let i = 0; i < 16; i++) {
        uuidArray.push(Math.floor(Math.random() * 256));
    }
    const uuidPart = uuidArray.map(b => ('0' + b.toString(16)).slice(-2)).join('').toUpperCase();
    const authorization = md5Part + currentTimeMillis + uuidPart;
    const sep = videoPath.includes('?') ? '&' : '?';
    return videoPath + sep + 'Authorization=' + authorization;
}

function main(paramstr) {
   const response = mytv.fetch("http://btv8kappvms.interway.com.cn");
   const data = JSON.parse(response);
   const videoPath = data.data.flv_url;
   const url = generateAuthorization(videoPath);
   return JSON.stringify({ url });
}
```

# 网络请求

## get

```
const headers = [
    'Content-Type: application/json'
];
const body = mytv.get(url, headers);
```

## post

```
const headers = [
    'Content-Type: application/json'
];
const post_body = JSON.stringify({
    code: prefix,
    keep_alive: false,
    password: pwd,
    phone: user
});
const body = mytv.post(url, headers, post_body);
```

## fetch
```
const body = mytv.fetch(url);
```

## 存储文件

```
// 判断文件存在
mytv.fileExists(fileName) -> Boolean

//读取文件
const contentStr = mytv.readFile(filename);

//写入文件
mytv.writeFile(filename, contentStr);

```

