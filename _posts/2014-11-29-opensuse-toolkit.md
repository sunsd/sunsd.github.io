---
layout: post
title: OpenSUSE toolkit
category: linux
tags: opensuse
---
计划：找个时间写个脚本，自动安装/升级所有工具软件，不要太依赖系统是否已安装。


### update
```sh
sudo zypper -n up
```

### zypper in
```sh
ack     #curl http://beyondgrep.com/ack-2.14-single-file > ~/bin/ack && chmod 0755 !#:3
git
Jekyll
mlocate
```

### git
```sh
git config --global user.name "sunsd"
git config --global user.email "sunsd0@gmail.com"
git clone https://github.com/gmarik/vundle.git ~/.vim/bundle/vundle
git clone https://github.com/sunsd/vim.git          #BundleInstall
```

### Jekyll
```sh
sudo gem install bundler
sudo zypper in ruby-devel     #用于执行`bundle install`时编译安装gem包
sudo zypper in nodejs          #js运行环境

echo "source 'https://rubygems.org'
gem 'github-pages'" >~/Gemfile

bundle install     #不加sudo。若提示找不到bundle，手动添加路径：locate -w bundle|grep bundle$

bundle exec jekyll serve
```
