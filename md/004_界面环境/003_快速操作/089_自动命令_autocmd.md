---
show: step
version: 1.0
enable_checker: true
---

# 自动命令 autocommand

## 回忆

- 上次我们研究的是外部命令grep
	- 可以在vim中使用grep
	- 搜索的结果进入了列表
	- 可以打开、遍历、跳转、关闭这个列表
	- 也可以给列表中的匹配行或者每个文件执行命令
- 到此为止学了很多的命令
    - 有内部的也有外部的
    - 有的在命令行模式里面执行
    - 还有的映射到一组键盘在正常模式下执行
    - 但是都需要按下些什么按键才能支持
- 能否什么都不按自动就执行呢？🤔

### 自动命令autocmd

- 在~/.vim/ftplugin(当前用户的插件目录)编辑插件文件
- ftplugin中
	- ft代表filetype
	- plugin代表插件

```
mkdir -p ~/.vim/ftplugin
sudo vi ~/.vim/ftplugin/log.vim
```

- 定义函数

```
function DateInsert()
	$delete
	read !date
endfunction
```

- $delete
	- 把原来的最后一行的日期删除掉
- read !date 
	- 读取当前日期
	- 并且写到最后一行

### 尝试调用

- 新建并打开log文件
	- vi a.log
- 调用函数
	- `:call DateInsert()`
	- 但是他好像不认识log文件一样
	- `:function`中没有DateInsert()
	- `:function DateInsert`

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20220119-1642557361728)

### 设置filetype文件类型
- <kbd>Q</kbd>进入Ex模式
- `:filetype`
	- 查看文件类型检测情况
- `:set filetype=log`
	- 强制设置文件类型为log
- `:function DateInsert`
	- 观察DateInsert函数
- 可以找到了定义的函数了
- 是否可以调用呢？


![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210802-1627877960861)

- `:call DateInsert()`
	- 调用DateInsert()
- `:visual`
	- 回到编辑模式
- 但是每次都要手动设置filetype么？
- 这颗太麻烦了
- 能否自动检测呢？

### 自动检测文件类型filetype
- 查询帮助
	- `:h filetype`

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210802-1627878113532)

- `mkdir ~/.vim/ftdetect`
- `sudo vi ~/.vim/ftdetect/log.vim`
	- ft代表filetype
	- detect 代表文件自动检测
- 在文件里面写上

```vim
au BufRead,BufNewFile *.log set filetype=log
```

- `vi a.log`

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210802-1627878382575)

- 检测成功
- `:call DateInsert()`
- 函数调用成功
- 我想把这个函数做成一个命令可以么？

### 制作命令

- `:nnoremap Di :call DateInsert()<CR>`
	- `nnoremap`
		- `n` normal 正常模式
		- `noremap` 不再递归映射
    - Di 指定的命令Command
    - :call DateInsert() 调用函数
    - `<CR>` carriage 回车
- 在normal模式下依次按下`Di` 
	- 可以用
- 再次`Di`可以刷新时间
- 我想保存的时候自动调用可以么？

### 自动命令

- `:autocmd BufWritePre *.log call DateInsert()`
	- `autocmd` 是命令的名字 
		- 作用是添加自动命令
	- `BufWritePre` 是{events} 是触发条件 
		- `Buf`指的是当前缓存buffer
		- `Write`指的是写当前缓存
		- `Pre`指的是在写当前缓存之前
	- `*.log` 是 {file_pattern} 是文件模式
		- 目前文件模式为`*.log`
	- `call DateInsert()` 是{cmd} 
		- 对应具体执行的命令
- `:wq`
- `cat a.log`
- 最新的时间已经写在最后一行了
- 不过这个自动命令最好写在filetype的插件里面
- 这样自动命令就被定义到自动插件里面了


### 删除自动命令
- `:autocmd`	
	- 可以查看所有的自动命令autocmd
- `:autocmd! FileWritePre *.log`
	- 注意那个叹号是否定的意思
	- 把他定义为啥都没有
	- 也就删除了

### 配置插件

- `vi ~/.vim/ftplugin/log.vim`
- `:autocmd BufWritePre *.log call DateInsert()`
	- 为什么强调`*.log`呢
	- 因为如果{file_pattern}是`*.*`的话
	- 一旦打开了`log`文件
	- 保存任何其他文件时都会执行`DateInsert()`了


```vim
echo "i am a log!"
set filetype=log	
source ~/.vim/ftplugin/log.vim
autocmd BufWritePre *.log call DateInsert()
```

### 查看帮助
- `:h autocmd`

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210802-1627879118769)


- {events} 具体的触发事件
- {pattern} 文件模式
- {group} 成组 自动命令成组并命名 可省
- group 应该如何理解？

### 成组自动命令
- 成组是可选的
- 可以成组也可以不成组
- 这是绕口令么？
- 我们来看看

```
augroup cprograms
	autocmd BufReadPost *.c *.h :set sw=4 sts=4
	autocmd BufReadPost *.cpp :set sw=3 sts=3
augroup END
```

- 上面说的是在读取.c、.h 之后
    - 自动设置缩进宽度为4
- 在读取.cpp之后
    - 自动设置缩进宽度为3
- 总而言之这两自动命令成为了一个组叫做cprograms
- 也可以写成


```
	autocmd cprograms BufReadPost *.c *.h :set sw=4 sts=4
	autocmd cprograms BufReadPost *.cpp :set sw=3 sts=3
```

- 下面这样可以删除组中所有的自动命令
- `:autocmd! cprograms`
- 这里面提到的Post和Pre应该如何理解呢？

### Post、Pre
- pre就是之前
- post就是之后

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20220429-1651237261135)


- 保存之前可以写一个时间戳做标记
- 读取之后可以设置所之类的东西或者做一些清理工作

### 触发事件events

- `:autocmd BufReadPost *.gsm set filetype=asm`
	- `BufReadPost`是读取之后
	- `set filetype=asm`
		- 把gsm文件的文件格式filetype设置为asm
		- gsm是gnu的assemble language
- `:autocmd Filetype text source ~/.vim/abbrevs.vim`
	- `Filetype text`
		- 是检测到文件类型为文本的时候
		- `vi a.txt`可以触发`Filetype text`
	- `source ~/.vim/abbrevs.vim`
		- 加载一些缩写
- `:autocmd BufNewFile *.[ch] 0read ~/sktletons/skel.c`
	- ` BufNewFile *.[ch]`
		- `BufNewFile` 新建缓存文件的时候
		- `*.[ch]` 
			- 文件类型对应`*.c或者*.h`
	- `0read ~/sktletons/skel.c`
		- 加载c一个骨架文件作为框架
		- 然后添皮加肉

### 文件模式patterns
- 通配符wild character	
	- *任意多个字符或数字
		- *.c
		- *.h
	- ?一个字母
		- D???.c
	- .
		- 对应一个点dot 
	- []或者关系
		- *.[ch]
	- {}或者关系
		- a{b,c}
		- 对应ab或ac
	- /
        - 在对应路径中使用，比如：
		- ~/.vim/ftplugin/*
		- /home/oeasy/*.txt

### 自动命令的嵌套
- 一般来说自动命令的执行结果不会触发另一个自动命令
- 但是，如果你偏要触发
- 可以加上 nested
- `:autocmd FileChangedShell * nested edit`
	- 比如你打开了文件时触发了自动命令
	- 自动命令做出了一些修改
	- 这些修改就也会触发这个事件

### 使用文件名和扩展名
- `touch oeasy.txt oeasy.txt.new`
	- 新建两个文件
- `vi`
- `:echo expand("%:t")`
	- 输出文件名
	- 因为现在啥都没有所以啥都没输出
- `:e oeasy.txt`
	- 打开oeasy.txt进入缓存buffer
- `:echo expand("%:t")`
	- 输出当前文件名
- `:echo "hello i am " . expand("%:t")`
	- 输出一句话

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210802-1627896494828)

- `autocmd BufReadPost * echo "hello i am " . expand("%:t")`
	- 定义自动命令 
	- 读取任何文件格式的文件之后
	- 输出hello 和 当前文件名
- `:h expand`
	- 显示`hello i am eval.txt`
- 成功

### 强制触发

- `:doautocmd BufReadPost oeasy.txt`
	- 虽然后没有打开`oeasy.txt`
	- 但是他强制执行了`BufReadPost oeasy.txt`对应的自动命令`autocmd`

- 这样的话我可以在读取oeasy.txt.new的时候
- 然后显示出oeasy.txt么？
- `:autocmd BufReadPost *.new echo "hhh " . expand("<afile>:r")`

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20220119-1642562847311)

- 先退出vim

### 执行其他的自动命令

- 重新打开vim
- 输入
	- `:autocmd BufReadPost *.txt echo "hello i am txt:" . expand("%:t")`
	- 定义对于txt文件读取之后的自动命令
- 然后打开txt文件和非txt文件
- 打开或切换时会有相应的显示
- 而非txt文件不会有显示

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20220119-1642565116507)

- 此时打开oeasy.txt.new没有任何提示
- `:autocmd BufReadPost *.new execute "doautocmd BufReadPost " . expand("<afile>:r")`
	- `autocmd BufReadPost *.new`
		- 定义`*.new`打开之后对应的自动命令
	- `excute "doautocmd BufReadPost " . expand("<afile>:r")`
		- 执行 "doautocmd BufReadPost " . expand("<afile>:r")

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210802-1627897130065)

- 如果我们重新打开`oeasy.txt.new` 或者切换buffer的时候
	- 强制执行`oeasy.txt`打开后的自动命令
	- 也就是`execute "doautocmd BufReadPost oeasy.txt"`

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20220119-1642565370508)

### 执行正常模式命令

- 刚才执行的都是命令行模式的命令
- 如果我想执行正常模式的命令应该如何呢？
- `:autocmd BufReadPost *.log normal G`
	- 读取`*.log`的时候
	- normal G 在正常模式下执行G
	- 跳到最后一行，查看最新的日志
- 那么可以自动命令可以进入插入模式么？


- `normal`进入正常模式，在正常模式下
	- <kbd>i</kbd>进入插入模式 <kbd>esc</kbd>退出
	- <kbd>:</kbd>进入命令模式 <kbd>esc</kbd>退出
	- <kbd>/</kbd>进入搜索模式 <kbd>esc</kbd>退出
- `:autocmd BufReadPost *.txt execute "normal ggONew entry:\<Esc>" | 1read !date`
	- `autocmd BufReadPost *.txt `
		- 制作读取txt文件后对应自动命令
	- `excute "normal ggONew entry:\<Esc>" | 1read !date`
		- 执行`normal ggONew entry:\<Esc>`在第一行写字
		- `|` 然后执行
		- `1read !date` 在第二行写日期

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210802-1627898210026)

- 我们最后来看看已经写好的一些autocmd

### vim的系统文件夹

- 我们之前都是在用户的vim文件夹进行配置
	- 用户的vim文件夹只能配置当前用户的vim
- 现在我们去系统的vim文件夹看看具体的配置
	- 这样我们就可以给所有用户配置vim了
- 系统的vim的文件夹在
	- `/usr/share/vim/vim81` 
- 基本上配置都在这里完成

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20220119-1642567256052)

- 其中有一些缩写
	- au autocmd
	- exe execute

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20220119-1642566993343)

## 总结

- 这个自动命令还是很方便的
- 打开时、保存时就会有自动执行的操作
- 自动命令有这么几大元素
	- `{event}` 触发事件
	- `{pattern}` 文件模式
	- `{cmd}` 具体执行命令
	- `{augroup}` 命令组
- 自动命令可以新建、删除、列表、查询
- 还可以强制执行
- 有这个我们可以
    - 针对每种不同的文件的类型
    - 定义相应的触发事件
    - 然后执行各种各样的命令
    - 方便操作
- 不过关于文件类型的高亮显示还是没有讲的特别清楚
    - 为什么`public`在`java`文件里面就可以改变颜色呢？？🤔
- 下次再说！





