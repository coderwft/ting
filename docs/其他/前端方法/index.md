### 时间转换

```js
/**
 * @param {(Object|string|number)} times
 * @param {string} cFormat
 * @returns {string}
 */
function formateTime(times, cFormat) {
    if ((typeof times === 'number') && (times.toString().length === 10)) {
        times = times * 1000
    }
    var date = new Date(times)
    const format = cFormat || '{y}-{m}-{d} {h}:{i}:{s}'
    const formatObj = {
        y: date.getFullYear(),
        m: date.getMonth() + 1,
        d: date.getDate(),
        h: date.getHours(),
        i: date.getMinutes(),
        s: date.getSeconds(),
        a: date.getDay()
    }
    const res = format.replace(/{([ymdhisa])+}/g, (result, key) => {
        const value = formatObj[key]
        // Note: getDay() returns 0 on Sunday
        // if (key === 'a') {
        //     return ['日', '一', '二', '三', '四', '五', '六'][value]
        // }
        return value.toString().padStart(2, '0') //不足2位数前面补0
    })
    return res
}

/**
 * @param {(Object|string|number)} times
 * @returns {string}
 */
function formateTime2(times) {
    if ((typeof times === 'number') && (times.toString().length === 10)) {
        times = times * 1000
    }
    var date = new Date(times)
    var Y = date.getFullYear() + '-'
    var M = (date.getMonth() + 1 < 10 ? '0' + (date.getMonth() + 1) : date.getMonth() + 1) + '-'
    var D = (date.getDate() < 10 ? '0' + date.getDate() : date.getDate()) + ' '
    var h = (date.getHours() < 10 ? '0' + date.getHours() : date.getHours()) + ':'
    var m = (date.getMinutes() < 10 ? '0' + date.getMinutes() : date.getMinutes()) + ':'
    var s = (date.getSeconds() < 10 ? '0' + date.getSeconds() : date.getSeconds())

    var res = Y + M + D + h + m + s
    return res
}
```

