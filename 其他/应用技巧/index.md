### 前端excel的导入和导出

#### 导出

**`responseType`** 是一个枚举字符串值，用于指定`响应中包含的数据类型`(服务器响应的数据类型)

* ' '：值为空字符串的responseType与默认类型 text 相同，
* text： [`DOMString`](https://developer.mozilla.org/zh-CN/docs/Web/API/DOMString) 对象中的文本,

* json：通过将接收到的数据内容解析为 JSON 而创建的 JavaScript 对象。    //axios中，responseType默认值为json，

* blob：包含二进制数据的 [`Blob`](https://developer.mozilla.org/zh-CN/docs/Web/API/Blob) 对象。
* arrayBuffer：包含二进制数据的 JavaScript [`ArrayBuffer`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/ArrayBuffer)。

> `Blob` 对象表示一个不可变、原始数据的类文件对象。它的数据可以按文本或二进制的格式进行读取，也可以转换成 [`ReadableStream`](https://developer.mozilla.org/zh-CN/docs/Web/API/ReadableStream) 来用于数据操作。 
>
> Blob 表示的不一定是JavaScript原生格式的数据。[`File`](https://developer.mozilla.org/zh-CN/docs/Web/API/File) 接口基于`Blob`，继承了 blob 的功能并将其扩展使其支持用户系统上的文件。

```js
//导出excel   api
export function exportExcel(params) {
    return request({
        url: "/ledger/exportExcel",
        method: 'get',
        params,
        responseType: 'blob'
    })
}
```

调用

```js
	toExcel() {
      exportExcel(params).then((res) => {
        console.log("excel", res);
        this.downloadfile(res);//下载
      });
    },

    downloadfile(res) {
      const fileName = "机电设备.xlsx";
      if ("download" in document.createElement("a")) {
        // 非IE下载
        const blob = new Blob([res], { type: "application/ms-excel" });
        const elink = document.createElement("a");
        elink.download = fileName;
        elink.style.display = "none";
        elink.href = URL.createObjectURL(blob);
        document.body.appendChild(elink);
        elink.click();
        URL.revokeObjectURL(elink.href); // 释放URL 对象
        document.body.removeChild(elink);
      }
    },
```

#### 导入

一般是调用接口导入到数据库，再请求数据进行展示

### 前端二维码生成及打印

#### 生成

将responseType设置为blob

```js
//api
export function qrcode(code) {
    return request({
        url: '/common/qrcode/' + code,
        method: 'get',
        responseType: 'blob'
    })
}
//调用
generate() {
    qrcode(code).then((res) => {
        const blob = new Blob([res]);
        this.qrImg = window.URL.createObjectURL(blob);//生成图片src使用img元素展示
    });
},
```

#### 打印

根据后端返回的blob流，在线调取打印机，打印页面指定元素（区域）的内容，vue中可以使用插件  [vue-print-nb](https://www.npmjs.com/package/vue-print-nb)