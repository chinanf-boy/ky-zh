
# ky [![translate-svg]][translate-list]

[translate-svg]: http://llever.com/translate.svg
[translate-list]: https://github.com/chinanf-boy/chinese-translate-list\

「 Ky是一个小巧典雅的基于浏览器[Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/WindowOrWorkerGlobalScope/fetch)的HTTP客户端 」

[中文](./readme.md) |  [english](https://github.com/sindresorhus/ky)

---

### 校对 🀄

<!-- doc-templite START generated -->
<!-- repo = 'sindresorhus/ky' -->
<!-- time = '2018 9.7' -->
<!-- commit = '02c3a7e0ef2781db48be126e7ac467f062df8858' -->
翻译的原文 | 与日期 | 最新更新 | 更多
---|---|---|---
[commit] | ⏰ 2018 9.7 | ![last] | [中文翻译][translate-list]

[last]: https://img.shields.io/github/last-commit/sindresorhus/ky.svg
[commit]: https://github.com/sindresorhus/ky/tree/02c3a7e0ef2781db48be126e7ac467f062df8858
<!-- doc-templite END generated -->

### 贡献

欢迎 👏 勘误/校对/更新贡献 😊 [具体贡献请看](https://github.com/chinanf-boy/chinese-translate-list#贡献)

## 生活

[help me live , live need money 💰](https://github.com/chinanf-boy/live-need-money)

---

<div align="center">
	<br>
	<div>
		<img width="600" height="600" src="media/logo.svg" alt="ky">
	</div>
	<br>
	<br>
	<br>
</div>

> Ky是一个小巧典雅的基于浏览器[Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/WindowOrWorkerGlobalScope/fetch)的HTTP客户端

[![Build Status](https://travis-ci.com/sindresorhus/ky.svg?branch=master)](https://travis-ci.com/sindresorhus/ky) [![codecov](https://codecov.io/gh/sindresorhus/ky/branch/master/graph/badge.svg)](https://codecov.io/gh/sindresorhus/ky)

ky的目标是[modern browsers](#browser-support). 对于较旧的浏览器,您需要导入并使用[`fetch` polyfill](https://github.com/github/fetch). 对于Node.js,使用[Got](https://github.com/sindresorhus/got)就好了.

1 KB *(minified & gzipped)** ,一个文件,没有依赖项. 

## 比`fetch`更少要求

-   简单API
-   方法快捷方式 (`ky.post()`) 
-   将非200状态码作为错误处理
-   请求失败,重试
-   可JSON化
-   超时支持
-   具有自定义默认值的实例

## 安装

    $ npm install ky

<a href="https://www.patreon.com/sindresorhus">
	<img src="https://c5.patreon.com/external/logo/become_a_patron_button@2x.png" width="160">
</a>

## 用法

```js
import ky from 'ky';

(async () => {
	const json = await ky.post('https://some-api.com', {json: {foo: true}}).json();

	console.log(json);
	//=> `{data: '🦄'}`
})();
```

用`fetch`这将是大量的: 

```js
(async () => {
	class HTTPError extends Error {}

	const response = await fetch('https://sindresorhus.com', {
		method: 'POST',
		body: JSON.stringify({foo: true}),
		headers: {
			'content-type': 'application/json'
		}
	});

	if (!response.ok) {
		throw new HTTPError(`Fetch error:`, response.statusText);
	}

	const json = await response.json();

	console.log(json);
	//=> `{data: '🦄'}`
})();
```

## API

### KY (input,[options]) 

这个`input`和`options`和[`fetch`](https://developer.mozilla.org/en-US/docs/Web/API/WindowOrWorkerGlobalScope/fetch)一样, 但有些例外: 

-   默认情况下,这个`credentials`选项是`same-origin`,这也是规范中的默认值,但并非所有浏览器都已经规范化了. 
-   添加更多选项. 见下文. 

返回一个[`Response` 对象](https://developer.mozilla.org/en-US/docs/Web/API/Response),其具有[`Body` 方法](https://developer.mozilla.org/en-US/docs/Web/API/Body#Methods)为了方便而添加. 例如,你可以直接对`Response`用`ky.json()`,而不用等待. 不像`window.Fetch`的`Body`方法,如果响应状态不在`200...299`范围内,就扔出`HTTPError`错误.

#### options

类型: `Object`

##### json

类型: `Object`

发送JSON的快捷方式. 用这个代替`body`选项. 接受一个plain的对象,将被`JSON.stringify()`处理,且为您设置正确的`header`. 

### ky.get(input, [options])
### ky.post(input, [options])
### ky.put(input, [options])
### ky.patch(input, [options])
### ky.head(input, [options])
### ky.delete(input, [options]) 

`options.method`方法集合名称,发出请求. 

#### retry

- 类型: `number`
- 默认: `2`
- 描述: 重试

用以下方法之一导致的网络错误或状态代码之一, 会重试失败的请求.

方法: `GET` `PUT` `HEAD` `DELETE` `OPTIONS` `TRACE`<br>状态码: [`408`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/408) [`413`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/413) [`429`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/429) [`500`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/500) [`502`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/502) [`503`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/503) [`504`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/504)

#### timeout

类型: `number`<br>
默认: `10000`

以毫秒为单位,获得响应的超时时间. 

### ky.extend(defaultOptions)

创建新的`ky`实例,自己重写部分选项. 

#### defaultOptions

类型: `Object`

### ky.HTTPError

暴露给`instanceof`做检查. 错误会有[`Response` 对象](https://developer.mozilla.org/en-US/docs/Web/API/Response)的`response`属性.

### ky.TimeoutError

请求超时时,引发的错误. 

## 提示

### 取消

Fetch (因此Ky) 具有内置的请求取消的支持,通过[`AbortController` API](https://developer.mozilla.org/en-US/docs/Web/API/AbortController).[查阅更多.](https://developers.google.com/web/updates/2017/09/abortable-fetch)

例子: 

```js
import ky from 'ky';

const controller = new AbortController();
const {signal} = controller;

setTimeout(() => controller.abort(), 5000);

(async () => {
	try {
		console.log(await ky(url, {signal}).text());
	} catch (error) {
		if (error.name === 'AbortError') {
		  console.log('Fetch aborted');
		} else {
		  console.error('Fetch error:', error);
		}
	}
})();
```

## 常见问题解答

#### 它与[`r2`](https://github.com/mikeal/r2)有什么不同?

看到我的答案[#10](https://github.com/sindresorhus/ky/issues/10).

## 浏览器支持

最新版本的Chrome,Firefox和Safari. 

## 相关的

-   [got](https://github.com/sindresorhus/got)-对Node.js的HTTP请求

## 许可证

MIT © [Sindre Sorhus](https://sindresorhus.com)
