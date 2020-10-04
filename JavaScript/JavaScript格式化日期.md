# JavaScript格式化日期

~~~javascript
//JavaScript函数：
/**
 *格式化日期
 * @param value
 * @returns {string}
 */
function getStrDate(value){
    Date.prototype.Format = function (fmt) {
        let o = {
            "M+": this.getMonth() + 1, //月份
            "d+": this.getDate(), //日
            "h+": this.getHours(), //小时
            "m+": this.getMinutes(), //分
            "s+": this.getSeconds(), //秒
            "q+": Math.floor((this.getMonth() + 3) / 3), //季度
            "S": this.getMilliseconds() //毫秒
        };
        if (/(y+)/.test(fmt)) fmt = fmt.replace(RegExp.$1, (this.getFullYear() + "").substr(4 - RegExp.$1.length));
        for (let k in o)
            if (new RegExp("(" + k + ")").test(fmt)) fmt = fmt.replace(RegExp.$1, (RegExp.$1.length == 1) ? (o[k]) : (("00" + o[k]).substr(("" + o[k]).length)));
        return fmt;
    };
    console.log(value)
    return new Date(value).Format("yyyy-MM-dd hh:mm:ss");
}



/**
 * js函数代码：字符串转换为时间戳
 * @param dateStr
 * @returns {number}
 */
function getDateTimeStamp(dateStr){
    return Date.parse(dateStr.replace(/-/gi,"/"));
}




/**
 *1分钟以内显示为：刚刚
 *1小时以内显示为：N分钟前
 *当天以内显示为：今天 N点N分（如：今天 22:33）
 *昨天时间显示为：昨天 N点N分（如：昨天 10:15）
 *当年以内显示为：N月N日 N点N分（如：02月03日 09:33）
 *今年以前显示为：N年N月N日 N点N分（如：2000年09月18日 15:59）
 * @param timestamp
 * @returns {string}
 */
function timestampFormat(timestamp) {
    function zeroIze(num) {
        return (String(num).length === 1 ? '0' : '') + num;
    }

    let curTimestamp = new Date().getTime(); //当前时间戳
    let timestampDiff = curTimestamp - timestamp; // 参数时间戳与当前时间戳相差秒数

    let curDate = new Date(curTimestamp); // 当前时间日期对象
    let tmDate = new Date(timestamp);  // 参数时间戳转换成的日期对象

    let Y = tmDate.getFullYear(), m = tmDate.getMonth() + 1, d = tmDate.getDate();
    let H = tmDate.getHours(), i = tmDate.getMinutes(), s = tmDate.getSeconds();

    if (timestampDiff < 60) { // 一分钟以内
        return "刚刚";
    } else if(timestampDiff < 3600) { // 一小时前之内
        return Math.floor( timestampDiff / 60 ) + "分钟前";
    } else if (curDate.getFullYear() === Y && curDate.getMonth()+1 === m && curDate.getDate() === d ) {
        return '今天' + zeroIze(H) + ':' + zeroIze(i);
    } else {
        let newDate = new Date(curTimestamp - 86400000); // 参数中的时间戳加一天转换成的日期对象
        if ( newDate.getFullYear() === Y && newDate.getMonth() + 1 === m && newDate.getDate() === d ) {
            return '昨天' + zeroIze(H) + ':' + zeroIze(i);
        } else if ( curDate.getFullYear() === Y ) {
            return  zeroIze(m) + '月' + zeroIze(d) + '日 ' + zeroIze(H) + ':' + zeroIze(i);
        } else {
            return  Y + '年' + zeroIze(m) + '月' + zeroIze(d) + '日 ' + zeroIze(H) + ':' + zeroIze(i);
        }
    }
}





/**
 *
 * @param dateTimeStamp
 * @returns {string}
 */
function getDateDiff(dateTimeStamp){
    const minute = 1000 * 60;
    const hour = minute * 60;
    const day = hour * 24;
    const week = day * 7;
    const month = day * 30;
    const year = 12 * month;
    let now = new Date().getTime();
    let diffValue = now - dateTimeStamp;
    if(diffValue < 0){
        return '';
    }
    let yearC = diffValue / year;
    let monthC = diffValue / month;
    let weekC = diffValue / week;
    let dayC = diffValue / day;
    let hourC = diffValue / hour;
    let minC = diffValue / minute;
    let result;
    if(yearC >= 1){
        result = "发表于" + Math.floor(yearC) + "年前";
    } else if(monthC >= 1){
        result = "发表于" + Math.floor(monthC) + "个月前";
    } else if(weekC >= 1){
        result = "发表于" + Math.floor(weekC) + "周前";
    } else if(dayC >= 1){
        result = "发表于"+ Math.floor(dayC) +"天前";
    } else if(hourC >= 1){
        result = "发表于"+ Math.floor(hourC) +"个小时前";
    } else if(minC >= 1){
        result = "发表于"+ Math.floor(minC) +"分钟前";
    } else {
        result = "刚刚发表";
    }
    return result;
}

console.log(timestampFormat(getDateTimeStamp('2016-10-11 15:26:10')));
console.log(getDateDiff(getDateTimeStamp('2016-10-11 15:26:10')));
~~~

