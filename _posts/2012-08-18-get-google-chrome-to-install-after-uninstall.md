---
layout: post
title: 谷歌浏览器卸载后无法重装的解决办法
category: browser
tags: chrome reinstall
---
### 打开注册表[Run - regedit]
删除下面两处注册表项即可。


{% highlight registry linenos %}
[HKEY_CURRENT_USER\Software\Google\Update\Clients\{8A...96}]
[HKEY_CURRENT_USER\Software\Google\Update\ClientStatue\{8A...96}]
{% endhighlight %}

至此，问题解决，可以重装了。
