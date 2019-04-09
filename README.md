# miniprogram-diff

小程序setData diff算法

## 目的
为了解决小程序内因setData执行频繁且数据传输量大而引发的页面渲染阻塞问题。

## 使用
可以对setData做一个新的封装，Promise化使用
```javascript
import diff from './src/diff';

// this指向的是Page对象
this.update = (data) => {
    return new Promise((resolve, reject) => {
        if (Object.prototype.toString.call(data) !== OBJECTTYPE) {
            reject('Error data type');
            return;
        }
        const result = diff(data, this.data);
        if (!Object.keys(result).length) {
            resolve(null);
            return;
        } 
        this.setData(result, () => {
            resolve(result);
        });
    });
}
```
封装后使用
```javascript
this.update({
    a: 1,
    b: {
        c: 2
    },
    d: [1, 2, 3]
}).then((res) => {
    // 渲染成功回调
    // do something
});
```