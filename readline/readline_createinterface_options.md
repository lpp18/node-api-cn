<!-- YAML
added: v0.1.98
changes:
  - version: v8.3.0, 6.11.4
    pr-url: https://github.com/nodejs/node/pull/13497
    description: Remove max limit of `crlfDelay` option.
  - version: v6.6.0
    pr-url: https://github.com/nodejs/node/pull/8109
    description: The `crlfDelay` option is supported now.
  - version: v6.3.0
    pr-url: https://github.com/nodejs/node/pull/7125
    description: The `prompt` option is supported now.
  - version: v6.0.0
    pr-url: https://github.com/nodejs/node/pull/6352
    description: The `historySize` option can be `0` now.
-->

* `options` {Object}
  * `input` {stream.Readable} 要监听的[可读流]。该选项是必需的。
  * `output` {stream.Writable} 要写入逐行读取数据的[可写流]。
  * `completer` {Function} 一个可选的函数，用于 Tab 自动补全。
  * `terminal` {boolean} 如果 `input` 和 `output` 应被当作一个 TTY，且要写入 ANSI/VT100 转换的代码，则设为 `true`。
    默认为实例化时在 `output` 流上检查 `isTTY`。
  * `historySize` {number} 保留的历史行数的最大数量。
    设为 `0` 可禁用历史记录。
    该选项只有当 `terminal` 被用户或内部 `output` 设为 `true` 时才有意义，否则历史缓存机制不会被初始化。
    默认为 `30`。
  * `prompt` - 要使用的提示字符串。默认为 `'> '`。
  * `crlfDelay` {number} 如果 `\r` 与 `\n` 之间的延迟超过 `crlfDelay` 毫秒，则 `\r` 和 `\n` 都会被当作换行分隔符。
    `crlfDelay` 强制设置为不少于 `100`. 可以设置为
    `Infinity`, 这种情况下， `\r` 跟着 `\n` 会被视为单个新行(也许对带有`\r\n`分隔符的`[reading files][]`来说是非常合理的)。
    默认为 `100` 毫秒。
  * `removeHistoryDuplicates` {boolean} 如果为 `true`, 当新输入行与历史列表中的某行相同时，
  那么移除旧有的行。
    默认为 `false`。

`readline.createInterface()` 方法会创建一个新的 `readline.Interface` 实例。

例子：

```js
const readline = require('readline');
const rl = readline.createInterface({
  input: process.stdin,
  output: process.stdout
});
```

一旦 `readline.Interface` 实例被创建，最常见的用法是监听 `'line'` 事件：

```js
rl.on('line', (line) => {
  console.log(`接收到：${line}`);
});
```

如果该实例的 `terminal` 为 `true`，则若它定义了一个 `output.columns` 属性则 `output` 流会获得最佳兼容性，且如果或当列发生变化时，`output` 上触发一个 `'resize'` 事件（当它为一个 TTY 时，[`process.stdout`] 会自动处理这个）。

