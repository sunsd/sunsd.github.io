---
layout: post
title: 修改Debian时区，配置中文环境
category: Linux
tags: Debian timezone font
permalink: /posts/change-time-zone-and-install-chinese-font.html
desc: "更改系统时区；安装中文字体，如微软雅黑。"
---
### 配置时区
终端输入 `sudo dpkg-reconfigure tzdata`

它会改这两个文件：

- `/etc/timezone` #*系统时区*

- `/etc/localtime` #*本地时间*，还可以这样手动更改：
    `ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime`

是否用UTC时间可以改这个文件: `/etc/default/rcS`

UTC=no 或者 UTC=yes

### 启用中文环境
1. 终端输入 `dpkg-reconfigure locales`

2. 一路向下，找到 `zh_CN.UTF-8 UTF-8` ，按**空格键**选中，按**Tab键**确认。

3. 注销/重启。

### 添加中文字体
#### 法一
使用 `aptitude search wqy` 搜索字体文件；

然后选择安装搜到的字体 `apt-get install xxx` ，其中`xxx`是你打算安装的中文字体名 。

#### 法二
从Windows系统（`C:\Windows\Fonts\`）拷贝中文字体**yyy.ttf**到Linux `/usr/share/fonts/truetype`，其中，`yyy`是字体名。

如果没有改变，继续下面的步骤，终端输入：

<div class="panel panel-default terminal">
  <div class="panel-heading">Terminal</div>
  <div class="panel-body">
{% highlight sh %}
	 ~ $ su
	~ $ cd /usr/share/fonts/truetype/
	~ $ chmod 644 yyy.ttf
	~ $ mkfontscale
	~ $ mkfontdir
	~ $ fc-cache
{% endhighlight %}
  </div>
</div>
