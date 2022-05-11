---
show: step
version: 1.0
enable_checker: true
---

# 高亮细节 highight

## 回忆
- 这个自动命令 `autocmd` 还是很方便的
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

### 颜色方案
```
git clone http://gitee.com/overmind1980/oeasyvim.git
cd oeasyvim
vi oeasy.java
```

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210802-1627907966621)

- 字符串是棕色的
- `public`、`void`是绿色的
- `:colorscheme murphy`

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210802-1627908044723)

- 我们发现在murphy颜色方案下
- 字符串不变色了
- `public`、`void`也不变色了

### 查看配色
- `:hi`

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210802-1627908193897)

- 查看不到为什么`public`没有改变颜色


### 语法文件syntax
- 我要找到关于java语法定义Syntax的文件`java.vim`
- 于是我搜索一下
- `sudo find / -name java.vim`

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210802-1627908359160)

- 这三个java.vim都位于系统的配置文件夹中
	- ftplugin 对应java的系统插件
		- ft 是 filetype
		- plugin 是 插件 
        - /usr/share/vim/vim81/ftplugin/ 对应系统的插件
		- ~/.vim/ftplugin 对于用户的插件
	- indent 对应java的系统缩进
	- syntax 对应java的系统语法
- `ls /usr/share/vim/vim81/syntax/java.vim`

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210802-1627908609574)

- `vi /usr/share/vim/vim81/syntax/java.vim`

### 查找String
- `/String`
	- 找到41个匹配
- `/\<String\>`
	- 找到3个匹配

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210802-1627909795605)

- 大概率String本身就是那个高亮元素
- `:q` 退回到`oeasy.java`

### 设置高亮

- `:colorscheme murphy`
- `:hi String ctermfg=brown`
	- hi 是highlight
	- String是需要定义颜色的组名称
	- cterm是color terminal foreground
	- brown 是棕色

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210802-1627909966231)

- String的高亮设置成功
- 我们来分析一下什么是String

### String的定义

- `:e /usr/share/vim/vim81/syntax/java.vim`
- `/\<String\>`

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210802-1627910478613)

- `/javaString`
- `/\<javaString\>`
- `:set wrap`

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210802-1627910401270)

- 大致是定义了一个region区域javaString
- 然后和让javaString连接到了String上
- 在syntax/java.vim定义了各种区域region
	- 有些是关键字keywords
	- 有些是字符串String
- 不过具体给这些定义好的区域上什么颜色
- 还是得配色文件来完成
	- 比如murphy.vim
- 在`murphy.vim`目前没有定义String这个区域的颜色
- 但是我们在最后强行`hi String ctermfg=brown`了
- 难道`murphy.vim`就真没有`String`区域的颜色定义么？

### murphy.vim

- `sudo find / -name murphy.vim`
- `vi /usr/share/vim/vim81/colors/murphy.vim`
- `/String`

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210802-1627910767648)

- `sudo vi /usr/share/vim/vim81/colors/murphy.vim`
- 在最末一行加上一句

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210802-1627910857436)

- 保存murphy.vim并退出
- `vi oeasy.java` 再切换到murphy配色
- 设置成功
- 我们再来看看 `public` 等关键字

### 找到public
- `vi /usr/share/vim/vim81/syntax/java.vim`
- `/public`

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210802-1627911093095)

- public、protected这些关键字都已经被定义到了一个javaScopeDecl里面
- 鼠标放到 `javaScopeDecl` 上 按下<kbd>*</kbd>

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210802-1627911211862)

- 我们可以发现`javaScopeDecl`被定义为`javaStorageClass`
- `javaStorageClass`被定义为`StorageClass`

### 尝试修改
- 回到`oeasy.java`
- `:hi StorageClass=cyan`

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210802-1627911673601)

- 修改成功
- `sudo vi /usr/share/vim/vim81/colors/murphy.vim`
- 在最后添加 `:hi StorageClass=cyan`
- 然后再打开`oeasy.java`
- 试验成功！！！



## 总结

- 我们这次研究了一下配色方案
    - murphy虽然配色好看
    - 但是对于java的支持并不好
    - 我们对于murphy进行了修改
    - 增加了`String`、`StorageClass`颜色的定义
    - 整体思路是在syntax中定义组
    - 然后在配色方案中对于组进行颜色设置
- 语法高亮总共分成两块
	- 核心语法引擎
		- 描述各种高亮分组的匹配模式特征
	- 高亮分组样式定义
		- 描述具体某个高亮分组具体是什么颜色
- 可是syntax中如何定义组的呢？🤔
- 下次再说！


