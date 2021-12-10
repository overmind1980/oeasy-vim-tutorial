---
show: step
version: 1.0
enable_checker: true
---

# 窗口window

## 回忆上次
- 我们这次了解了缓冲区buffer
- `ls`可以查看`buffer`
- 如下是`buffer`缓冲的一些`flag`
 	- `+`   有修改未保存内容      
	- `-`   可修改标签关闭
	- `=`   只读缓冲区
	- `a`   活跃缓冲区                                         
	- `u`   没有在列表里面的缓冲区
	- `h`   隐藏缓冲区
	- `x`   有读取错误的缓冲区
	- `%`   当前缓冲区
	- `#`   有`swap`替换文件的缓冲区

- 可以在`buffer`跳转
	- 可以通过`bn[ext]`,选择下一个`buffer`
	- 可以用`b[uffer1]`、`b2`进入指定的 `buffer`
	- 也可以用文件名，比如`b[uffer] oeasy.txt`
	- 可以用`bf[irst]`，打开第一个`buffer`
	- 可以用`bl[ast]`，打开最后一个`buffer`
- 不保存也可以跳转
	- 设置 `hidden`
- 跳转的时候自动保存
	- 设置 `autowrite`
- 可以用大写字母设置跨文件标记mark
	- `mA` 添加标记
	- `'A` 跳转到标记
- 关于强行跳转和保存退出
	- 强制退出当前窗口 `:q!`
	- 不保存强行退出所有`buffer` `:qall!`
	- 保存所有未保存的文件`:wall`
	- 全部保存并退出`:wqall`
- `vim`中打开的两个文件缓存`buffer`
- 可以放到两个窗口里面同时查看吗？🤔

### 查询手册

- 查一下手册`:h window`

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210808-1628393469466)

- 三者定位
	- 上次的 `buffer`缓存 是内存中的文件缓存
	- 这次的 `window`窗口 是打开 `buffer` 的一个视角
	- `tab` 标签是 `window`窗口的集合
- 我们可以
	- 通过`:sp[lit]`来上下切分窗口
	- 且切分了之后
	- 我们可以在一屏幕内同时看到第1行和第1000行
	- 切分出窗口之后可以用`:q`退出当前窗口
	- 这样就又回到单一窗口的状态

### 在此切分

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210204-1612419465415)

- `:sp[lit]`切分 窗口之后
	- 可以切换窗口
	- <kbd>ctrl-w</kbd> <kbd>ctrl-k</kbd>切换到当前窗口上面的窗口
	- <kbd>ctrl-w</kbd> <kbd>ctrl-j</kbd>切换到当前窗口下面的窗口
	- <kbd>ctrl-w</kbd> 
		- 意味着window
	- <kbd>ctrl-k</kbd>、<kbd>ctrl-j</kbd>
		- 意味着切换的方向
- 切分之后可以再次切分 `:sp[lit]`
	- 再次切换
	- <kbd>ctr-w</kbd>再<kbd>ctrl-k</kbd>
		- 切换到当前窗口上面的窗口
	- <kbd>ctrl-w</kbd>再<kbd>ctrl-j</kbd>
		- 切换到当前窗口下面的窗口

### 纵向切分

- 我们现在位于中间的一层
- `:vsp[lit]`
	- v的意思是 `vertical` 垂直方向
	- 于是就可以把这个 `window` 再垂直方向上进行切分
- 在左右切换的时候，使用
	- <kbd>ctrl-w</kbd>再<kbd>ctrl-h</kbd>
		- 切换到当前窗口左面的窗口
	- <kbd>ctrl-w</kbd>再<kbd>ctrl-l</kbd>
		- 切换到当前窗口右面的窗口

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210204-1612419978345)

### 退出窗口

- 退出当前window窗口 
	- `:q`
- 强制退出当前window窗口不保存
	- `:q!`  
- 不保存强行退出所有
	- `buffer` `:qall!`
- 保存所有未保存的文件
	- `:wall`
- 全部保存并退出
	- `:wqall`

### 设置window显示与隐藏
- `:on[ly]`
	- 保留当前`window`，其余的都`hidden`
- `:hide`
	- 隐藏当前`window`
- 如果`:set autowrite`了
	- `hidden`的时候就自动保存

### 调整`高`度

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210808-1628408398548)

- 调整窗口 `window` 高度减小
	- 高度减小
		- <kbd>ctrl-w</kbd> <kbd>-</kbd>
	- 高度减小N
		- `:res[ize] -N` 	
- 调整窗口 `window` 高度增大
	- 高度增大
		- <kbd>ctrl-w</kbd> <kbd>+</kbd>
	- 高度增大N
		- `:res[ize] +N` 
- 设置当前窗口高度值为N
	- `:res[ize] N`
	- 或者直接在正常模式下
		- z15<kbd>cr</kbd>
- 总共是 `80*24`
	- 看着分配
- 窗口高度都统一
	- <kbd>ctrl-w</kbd>再<kbd>=</kbd> 

### 调整窗口`宽`度 

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210808-1628408944136)

- 调整窗口 `window` 宽度减小
	- 宽度减小
		- <kbd>ctrl-w</kbd> <kbd><</kbd>	
	- 窗口 `window` 宽度减小N
		- `:vertical res[ize] -N`
- 调整窗口 `window` 宽度增大
	- 宽度增大
		- <kbd>ctrl</kbd>+<kbd>w</kbd> <kbd>></kbd>
	- 宽度增大N
		- `:res[ize] +N` 
	- 设置当前窗口宽度值为N
		- `:vertical res[ize] N`
- 窗口高度宽度都统一
	- <kbd>ctrl-w</kbd>再<kbd>=</kbd> 
- 总共是 `80*24`
	- 还是得看着分配看着分配

### 状态横条

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210204-1612420149779)

- `:set laststatus=0`不用状态横条
- `:set laststatus=1`有多个窗口的时候，最底下有一个状态横条
- `:set laststatus=0`各窗口有各自的状态横条

### 快速终端

- 可以用`:terminal`或者`:ter`切换到命令模式
- 只有vim8以上的版本才支持
- 可以试试不？
	- 看起来是单独新建了一个进程
- 如何上下切换 窗口 `window` ?
	- 快捷键
- 如何切出 `shell` ?
	- 试试 `exit` 命令
	- 或者万能的退出快捷键<kbd>ctrl-d</kbd>

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210727-1627394618491)

- 想要纵向分屏出shell？
	- `:vertical terminal`
	- `:vert ter`

## 总结

- 我们这次了解了 窗口`window`
- 窗口是用来装 缓冲`buffer` 的
- `buffer` 是在内存里面载入的硬盘文件
- 窗口的切分
	- `:sp[lit]` 水平切分
	- `:vsp[lit]` 垂直切分
- 窗口的切换
	- <kbd>ctrl-w</kbd>再<kbd>ctrl-k</kbd>
		- 切换到当前窗口上面的窗口
	- <kbd>ctrl-w</kbd>再<kbd>ctrl-j</kbd>
		- 切换到当前窗口下面的窗口
	- <kbd>ctrl-w</kbd>再<kbd>ctrl-h</kbd>
		- 切换到当前窗口左面的窗口
	- <kbd>ctrl-w</kbd>再<kbd>ctrl-l</kbd>
		- 切换到当前窗口右面的窗口
- 窗口的隐藏和全屏
	- `:hid[e]`可以隐藏当前窗口
		- 隐藏的`window`中`buffer`不保存
		- 除非设置了`autowrite`
	- `:on[ly]`可以全屏当前窗口
		- 其他的窗口都进入`:hide`状态
- `'laststatus'` 可以设置状态横条
- `:terminal` 可以开启终端
- 这次主要就是`window`窗口,还挺方便
- 尤其多文件操作
- 这个还有什么可玩的吗？🤔
- 下次再说 👋