---
layout: post
title: shell在工作中的应用 小记（一）
category: shell
tags: shell linux
permalink: /posts/using-shell-in-work-1.html
desc: "自动化重复劳动"
---
### 说明
下面的脚本*依赖*2个自定义环境变量:`$PROJ`，`$MOD`，分别显式定义了当前的默认`工程名`及`模块`。如果没有设定的话，脚本会在执行过程中尝试自动获取。当前，这必须是在工程目录下时才可能猜正确。
### 编写共用脚本
- confirm: 输出确认信息，第二个参数是默认选项。

{% highlight sh linenos %}
#! /bin/sh
# 2013/11/15, sunsd
# confirm "prompt msg..." [default_answer]
# 1 - yes
confirm()
{
    def_ans="no"
    if [ "$2" = "1" ] ;then
        def_ans="yes"
    fi
    echo "$@" >&2
    echo -n "Continue? [ y/n/? ] ($def_ans): " >&2
    read ANS
    case "$ANS" in
        y|yes|Yes|YES)
            exit 0;;
        [nN]*)
            exit 1;;
        *)
            if [ "$def_ans" = "yes" ] ;then
                exit 0
            else
                exit 1
            fi
    esac
}
confirm "$@"
{% endhighlight %}

- ct: 给定参数`n`，截取*前面n级目录*并输出。**注意**，这里把`/`作为一级目录。

{% highlight sh linenos %}
#! /bin/sh
# 2013/12/08, sunsd
# ct, cut the top n level of the directory

# check if argument is consist of digital
echo $1|grep -q '^[0-9]\+$'
if [ $? -eq 0 ]; then
    let f=$1
    # take / as the first level of directory
    if [ $f -eq 1 ]; then
        dir=/
    else
        [ "$2" != "" ] && dir="$2" || dir=`pwd`
        dir=`echo $dir|cut -d/ -f1-$f`
    fi
    echo $dir
else
    echo "Warning: argument must a number!" >&2
    exit 1
fi
{% endhighlight %}

- pd: 输出当前的工程*主目录*。如工程名为`SZ5555`，则输出为`project/SZ5555/ccsrc`。

{% highlight sh linenos %}
#!/bin/sh
# 2012/12/12, sunsd
# pd, print the root work directory of the project
pdirname()
{
    dir=/project/$PROJ/ccsrc
    if [ "$PROJ" = "" ]; then
        echo "Warning: env PROJ is null!" >&2
        dir=$(ct 4)
        if [[ $dir = /ccsrc* ]]; then
            dir=$(ct 2)
        elif [[ $dir != */ccsrc* ]]; then
            echo "Error: none possible value for PROJ!" >&2
            exit 1
        fi
        echo "Automatically set PROJ dir = { $dir }" >&2
    fi
    if [ -d $dir ]; then
        echo $dir
    else
        echo "project $PROJ does not exist!" >&2
        exit 1
    fi
}
pdirname
{% endhighlight %}

- inst: 输出*某个模块的安装目录*。通过读取Makefile获得。

{% highlight sh linenos %}
#!/bin/sh
# 2013/12/13, sunsd
# inst, fetch the install dir in the Makefile

HTDOCS=$CCRUN/htdocs

pwd=`pwd`
pd=$(pd)
[ $# -gt 0 ] && module=$1 || module=$MOD
if [ "$module" = "" ]; then
    echo "Warning: env MOD is null!" >&2
    if [[ $pwd = *ccsrc* ]]; then
        n=5
        [[ $pwd = /ccsrc* ]] && n=3
        guess_mod=`echo $pwd|cut -d/ -f$n`
        (confirm "Automatically set MOD = { $guess_mod }")
        [ $? -ne 0 ] && exit 1
        module=$guess_mod
    else
        echo "Error: none possible value for MOD!" >&2
        exit 1
    fi
fi
makefile="$pd/$module/Makefile"
if [ -e $makefile ]; then
    INSTDIR=$(grep -m 1 "^INSTALLDIR" $makefile|cut -d= -f2|tr '()' '{}')
    INSTDIR=$(eval echo $INSTDIR)
    echo $INSTDIR
else
    echo "Error: module { $module } not found!" >&2
    exit 1
fi
{% endhighlight %}

### 脚本化常用操作
- cd2: 调用`ct`实现*向上*切换到第n级目录。

{% highlight sh linenos %}
#! /bin/sh
# 2013/12/08, sunsd
# cd2
dir=$(ct $1)
cd $dir
{% endhighlight %}

到这还差一步：在`.bashrc`中加入命令别名，`alias c='. cd2'`。最后，更新环境变量，`. ~/.bashrc`。至此，可以通过在终端输入`c 4`切换到**工程主目录**了（当然，还可以更简：`alias s='c 4'`，现在输入`s`就行了）。其实ct更多地用在别处。

- wp: 快速安装修改好的文件。

{% highlight sh linenos %}
#!/bin/sh
# 2013/12/02, sunsd
# wp, install the given file

list=""
declare -i L=0

usage()
{
    echo "usage: wp [-l] file"
    exit 1
}

install()
{
f=$1
pwd=`pwd`
module=$MOD
if [ "${f:0:1}" != "/" ]; then
    f="$pwd/$f"
fi
if [[ "$f" = */$module* ]]; then
    INSTDIR=$(inst $module)
    [ $? -ne 0 ] && exit 1
    cmd=".dosu install --backup=simple -C -D -m 640 $f $INSTDIR/${f#*$module/}"
    echo $cmd
    eval $cmd
    [ $? -eq 0 ] && echo "[7mDone.[0m"
else
    echo "Error: Path is not valid!" >&2
fi
}

while getopts "l:" OPTION
do
    case $OPTION in
        l) L=1
        list=$OPTARG
        ;;
        \?)
        usage
        ;;
    esac
done

if [ $L -eq 1 ]; then
    while read line
    do
        install $line
    done < $list
elif [ $# -eq 1 ]; then
    install $1
else
    usage
fi
{% endhighlight %}

接下来参照上面对脚本`cd2`的处理进行类似操作即可，如命名为`wp`命令。
为了让脚本更实用，将如下代码插入`~/.vimrc`中：

{% highlight vim linenos %}
map <F11> :call Cf2wf()<CR>
"copy file to webframe dir
function Cf2wf()
    let path = expand("%:p")
    if(executable('wp'))
        execute "!wp ".path
    endif
endfunction
{% endhighlight %}

现在可以在vim中通过按F11一键安装文件（*安装前记得先`:w`保存*）。

- 备份文件，保留历史修改记录

将如下配置加入`~/.vimrc`。

{% highlight vim linenos %}
" backup
set bk
"backupdir, set in ~/.bashrc: export VIMBKDIR=~/tmp
set bdir=$VIMBKDIR
au BufWritePre * let &bex = '~' . strftime("%m%d%H%M")
{% endhighlight %}

其中`$VIMBKDIR`是在`~/.bashrc`中自定义的备份目录，我用的`~/tmp/`。现在每次修改文件都会产生一个备份文件。如果`$VIMBKDIR`不存在会提示保存不了。需要手动新建该备份目录。将`$VIMBKDIR`写入`～/.bashrc`后记得执行`source`命令！否则还是会遇到该问题。可以在vim中输入`:h bex`查看详细文档。

- 对比最新改动，映射到<F10>，输出diff结果

{% highlight vim linenos %}
map <F10> :call C2Lf()<CR>
"compare to the latest file in $VIMBKDIR, e.g. last version.
function C2Lf()
    let path = expand("%:p")
    let file = expand("%:p:t")
    exe "!ls -t $VIMBKDIR/".file."~* |head -1 |xargs -I {} diff ".path." {}"
endfunction
{% endhighlight %}

- 加速打开大文件(打开时，关闭解析等)

{% highlight vim linenos %}
let g:LargeFile = 1048576 * 0.5
augroup LargeFile
  au BufReadPre * let f=expand("<afile>") |
      \if getfsize(f) > g:LargeFile | set ei+=FileType | setlocal noswf bh=unload bt=nowrite ul=-1 |
      \else | set ei-=FileType | endif
augroup END
{% endhighlight %}

- sc: 在工作目录与安装目录间快速切换

{% highlight sh linenos %}
#!/bin/sh
# 2013/12/13, sunsd
# sc, switch between the project's work dir and the install dir

module=$MOD
if [ $# -gt 0 ]; then
    module=$1
fi
pwd=`pwd`
instdir=$(inst $module)
moddir=$(pd)/$module

if [[ "$pwd" = ${CCRUN}/* ]]; then
    cmd="cd $moddir/${pwd#$instdir}"
else
    cmd="cd $instdir/${pwd#$moddir}"
fi
eval $cmd
{% endhighlight %}

### 其他脚本
- ack: 非常实用的搜索脚本，官网<http://beyondgrep.com>。搜索多个关键字好像目前无解。但这可以用`grep -e "x1" -e "x2"`这种形式代替。其中，`x1`, `x2`是要搜索的关键字。
- p: 转换`time_t`型的*unix_time*，下面是一个简单实现。编译后放入`~/home/bin/`中即可。我取名`p`。

{% highlight c linenos %}
#include <sys/types.h>
#include <time.h>
#include <stdio.h>
#include <stdlib.h>

int main(int argc, char** argv) {
    int i = 0;
    if (argc == 1) {
        time_t tm_sec = time(NULL);
        printf("%ld\n", tm_sec);
    }
    else {
        while (++i != argc) {
            time_t tm_sec = (time_t)atol(argv[i]);
            char *ctm = ctime(&tm_sec);
            printf("%s => %s", argv[i], ctm); 
        }
    }

    return 0;
}
{% endhighlight %}

