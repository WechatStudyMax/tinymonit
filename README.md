# tinymonit

[![Build Status](https://travis-ci.org/hardog/tinymonit.svg?branch=master)](https://travis-ci.org/hardog/tinymonit)
[![Coverage Status](https://img.shields.io/codecov/c/github/hardog/tinymonit.svg)](https://codecov.io/github/hardog/tinymonit?branch=master)
[![License](https://img.shields.io/npm/l/tinymonit.svg)](https://www.npmjs.com/package/tinymonit)
[![npm Version](https://img.shields.io/npm/v/tinymonit.svg)](https://www.npmjs.com/package/tinymonit)

collect the cpu & mem information from the specified machine(remote or local), supports cluster mode which gather all stats from config list!

## Examples

- [get process stat](https://github.com/hardog/tinymonit/blob/master/example/proc_stat.js)
- [get system stat](https://github.com/hardog/tinymonit/blob/master/example/sys_stat.js)
- [use alarm](https://github.com/hardog/tinymonit/blob/master/example/threshold.js)
- [cluster mode](https://github.com/hardog/tinymonit/blob/master/example/cluster)

if you wanna debug, just execute `$ DEBUG=* node example.js` to enable debug.
following, also i have made a wechat app example:

![tm-wechat](http://hardog.net/images/assist/20161008/tm-wechat-example.gif)

wechat-tinymonit 代码详见: [点击查看wechat ui代码](https://github.com/hardog/wechat-app-flexlayout)

# Feature list

- promisify function
- gather system cpu usage
- gather system mem usage
- gather specified process cpu & mem usage
- judge if over the threshold
- support cluster mode

# Install

`$ npm install tinymonit -g`

# Test

run test:
```
$ npm run test
```

run test coverage:
```
$ npm run cover
```

# Usage

## get os stat

```
const tm = require('tinymonit');
const osstat = tm.osstat;

Promise.resolve()
.then(() => osstat([8122]))
.then((stat) => console.log(r));
```

## create a part

```
const Part = require('tinymonit').part;

let part = new Part(3000, {
	timeout: 100,
	pid: process.pid // this is defalut
});
```

## create a central

```
const Central = require('tinymonit').central;

let ctl = new Central({
	timeout: 100,
	parts:[
		3000,
		[3001, 200]
		//'[remote ip]:[port]'
	]
});

// start collect performance data from 3000, 3001
Promise.resolve()
.then(() => ctl.collect())
.then((allstats) => {
	// do sth
});
```

## alarm when over threshold

```
const tm = require('tinymonit');
const pid = process.pid;
const osstat = tm.osstat;
const alarm = tm.alarm;

Promise.resolve()
.then(() => osstat([pid]))
.then((stat) => {
	console.log(alarm.should_cpu_alarm(rstat, 1));
	console.log(alarm.should_mem_alarm(stat));
	console.log(alarm.should_load_alarm(stat));
	console.log(alarm.should_procs_alarm(stat, 2));
});

```

## License

[MIT](https://github.com/hardog/tinymonit/blob/master/LICENSE)
