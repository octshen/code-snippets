
## 一些常用的js 代码片段
```
// 对于给定字符串中的每个字母，为字母创建字谜
const anagrams = str => {
  if (str.length <= 2) return str.length === 2 ? [str, str[1] + str[0]] : [str];
  return str.split('').reduce((acc, letter, i) =>
    acc.concat(anagrams(str.slice(0, i) + str.slice(i + 1)).map(val => letter + val)), []);
};
// anagrams('abc') -> ['abc','acb','bac','bca','cab','cba']

// 数组平均数
const average = arr => arr.reduce((acc, val) => acc + val, 0) / arr.length;
// average([1,2,3]) -> 2


// 大写每个单词的首字母  \b匹配边界
const capitalizeEveryWord = str => str.replace(/\b[a-z]/g, char => char.toUpperCase());
// capitalizeEveryWord('hello world!') -> 'Hello World!'

// 首字母大写
const capitalize = (str, lowerRest = false) =>
  str.slice(0, 1).toUpperCase() + (lowerRest ? str.slice(1).toLowerCase() : str.slice(1));
// capitalize('myName', true) -> 'Myname'

// 检查回文
const palindrome = str => {
  const s = str.toLowerCase().replace(/[\W_]/g, '');
  return s === s.split('').reverse().join('');
}
// palindrome('taco_ cat')-> true

// 计数数组中值的出现次数
const countOccurrences = (arr, value) => arr.reduce((a, v) => v === value ? a + 1 : a + 0, 0);
// countOccurrences([1,1,2,1,2,3], 1) -> 3

// Curry使用递归。如果提供的参数（args）数量足够，则调用传递函数f，否则返回一个curried函数f。
const curry = (fn, arity = fn.length, ...args) => {
  return arity <= args.length ?
    fn(...args) :
    curry.bind(null, fn, arity, ...args);
}

// curry(Math.pow)(2)(10) -> 1024
// curry(Math.min, 3)(10)(50)(2) -> 2

// 数组拍平
const deepFlatten = arr =>
  arr.reduce((a, v) => a.concat(Array.isArray(v) ? deepFlatten(v) : v), []);
// deepFlatten([1,[2],[[3],4],5]) -> [1,2,3,4,5]


// 数组之间的不同
const difference = (a, b) => {
  const s = new Set(b);
  return a.filter(x => !s.has(x));
};
// difference([1,2,3], [1,2]) -> [3]


//使用Math.hypot（）计算两点之间的欧几里德距离。
const distance = (x0, y0, x1, y1) => Math.hypot(x1 - x0, y1 - y0);
// distance(1,1, 2,3) -> 2.23606797749979


// 转义正则表达式使用replace（）来转义特殊字符。
const escapeRegExp = str => str.replace(/[.*+?^${}()|[\]\\]/g, '\\$&');
// escapeRegExp('(test)') -> \\(test\\)

//阶乘
// const factorial = n => n <= 1 ? 1 : n * factorial(n - 1);
// 尾调优化
const factorial = (n, total) => {
  if (n === 1) return total;
  return factorial(n - 1, n * total);
}
// factorial(5, 1) // 120
// factorial(6) -> 720

//斐波那契数组生成器
const fibonacci = n =>
  Array(n).fill(0).reduce((acc, val, i) => acc.concat(i > 1 ? acc[i - 1] + acc[i - 2] : i), []);
// fibonacci(5) -> [0,1,1,2,3]

//过滤数组中的非唯一值
const filterNonUnique = arr => arr.filter(i => arr.indexOf(i) === arr.lastIndexOf(i));
// filterNonUnique([1,2,2,3,4,4,5]) -> [1,3,5]

// 数组去重
const dedup = arr => arr.filter((val, ind, arr) => ind === arr.indexOf(val))
// dedup([1,2,3,2,3])  //[1,2,3]
const unique = arr => [...new Set(arr)];
// unique([1,2,2,3,4,4,5]) -> [1,2,3,4,5]


// 最大公约数（GCD）
const gcd = (x, y) => !y ? x : gcd(y, x % y);
// gcd (8, 36) -> 4

// 用值初始化数组
const initializeArray = (n, value = 0) => Array(n).fill(value);
// initializeArray(5, 2) -> [2,2,2,2,2]

// 测试功能所花费的时间
const timeTaken = callback => {
  console.time('timeTaken');
  const r = callback();
  console.timeEnd('timeTaken');
  return r;
};
// timeTaken(() => Math.pow(2, 10)) -> 1024
// (logged): timeTaken: 0.02099609375ms


// 来自键值对的对象使用Array.reduce（）来创建和组合键值对。
const objectFromPairs = arr => arr.reduce((a, v) => (a[v[0]] = v[1], a), {});
// objectFromPairs([['a',1],['b',2]]) -> {a: 1, b: 2}

// 使用Array.reduce（）通过函数传递值。
const pipe = (...funcs) => arg => funcs.reduce((acc, func) => func(acc), arg);
// pipe(btoa, x => x.toUpperCase())("Test") -> "VGVZDA=="

// Powerset 使用reduce（）与map（）结合来遍历元素，并将其组合成包含所有组合的数组。
const powerset = arr =>
  arr.reduce((a, v) => a.concat(a.map(r => [v].concat(r))), [
    []
  ]);
// powerset([1,2]) -> [[], [1], [2], [2,1]]

// 范围内的随机整数
const randomIntegerInRange = (min, max) => Math.floor(Math.random() * (max - min + 1)) + min;
// randomIntegerInRange(0, 5) -> 2

//范围内的随机数
const randomInRange = (min, max) => Math.random() * (max - min) + min;
// randomInRange(2,10) -> 6.0211363285087005

// 随机化数组的顺序
const shuffle = arr => arr.sort(() => Math.random() - 0.5);
// shuffle([1,2,3]) -> [2,3,1]


// 重定向到URL使用window.location.href或window.location.replace（）重定向到url。 传递第二个参数来模拟链接点击（true - default）或HTTP重定向（false）。
const redirect = (url, asLink = true) =>
  asLink ? window.location.href = url : window.location.replace(url);
// redirect('https://google.com')

// 反转一个字符串
const reverseString = str => [...str].reverse().join('');
// reverseString('foobar') -> 'raboof'


// 数组之间的相似性使用filter（）移除不是values的一部分值，使用includes（）确定。
const similarity = (arr, values) => arr.filter(v => values.includes(v));
// similarity([1,2,3], [1,2,4]) -> [1,2]

// 数组总和
const sum = arr => arr.reduce((acc, val) => acc + val, 0);
// sum([1,2,3,4]) -> 10

// URL参数
const getUrlParameters = url =>
  url.match(/([^?=&]+)(=([^&]*))/g).reduce(
    (a, v) => (a[v.slice(0, v.indexOf('='))] = v.slice(v.indexOf('=') + 1), a), {}
  );
// getUrlParameters('http://url.com/page?name=Adam&surname=Smith') -> {name: 'Adam', surname: 'Smith'}

// 月(M)、日(d)、12小时(h)、24小时(H)、分(m)、秒(s)、周(E)、季度(q)
// 可以用 1-2 个占位符 * 年(y)可以用 1-4 个占位符，毫秒(S)只能用 1 个占位符(是 1-3 位的数字) * eg: * (new
// Date()).pattern("yyyy-MM-dd hh:mm:ss.S")==> 2006-07-02 08:09:04.423     
// * (new Date()).pattern("yyyy-MM-dd E HH:mm:ss") ==> 2009-03-10 二 20:09:04     
// * (new Date()).pattern("yyyy-MM-dd EE hh:mm:ss") ==> 2009-03-10 周二 08:09:04     
// * (new Date()).pattern("yyyy-MM-dd EEE hh:mm:ss") ==> 2009-03-10 星期二 08:09:04     
// * (new Date()).pattern("yyyy-M-d h:m:s.S") ==> 2006-7-2 8:9:4.18     
const formatDate = function (date, fmt) {
  let o = {
    "M+": date.getMonth() + 1, //月份         
    "d+": date.getDate(), //日         
    "h+": date.getHours() % 12 == 0 ? 12 : date.getHours() % 12, //小时         
    "H+": date.getHours(), //小时         
    "m+": date.getMinutes(), //分         
    "s+": date.getSeconds(), //秒         
    "q+": Math.floor((date.getMonth() + 3) / 3), //季度         
    "S": date.getMilliseconds() //毫秒         
  };
  let week = {
    "0": "\u65e5",
    "1": "\u4e00",
    "2": "\u4e8c",
    "3": "\u4e09",
    "4": "\u56db",
    "5": "\u4e94",
    "6": "\u516d"
  };
  if (/(y+)/.test(fmt)) {
    fmt = fmt.replace(RegExp.$1, (date.getFullYear() + "").substr(4 - RegExp.$1.length));
  }
  if (/(E+)/.test(fmt)) {
    fmt = fmt.replace(RegExp.$1, ((RegExp.$1.length > 1) ? (RegExp.$1.length > 2 ? "\u661f\u671f" : "\u5468") : "") + week[date.getDay() + ""]);
  }
  for (let k in o) {
    if (new RegExp("(" + k + ")").test(fmt)) {
      fmt = fmt.replace(RegExp.$1, (RegExp.$1.length == 1) ? (o[k]) : (("00" + o[k]).substr(("" + o[k]).length)));
    }
  }
  return fmt;
}
// var date = new Date();
// console.log(formatDate(date, "yyyy-MM-dd EE hh:mm:ss"));


// 通过事件dispatch监听localStorage变化
const orignalSetItem = localStorage.setItem;
localStorage.setItem = function (key, value) {
  var setItemEvent = new Event("setItemEvent");
  setItemEvent.value = value;
  setItemEvent.key = key;
  window.dispatchEvent(setItemEvent);
  orignalSetItem.apply(this, arguments);
}
window.addEventListener("setItemEvent", function (e) {
  if (e.key == 'custom_key') {
    console.log(e.value)
  }
})

// A是起始位置；
// B是目标位置；
// rate是缓动速率；
// callback是变化的位置回调，支持两个参数，value和isEnding，表示当前的位置值（数值）以及是否动画结束了（布尔值）；
Math.easeout = function (A, B, rate, callback) {
  if (A == B || typeof A != 'number') {
    return
  }
  B = B || 0;
  rate = rate || 2

  var step = function () {
    A = A + (B - A) / rate
    if (A < 1) {
      callback(B, true)
      return
    }
    callback(A, false)
    requestAnimationFrame(step)
  }
  step()
};

// 千位分隔符
const formatNum = function (num) {
  let arr = num.toString().split('.')
  let reg = /\d{1,3}(?=(\d{3})+$)/g
  arr[0] = arr[0].replace(reg, '$&,')
  return arr.join('.')
}


// 判断浏览器类型
export const judgeBrowser = (getPlatform = false) => {
  const UA = navigator.userAgent
  // 只针对移动端浏览器，不考虑桌面端
  // safari 的话会有 qqbrowser, chrome, firefox 也包含进去，有可能还有许多其他外国浏览器和国产浏览器。。。
  // many lowbi browsers even copy safari-version
  const safariVersion = UA.match(/Version\/\d{1,2}\.\d/) ? UA.match(/Version\/\d{1,2}\.\d/)[0].replace('Version/', '') : 0
  let platform
  let browser
  const result = []
  if (/Android/i.test(UA)) {
    platform = 'android'
  }
  if (/iPhone|iPad|iPod/i.test(UA)) {
    platform = 'ios'
  }
  if (/MicroMessenger/i.test(UA)) {
    browser = 'weixin'
  } else if (/FxiOS/i.test(UA)) {
    browser = 'firefox'
  } else if (/VivoBrowser/i.test(UA)) {
    browser = 'vivo'
  } else if (/OppoBrowser/i.test(UA)) {
    browser = 'oppo'
  } else if (/(iPad|iPhone|iPod).*? (IPad)?QQ\/([\d]+)/.test(UA) || /\bV1_AND_SQI?_([\d]+)(.*? QQ\/([\d]+))?/.test(UA)) {
    browser = 'qq'
  } else if (/MQQBrowser/i.test(UA)) {
    browser = 'qqbrowser'
  } else if (/UCBrowser/i.test(UA)) {
    browser = 'uc'
  } else if (/Baidu/i.test(UA)) {
    browser = 'baidu'
  } else if (/Sogou/i.test(UA)) {
    browser = 'sogou'
  } else if (/QihooBrowser/i.test(UA)) {
    browser = 'qihoo'
  } else if (/FingerBrowser/i.test(UA)) {
    browser = 'finger'
  } else if (/SAMSUNG/i.test(UA)) {
    // before chrome， 流氓的三星
    browser = 'samsung'
  } else if (/CriOS/i.test(UA)) {
    // ios chrome
    browser = 'crios'
  } else if (/Chrome/i.test(UA)) {
    // android chrome
    browser = 'chrome'
  } else if (/iPhone; CPU iPhone OS/i.test(UA) && /Mac OS X/i.test(UA) && /AppleWebKit/i.test(UA) && /Version/i.test(UA) && /Mobile/i.test(UA) && /Safari/i.test(UA)) {
    if (safariVersion <= 8.0) { // not so sure whether to be 80
      // 猎豹采用 Safari7.0
      browser = 'liebao' // include lower safari
    } else {
      browser = 'safari'
    }
  } else {
    browser = 'other'
  }
  result.push(browser)
  if (getPlatform) {
    result.push(platform)
  }
  return result
}
```
