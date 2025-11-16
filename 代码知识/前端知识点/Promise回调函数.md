# Promise执行函数和回调

### 误区:

使用`Promise`的成功和失败回调函数后会直接返回，不执行后续方法体。

### 实际：

* **`resolve`和`reject`不会终止函数执行**，后续代码仍会运行。
* 若要避免`rej`或`res`后的代码执行，需**显式使用`return`**。

### 案例:

```js
const test = (num) => {
    return new Promise((res, rej) => {
        if (num > 10) {
            rej('数量大于10，不处理该数值')
        } else {
            res('已处理')
        }
        console.log(num)
    })

}

test(20).then((result) => {
    console.log(result)
}).catch((err) => {
    console.log(err)
});
```

### 结果:

```
20
数量大于10，不处理该数值
```
