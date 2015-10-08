# Coding 云端演示平台静态网站部署指南

### 1. 如何开启

只需要在项目根目录创建一个空文件 `Staticfile`，可以使用 `touch` 命令创建：`touch Staticfile`，或者参考此项目[源码](https://coding.net/u/demo/p/static-site)。

> **注意事项：**
>
> 1. `Staticfile` 为静态网站的特征文件，必须按照要求创建此文件项目才会被识别为静态网站。
> 2. 如果在项目的部署选项里配置了 **源码路径**，则 `Staticfile` 文件需要创建在此目录下。
> 3. 建议把演示项目的内存设置为 `64M`（事实上 20M 足矣），这样可以减少对你账户的内存消耗。

### 2. 自动更新
纯静态 HTML 网站支持通过 [WebHook](https://coding.net/help/about_git/what_is_web_hook) 自动更新网站内容，但默认不会开启，
需要用户手工创建 WebHook 并给演示配置 WebHook 的 `token` 后，此功能才会被激活。

**开启步骤如下：**

1. **添加环境变量:** 给演示添加环境变量，变量名为 `WEBHOOK_TOKEN`，变量值是一个密码，用来验证 WebHook 请求的合法性和激活自动更新功能。
2. **重新部署演示:** 重新部署演示以激活自动更新功能，并确保演示处于可访问状态（保证下一步创建的 WebHook 状态为绿色的钩）。
3. **创建 WebHook:** 到 **项目设置** 处创建 WebHook，`URL` 格式参考见下面的示例，`token` 填上面创建的 `WEBHOOK_TOKEN` 环境变量的值。

**WebHook URL 示例**

只需要在演示的访问 URL 后面添加 `/_` 即可，如演示的访问 URL 为 `http://docs.coding.io`，则 WebHook 的 `URL` 需要填写为:

```
http://docs.coding.io/_
```

> **注意：** 默认情况下 WebHook 只更新来自 `master` 分支的 push，如果你部署的不是 `master` 分支，
> 请务必设置演示环境变量 `GIT_REF` 的值为你部署的分支名并重启应用，否则到非 `master` 分支的 push 不会触发更新。

### 3. 可选配置

#### 3.1 网站根目录

如果你的 html 文件不在根目录下，可以在 `Staticfile` 文件内添加一行配置来指定根目录：

```
root: public
```

#### 3.2 HTTP Basic 认证

如果你想要让别人输入密码才可以访问你的页面，创建一个文件 `Staticfile.auth`，格式和 `.htpasswd` 一样。

此文件可以包含多行，每行指定一组用户名和密码，例如：

```
bob:$apr1$DuUQEQp8$ZccZCHQElNSjrg.erwSFC0
demo:$apr1$rdmy5.Y0$0VVnF9gJUhKakBILLM3k50
```

可以在线生成 `.htpasswd` 的网站：

* <http://www.htaccesstools.com/htpasswd-generator>
* <http://tool.oschina.net/htpasswd>

#### 3.3 列出网站目录

如果网站根目录下没有 `index.html` 文件，访问网站首页将会出现 `404` 页面，这时你可以选择开启列目录功能，这样服务器将会列出此目录下的文件。

在 `Staticfile` 文件内添加一行配置即可开启此功能：

```
directory: visible
```
