---
version: v1.4.13
category: API
redirect_from:
  - /docs/v0.37.8/api/session
  - /docs/v0.37.7/api/session
  - /docs/v0.37.6/api/session
  - /docs/v0.37.5/api/session
  - /docs/v0.37.4/api/session
  - /docs/v0.37.3/api/session
  - /docs/v0.36.12/api/session
  - /docs/v0.37.1/api/session
  - /docs/v0.37.0/api/session
  - /docs/v0.36.11/api/session
  - /docs/v0.36.10/api/session
  - /docs/v0.36.9/api/session
  - /docs/v0.36.8/api/session
  - /docs/v0.36.7/api/session
  - /docs/v0.36.6/api/session
  - /docs/v0.36.5/api/session
  - /docs/v0.36.4/api/session
  - /docs/v0.36.3/api/session
  - /docs/v0.35.5/api/session
  - /docs/v0.36.2/api/session
  - /docs/v0.36.0/api/session
  - /docs/v0.35.4/api/session
  - /docs/v0.35.3/api/session
  - /docs/v0.35.2/api/session
  - /docs/v0.34.4/api/session
  - /docs/v0.35.1/api/session
  - /docs/v0.34.3/api/session
  - /docs/v0.34.2/api/session
  - /docs/v0.34.1/api/session
  - /docs/v0.34.0/api/session
  - /docs/v0.33.9/api/session
  - /docs/v0.33.8/api/session
  - /docs/v0.33.7/api/session
  - /docs/v0.33.6/api/session
  - /docs/v0.33.4/api/session
  - /docs/v0.33.3/api/session
  - /docs/v0.33.2/api/session
  - /docs/v0.33.1/api/session
  - /docs/v0.33.0/api/session
  - /docs/v0.32.3/api/session
  - /docs/v0.32.2/api/session
  - /docs/v0.31.2/api/session
  - /docs/v0.31.0/api/session
  - /docs/v0.30.4/api/session
  - /docs/v0.29.2/api/session
  - /docs/v0.29.1/api/session
  - /docs/v0.28.3/api/session
  - /docs/v0.28.2/api/session
  - /docs/v0.28.1/api/session
  - /docs/v0.28.0/api/session
  - /docs/v0.27.3/api/session
  - /docs/v0.27.2/api/session
  - /docs/v0.27.1/api/session
  - /docs/v0.27.0/api/session
  - /docs/v0.26.1/api/session
  - /docs/v0.26.0/api/session
  - /docs/v0.25.3/api/session
  - /docs/v0.25.2/api/session
  - /docs/v0.25.1/api/session
  - /docs/v0.25.0/api/session
  - /docs/v0.24.0/api/session
  - /docs/v0.23.0/api/session
  - /docs/v0.22.3/api/session
  - /docs/v0.22.2/api/session
  - /docs/v0.22.1/api/session
  - /docs/v0.21.3/api/session
  - /docs/v0.21.2/api/session
  - /docs/v0.21.1/api/session
  - /docs/v0.21.0/api/session
  - /docs/v0.20.8/api/session
  - /docs/v0.20.7/api/session
  - /docs/v0.20.6/api/session
  - /docs/v0.20.5/api/session
  - /docs/v0.20.4/api/session
  - /docs/v0.20.3/api/session
  - /docs/v0.20.2/api/session
  - /docs/v0.20.1/api/session
  - /docs/v0.20.0/api/session
  - /docs/vlatest/api/session
source_url: 'https://github.com/electron/electron/blob/master/docs/api/session.md'
title: session
excerpt: 'Manage browser sessions, cookies, cache, proxy settings, etc.'
sort_title: session
---
# session

> Manage browser sessions, cookies, cache, proxy settings, etc.

Process: [Main]({{site.baseurl}}/docs/tutorial/quick-start#main-process)

The `session` module can be used to create new `Session` objects.

You can also access the `session` of existing pages by using the `session` property of [`WebContents`]({{site.baseurl}}/docs/api/web-contents), or from the `session` module.

```javascript
const {BrowserWindow} = require('electron')

let win = new BrowserWindow({width: 800, height: 600})
win.loadURL('http://github.com')

const ses = win.webContents.session
console.log(ses.getUserAgent())
```

## Methods

The `session` module has the following methods:

### `session.fromPartition(partition[, options])`

*   `partition` String
*   `options` Object
    *   `cache` Boolean - Whether to enable cache.

Returns `Session` - A session instance from `partition` string. When there is an existing `Session` with the same `partition`, it will be returned; othewise a new `Session` instance will be created with `options`.

If `partition` starts with `persist:`, the page will use a persistent session available to all pages in the app with the same `partition`. if there is no `persist:` prefix, the page will use an in-memory session. If the `partition` is empty then default session of the app will be returned.

To create a `Session` with `options`, you have to ensure the `Session` with the `partition` has never been used before. There is no way to change the `options` of an existing `Session` object.

## Properties

The `session` module has the following properties:

### `session.defaultSession`

A `Session` object, the default session object of the app.

## Class: Session

> Get and set properties of a session.

Process: [Main]({{site.baseurl}}/docs/tutorial/quick-start#main-process)

You can create a `Session` object in the `session` module:

```javascript
const {session} = require('electron')
const ses = session.fromPartition('persist:name')
console.log(ses.getUserAgent())
```

### Instance Events

The following events are available on instances of `Session`:

#### Event: 'will-download'

*   `event` Event
*   `item` [DownloadItem]({{site.baseurl}}/docs/api/download-item)
*   `webContents` [WebContents]({{site.baseurl}}/docs/api/web-contents)

Emitted when Electron is about to download `item` in `webContents`.

Calling `event.preventDefault()` will cancel the download and `item` will not be available from next tick of the process.

```javascript
const {session} = require('electron')
session.defaultSession.on('will-download', (event, item, webContents) => {
  event.preventDefault()
  require('request')(item.getURL(), (data) => {
    require('fs').writeFileSync('/somewhere', data)
  })
})
```

### Instance Methods

The following methods are available on instances of `Session`:

#### `ses.getCacheSize(callback)`

*   `callback` Function
    *   `size` Integer - Cache size used in bytes.

Returns the session's current cache size.

#### `ses.clearCache(callback)`

*   `callback` Function - Called when operation is done

Clears the session’s HTTP cache.

#### `ses.clearStorageData([options, callback])`

*   `options` Object (optional)
    *   `origin` String - Should follow `window.location.origin`’s representation `scheme://host:port`.
    *   `storages` String[] - The types of storages to clear, can contain: `appcache`, `cookies`, `filesystem`, `indexdb`, `local storage`, `shadercache`, `websql`, `serviceworkers`
    *   `quotas` String[] - The types of quotas to clear, can contain: `temporary`, `persistent`, `syncable`.
*   `callback` Function (optional) - Called when operation is done.

Clears the data of web storages.

#### `ses.flushStorageData()`

Writes any unwritten DOMStorage data to disk.

#### `ses.setProxy(config, callback)`

*   `config` Object
    *   `pacScript` String - The URL associated with the PAC file.
    *   `proxyRules` String - Rules indicating which proxies to use.
    *   `proxyBypassRules` String - Rules indicating which URLs should bypass the proxy settings.
*   `callback` Function - Called when operation is done.

Sets the proxy settings.

When `pacScript` and `proxyRules` are provided together, the `proxyRules` option is ignored and `pacScript` configuration is applied.

The `proxyRules` has to follow the rules below:

```
proxyRules = schemeProxies[";"<schemeProxies>]
schemeProxies = [<urlScheme>"="]<proxyURIList>
urlScheme = "http" | "https" | "ftp" | "socks"
proxyURIList = <proxyURL>[","<proxyURIList>]
proxyURL = [<proxyScheme>"://"]<proxyHost>[":"<proxyPort>]

```

For example:

*   `http=foopy:80;ftp=foopy2` - Use HTTP proxy `foopy:80` for `http://` URLs, and HTTP proxy `foopy2:80` for `ftp://` URLs.
*   `foopy:80` - Use HTTP proxy `foopy:80` for all URLs.
*   `foopy:80,bar,direct://` - Use HTTP proxy `foopy:80` for all URLs, failing over to `bar` if `foopy:80` is unavailable, and after that using no proxy.
*   `socks4://foopy` - Use SOCKS v4 proxy `foopy:1080` for all URLs.
*   `http=foopy,socks5://bar.com` - Use HTTP proxy `foopy` for http URLs, and fail over to the SOCKS5 proxy `bar.com` if `foopy` is unavailable.
*   `http=foopy,direct://` - Use HTTP proxy `foopy` for http URLs, and use no proxy if `foopy` is unavailable.
*   `http=foopy;socks=foopy2` - Use HTTP proxy `foopy` for http URLs, and use `socks4://foopy2` for all other URLs.

The `proxyBypassRules` is a comma separated list of rules described below:

*   `[ URL_SCHEME "://" ] HOSTNAME_PATTERN [ ":" <port> ]`

    Match all hostnames that match the pattern HOSTNAME_PATTERN.

    Examples: "foobar.com", "_foobar.com", "_.foobar.com", "_foobar.com:99", "https://x._.y.com:99"

*   `"." HOSTNAME_SUFFIX_PATTERN [ ":" PORT ]`

    Match a particular domain suffix.

    Examples: ".google.com", ".com", "http://.google.com"

*   `[ SCHEME "://" ] IP_LITERAL [ ":" PORT ]`

    Match URLs which are IP address literals.

    Examples: "127.0.1", "[0:0::1]", "[::1]", "http://[::1]:99"

*   `IP_LITERAL "/" PREFIX_LENGHT_IN_BITS`

    Match any URL that is to an IP literal that falls between the given range. IP range is specified using CIDR notation.

    Examples: "192.168.1.1/16", "fefe:13::abc/33".

*   `<local>`

    Match local addresses. The meaning of `<local>` is whether the host matches one of: "127.0.0.1", "::1", "localhost".

### `ses.resolveProxy(url, callback)`

*   `url` URL
*   `callback` Function
    *   `proxy` Object

Resolves the proxy information for `url`. The `callback` will be called with `callback(proxy)` when the request is performed.

#### `ses.setDownloadPath(path)`

*   `path` String - The download location

Sets download saving directory. By default, the download directory will be the `Downloads` under the respective app folder.

#### `ses.enableNetworkEmulation(options)`

*   `options` Object
    *   `offline` Boolean (optional) - Whether to emulate network outage. Defaults to false.
    *   `latency` Double (optional) - RTT in ms. Defaults to 0 which will disable latency throttling.
    *   `downloadThroughput` Double (optional) - Download rate in Bps. Defaults to 0 which will disable download throttling.
    *   `uploadThroughput` Double (optional) - Upload rate in Bps. Defaults to 0 which will disable upload throttling.

Emulates network with the given configuration for the `session`.

```javascript
// To emulate a GPRS connection with 50kbps throughput and 500 ms latency.
window.webContents.session.enableNetworkEmulation({
  latency: 500,
  downloadThroughput: 6400,
  uploadThroughput: 6400
})

// To emulate a network outage.
window.webContents.session.enableNetworkEmulation({offline: true})
```

#### `ses.disableNetworkEmulation()`

Disables any network emulation already active for the `session`. Resets to the original network configuration.

#### `ses.setCertificateVerifyProc(proc)`

*   `proc` Function
    *   `hostname` String
    *   `certificate` [Certificate]({{site.baseurl}}/docs/api/structures/certificate)
    *   `callback` Function
        *   `isTrusted` Boolean - Determines if the certificate should be trusted

Sets the certificate verify proc for `session`, the `proc` will be called with `proc(hostname, certificate, callback)` whenever a server certificate verification is requested. Calling `callback(true)` accepts the certificate, calling `callback(false)` rejects it.

Calling `setCertificateVerifyProc(null)` will revert back to default certificate verify proc.

```javascript
const {BrowserWindow} = require('electron')
let win = new BrowserWindow()

win.webContents.session.setCertificateVerifyProc((hostname, cert, callback) => {
  callback(hostname === 'github.com')
})
```

#### `ses.setPermissionRequestHandler(handler)`

*   `handler` Function
    *   `webContents` Object - [WebContents]({{site.baseurl}}/docs/api/web-contents) requesting the permission.
    *   `permission` String - Enum of 'media', 'geolocation', 'notifications', 'midiSysex', 'pointerLock', 'fullscreen', 'openExternal'.
    *   `callback` Function
        *   `permissionGranted` Boolean - Allow or deny the permission

Sets the handler which can be used to respond to permission requests for the `session`. Calling `callback(true)` will allow the permission and `callback(false)` will reject it.

```javascript
const {session} = require('electron')
session.fromPartition('some-partition').setPermissionRequestHandler((webContents, permission, callback) => {
  if (webContents.getURL() === 'some-host' && permission === 'notifications') {
    return callback(false) // denied.
  }

  callback(true)
})
```

#### `ses.clearHostResolverCache([callback])`

*   `callback` Function (optional) - Called when operation is done.

Clears the host resolver cache.

#### `ses.allowNTLMCredentialsForDomains(domains)`

*   `domains` String - A comma-seperated list of servers for which integrated authentication is enabled.

Dynamically sets whether to always send credentials for HTTP NTLM or Negotiate authentication.

```javascript
const {session} = require('electron')
// consider any url ending with `example.com`, `foobar.com`, `baz`
// for integrated authentication.
session.defaultSession.allowNTLMCredentialsForDomains('*example.com, *foobar.com, *baz')

// consider all urls for integrated authentication.
session.defaultSession.allowNTLMCredentialsForDomains('*')
```

#### `ses.setUserAgent(userAgent[, acceptLanguages])`

*   `userAgent` String
*   `acceptLanguages` String (optional)

Overrides the `userAgent` and `acceptLanguages` for this session.

The `acceptLanguages` must a comma separated ordered list of language codes, for example `"en-US,fr,de,ko,zh-CN,ja"`.

This doesn't affect existing `WebContents`, and each `WebContents` can use `webContents.setUserAgent` to override the session-wide user agent.

#### `ses.getUserAgent()`

Returns `String` - The user agent for this session.

#### `ses.getBlobData(identifier, callback)`

*   `identifier` String - Valid UUID.
*   `callback` Function
    *   `result` Buffer - Blob data.

Returns `Blob` - The blob data associated with the `identifier`.

#### `ses.createInterruptedDownload(options)`

*   `options` Object
    *   `path` String - Absolute path of the download.
    *   `urlChain` String[] - Complete URL chain for the download.
    *   `mimeType` String (optional)
    *   `offset` Integer - Start range for the download.
    *   `length` Integer - Total length of the download.
    *   `lastModified` String - Last-Modified header value.
    *   `eTag` String - ETag header value.
    *   `startTime` Double (optional) - Time when download was started in number of seconds since UNIX epoch.

Allows resuming `cancelled` or `interrupted` downloads from previous `Session`. The API will generate a [DownloadItem]({{site.baseurl}}/docs/api/download-item) that can be accessed with the [will-download]({{site.baseurl}}/docs/api/#event-will-download) event. The [DownloadItem]({{site.baseurl}}/docs/api/download-item) will not have any `WebContents` associated with it and the initial state will be `interrupted`. The download will start only when the `resume` API is called on the [DownloadItem]({{site.baseurl}}/docs/api/download-item).

#### `ses.clearAuthCache(options[, callback])`

*   `options` ([RemovePassword]({{site.baseurl}}/docs/api/structures/remove-password) | [RemoveClientCertificate]({{site.baseurl}}/docs/api/structures/remove-client-certificate))
*   `callback` Function (optional) - Called when operation is done

Clears the session’s HTTP authentication cache.

### Instance Properties

The following properties are available on instances of `Session`:

#### `ses.cookies`

A Cookies object for this session.

#### `ses.webRequest`

A WebRequest object for this session.

#### `ses.protocol`

A Protocol object (an instance of [protocol]({{site.baseurl}}/docs/api/protocol) module) for this session.

```javascript
const {app, session} = require('electron')
const path = require('path')

app.on('ready', function () {
  const protocol = session.fromPartition('some-partition').protocol
  protocol.registerFileProtocol('atom', function (request, callback) {
    var url = request.url.substr(7)
    callback({path: path.normalize(`${__dirname}/${url}`)})
  }, function (error) {
    if (error) console.error('Failed to register protocol')
  })
})
```
