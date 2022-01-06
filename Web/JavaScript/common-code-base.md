# JavaScript | 常用代码


## 数学
### 随机数

```jsx
Math.random() //返回[0,1)的随机数
Math.floor(Math.random() * k)  //返回[0,k-1]之间的随机整数

/**
 * 
 * @param {随机数下界(包括} low 
 * @param {随机数上界(包括)} high 
 * @returns 区间[low,high]之间的随机整数
 */
function myRandom(low, high) {
    return Math.floor(Math.random() * (high - low + 1)) + low;
}
```


## 日期

### 检测给出日期是否有效
```javascript
const isDateValid = (...val) => !Number.isNaN(new Date(...val).valueOf());

isDateValid("December 17, 1995 03:24:00");  // true
```

### 计算两日期间间隔
```javascript
const dayDif = (date1, date2) => Math.ceil(Math.abs(date1.getTime() - date2.getTime()) / 86400000)
    
dayDif(new Date("2021-11-3"), new Date("2022-2-1"))  // 90
```

### 查找日期位于一年中的第几天
```javascript
const dayOfYear = (date) => Math.floor((date - new Date(date.getFullYear(), 0, 0)) / 1000 / 60 / 60 / 24);

dayOfYear(new Date());   // 307
```

### 时间格式化为 hh:mm:ss
```javascript
const timeFromDate = date => date.toTimeString().slice(0, 8);
    
timeFromDate(new Date(2021, 11, 2, 12, 30, 0));  // 12:30:00
timeFromDate(new Date());  // 返回当前时间 09:00:00
```

