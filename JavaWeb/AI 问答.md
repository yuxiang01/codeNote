# AI 问答集
## 1. Promise + AJAX

```js
// 创建一个Promise
let promise = new Promise((resolve, reject) => {
  // 创建一个XMLHttpRequest对象
  let xhr = new XMLHttpRequest();

  // 设置请求方法和请求地址
  xhr.open('GET', 'https://example.com/api/data');

  // 发送请求
  xhr.send();

  // 监听状态改变事件
  xhr.onreadystatechange = () => {
    // 如果请求完成并且响应就绪
    if (xhr.readyState === 4 && xhr.status === 200) {
      // 解析响应数据
      let data = JSON.parse(xhr.responseText);
      // 如果成功，调用resolve方法
      resolve(data);
    } else {
      // 如果失败，调用reject方法
      reject(xhr.statusText);
    }
  };
});

// 调用then方法来处理成功和失败的情况
promise.then((data) => {
  // 请求成功时，在这里处理响应数据
  console.log(data);
}, (error) => {
  // 请求失败时，在这里处理错误信息
  console.error(error);
});
```

##  2. 支付轮询
- 支付结果解决方案
	1. 支付轮询
		1. 是一种常见的支付处理方式，其中浏览器或应用程序定期地向支付服务器发送请求，以检查交易是否已完成。这种方法的优点在于可以提供实时的交易状态更新，
		2. 但也有一些缺点，比如可能会增加服务器的负载，或者用户在等待交易完成时会感到不太愉快。
		3. 此外，如果轮询的频率过高，它可能会对用户的网络带宽造成一定的负担。
	2. 推送通知
		1. 当交易完成时，支付服务器会主动向浏览器或应用程序发送一条信息，通知用户交易已完成。
		2. 这种方法的优点在于，它可以减少对服务器的负载，并且用户可以立即收到交易完成的通知，而不需要等待下一次轮询。
		3. 此外，它还可以帮助减少对用户网络带宽的负担，因为浏览器或应用程序不需要定期地发送请求来检查交易状态

支付轮询
```js
// 轮询的时间间隔，单位为毫秒
const POLL_INTERVAL = 1000;

function pollForPaymentCompletion(transactionId) {
  // 定时执行轮询任务目录、   ​setInterval(() => {
    // 向支付服务器发送请求，检查交易是否已完成
    fetch(`/api/transactions/${transactionId}`)
      .then(response => response.json())
      .then(transaction => {
        if (transaction.status === 'complete') {
          // 如果交易已完成，显示提示信息
          alert('交易已完成！');
        }
      });
  }, POLL_INTERVAL);
}
```

fetch[节流 + 防抖]
```js
// 节流的时间间隔，单位为毫秒
const THROTTLE_INTERVAL = 1000;

// 防抖的延迟时间，单位为毫秒
const DEBOUNCE_DELAY = 500;

// 上一次发送请求的时间戳
let lastRequestTime = 0;

// 发送网络请求
function sendRequest(url) {
  // 获取当前时间戳
  const currentTime = Date.now();

  // 计算两次请求的时间间隔
  const timeDelta = currentTime - lastRequestTime;

  // 如果时间间隔小于节流的时间间隔，则不发送请求
  if (timeDelta < THROTTLE_INTERVAL) {
    return;
  }

  // 更新上一次发送请求的时间戳
  lastRequestTime = currentTime;

  // 使用 fetch 方法发送请求
  fetch(url)
    .then(response => response.json())
    .then(data => {
      // 处理响应内容
      // ...
    });
}

// 延迟发送请求
function debounce(fn, delay) {
  // 延迟执行函数
  let timer = null;

  // 返回一个新函数
  return function(...args) {
    // 清除定时器
    clearTimeout(timer);

    // 设置定时器
    timer = setTimeout(() => {
      // 执行函数
      fn(...args);
    }, delay);
  };
}

// 使用 debounce 函数包装 sendRequest
const debouncedSendRequest = debounce(sendRequest, DEBOUNCE_DELAY);

// 监听用户输入事件
document.querySelector('input').addEventListener('input', event => {
  // 获取用户输入的内容
  const input = event.target.value;

  // 延迟发送请求
  debouncedSendRequest(`/api/search?q=${input}`);
});
```

## Sql 合并
### 合并 1
```sql
SELECT COUNT(`status`) nopay FROM easybuy_order WHERE userId = 26 AND `status`= 1;
SELECT COUNT(`status`) noreceipt FROM easybuy_order WHERE userId = 26 AND `status`= 3;
SELECT COUNT(`status`) complete FROM easybuy_order WHERE userId = 26 AND `status`= 3;
```
可以使用 MySQL 的 SELECT 语句的聚合函数和条件语句来实现这三个查询语句的整合。具体实现方法如下：
```sql
SELECT 
    COUNT (CASE WHEN `status` = 1 THEN 1 END) AS nopay, 
    COUNT (CASE WHEN `status` = 3 THEN 1 END) AS noreceipt, 
    COUNT (CASE WHEN `status` = 4 THEN 1 END) AS complete 
FROM easybuy_order WHERE userId = 26;
```
以上 SQL 语句使用了三个聚合函数 COUNT 和三个条件语句 CASE WHEN，用于计算指定用户在 easybuy_order 表中各个状态的订单数量。其中，CASE WHEN 语句用于对 status 列进行条件判断，当 status 的值等于指定的值（1、3 或 4）时，返回 1；否则返回 NULL。COUNT 函数用于对返回的 1 或 NULL 值进行计数，从而得到各个状态的订单数量。

需要注意的是，这三个语句的查询条件相同，因此可以将其整合为一个语句，使用不同的条件值和别名来区分各个状态的订单数量。
### 合并 2
```sql
UPDATE `easybuy_user_address` SET `isDefault` = 0 WHERE userId = 18;
UPDATE `easybuy_user_address` SET `isDefault` = 1 WHERE id = 15;
```
可以使用 MySQL 的 UPDATE 语句的条件表达式来合并这两条 SQL 语句，具体实现方法如下：
```sql
UPDATE `easybuy_user_address` 
SET `isDefault` = (CASE WHEN id = 15 THEN 1 ELSE 0 END) 
WHERE userId = 18 OR id = 15;
```
以上 SQL 语句使用了条件表达式 `CASE WHEN`，用于对指定的地址进行判断，当地址的 id 等于指定的 id（15）时，将其 `isDefault` 列更新为 1；否则将 `isDefault` 列更新为 0。同时使用了 `OR` 运算符，将两个更新条件组合到一起，以在一条 SQL 语句中完成两个更新操作。

需要注意的是，在这个SQL语句中，使用了地址的`id`来判断是否是需要设为默认地址的地址，因此需要保证每个地址的`id`值唯一，并且不与`userId`的值重复。
