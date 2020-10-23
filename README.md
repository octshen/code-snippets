
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

function isObject(value) {
  const type = typeof value
  return value != null && (type == 'object' || type == 'function')
}

/**
 * @desc 深拷贝，结构化拷贝，支持string,number,date,reg等格式，不支持function拷贝
 * @param {Any} obj 
 * @param {WeakMap} hash 
 * @return {Any}
 */

function deepClone(obj, hash = new WeakMap()) {
  if (null == obj || "object" != typeof obj) return obj;
  let cloneObj
  let Constructor = obj.constructor
  switch (Constructor) {
    case RegExp:
      cloneObj = new Constructor(obj)
      break
    case Date:
      cloneObj = new Constructor(obj.getTime())
      break
    default:
      if (hash.has(obj)) return hash.get(obj)
      cloneObj = new Constructor()
      console.log(cloneObj)
      hash.set(obj, cloneObj)
  }
  for (let key in obj) {
    cloneObj[key] = isObject(obj[key]) ? deepClone(obj[key], hash) : obj[key];
  }
  return cloneObj
}

function wrap(ajax) {
  let stack = []
  return function () {
    let req
    let self = this
    let length = stack.length + 1
    let p = new Promise((reslove, reject) => {
      req = ajax.call(self, ...arguments).then(data => {
        if (stack.length === length) {
          // 最新的请求
          stack = []
          reslove(data)
        } else {
          // 如果是过期的请求就reject 或者随意
          reject('old request')
        }
      })
    })
    stack.push(req)
    return p
  }
}
// 假定一个普通的ajax请求
let ajax = () => {
  return new Promise((reslove, reject) => {
    setTimeout(() => reslove('success'), 3000)
  })
}
// 封装过的特殊ajax请求
let getSomeAjax = wrap(ajax)

// 发起请求
getSomeAjax().then(console.log, console.err)


// 数组convert
function convert(list) {
  const res = []
  const map = list.reduce((res, v) => ((res[v.id] = v), res), {})
  for (const item of list) {
    if (item.parentId === 0) {
      res.push(item)
      continue
    }
    if (item.parentId in map) {
      const parent = map[item.parentId]
      parent.children = parent.children || []
      parent.children.push(item)
    }
  }
  return res
}


const pipe = (...args) => x => args.reduce((res, cb) => cb(res), x)

const compose = (...args) => x => args.reduceRight((res, cb) => cb(res), x)

export function formatInp(val, len = 3) {
  val = val.replace(/[^\d.]/g, '') //清除“数字”和“.”以外的字符
  val = val.replace(/\.{2,}/g, '.') //只保留第一个. 清除多余的
  val = val
    .replace('.', '$#$')
    .replace(/\./g, '')
    .replace('$#$', '.')
  // eslint-disable-next-line prettier/prettier
  let pattern = ''
  eval(
    `pattern = /^(\\d+)\\.(${Array(len) //只能输入len位小数
      .fill('\\d')
      .join('')}).*$/`
  )
  // eslint-disable-next-line no-undef
  val = val.replace(pattern, '$1.$2')
  return val
}

export function arraySort(arr, orders) {
  arr.sort(function(a, b) {
    return sortByProps(a, b, orders)
  })
}
function sortByProps(a, b, orders) {
  let cps = [] // 存储排序属性比较结果。
  // 当return 的值大于0时当前比较的两项 交换位置 小于0不换 0不变
  if (orders && typeof orders === 'object') {
    for (let k in orders) {
      let asc = orders[k] === 'asc'
      if (a[k] > b[k]) {
        cps.push(asc ? 1 : -1)
        break
      } else if (a[k] === b[k]) {
        cps.push(0)
      } else {
        cps.push(asc ? -1 : 1)
        break
      }
    }
  }
  for (let j = 0; j < cps.length; j++) {
    if (cps[j]) {
      return cps[j]
    }
  }
  return 0
}

function createFlow(handlers) {
  return {
    effects: handlers.map(item => item.effects ? item.effects : item).flat(Infinity),
    results: [],
    async *[Symbol.asyncIterator]() {
      for (let func of this.effects) {
        const res=  await func()
        yield res
      }
    },
    async run (cb) {
      console.log(this)
      for await (let res of this) {
        this.results.push(res)
      }
      cb && cb(this.results)
    }
  }
}

export function initChart({
  data,
  dataOpt = {
    xAxis: '',
    dataKey: {
      y1: [],
      y2: []
    },
    format: {},
    legend: []
  },
  $el,
  chartOpt
}) {
  dataOpt.format = dataOpt.format || {}
  const option = deepClone(chartOpt)
  option.legend.data = Object.values(dataOpt.legend)
  option.xAxis.data = data.map(t => {
    const { xAxis, format } = dataOpt
    return format[xAxis] ? format[xAxis](t[xAxis]) : t[xAxis]
  })

  const y1Datas = dataOpt?.dataKey.y1?.map(key =>
    data.map(t => {
      return dataOpt.format[key] ? dataOpt.format[key](t[key]) : t[key]
    })
  )

  const y2Datas = dataOpt?.dataKey.y2?.map(key =>
    data.map(t => {
      return dataOpt.format[key] ? dataOpt.format[key](t[key]) : t[key]
    })
  )
  if (data.length >= 2) {
    y1Datas && calcMaxMin(y1Datas.flat(), option, 0)
    y2Datas && calcMaxMin(y2Datas.flat(), option, 1)
  }
  const allData = [...(y1Datas || []), ...(y2Datas || [])]
  option.series.forEach((t, i) => {
    t.data = allData[i]
    t.name = dataOpt.legend[i]
    option.legend.data[i] = allData[i].every(t => t === undefined)
      ? ''
      : option.legend.data[i]
  })
  const chartIns = window.echarts.init($el)
  chartIns.setOption(option)
  return chartIns
}
function calcMaxMin(data, option, ind) {
  if (data.every(t => t === undefined)) return
  const max = Math.max(
    ...data.filter(t => t !== undefined).map(j => j.value || j)
  )
  const min = Math.min(
    ...data.filter(t => t !== undefined).map(j => j.value || j)
  )
  const interval = (max - min) / 3
  if (Math.abs(interval) > 0 && min - interval >= 0) {
    option.yAxis[ind].min = min - interval
    option.yAxis[ind].max = max + interval
    option.yAxis[ind].interval = interval
  }
  if (Math.abs(interval) === 0) {
    const val = max
    option.yAxis[ind].min = 0
    option.yAxis[ind].max = +val + val / 4
    option.yAxis[ind].interval = val / 4
  }
  if (min - interval < 0) {
    option.yAxis[ind].min = 0
    option.yAxis[ind].max = max + max / 4
    option.yAxis[ind].interval = max / 4
  }
  if (min === 0 && max === 0 && interval === 0) {
    option.yAxis[ind].min = 0
    option.yAxis[ind].max = 5
    option.yAxis[ind].interval = 1
  }
}
// 转换当前时间到其他时区时间 
// offset 第几时区
function convertZoneTime(offset = 0) {
  let d = new Date()
  let localTime = d.getTime()
  let localOffset = d.getTimezoneOffset() * 60000
  let utc = localTime + localOffset
  let time = utc + 3600000 * offset
  return new Date(korean)
}

```
