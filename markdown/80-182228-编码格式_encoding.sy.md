---
show: step
version: 1.0
enable_checker: true
---

# 文件格式 fileformat

## 回忆

- 上次我们了解了文件格式
	- unix
	- dos
	- mac
- 不同文件格式对应的换行符不同
    - 可以修改当前文件的文件格式
    - 也可以用二进制来观察
    - 不同文件格式下
    - 具体换行符的样子
- 为什么'6f'就对应字母`o`?
	- 因为这是ascii基本的编码设定
    - 可中文字符又是如何编码的呢？🤔

### 插入ascii字符

- 我们新建一个`oeasy.txt`
    - <kbd>i</kbd>进入插入模式
    - 输入<kbd>ctrl</kbd>+<kbd>v</kbd> 65
    - 输入<kbd>ctrl</kbd>+<kbd>v</kbd> 66
    - 输入<kbd>ctrl</kbd>+<kbd>v</kbd> 67
    - 输入<kbd>ctrl</kbd>+<kbd>v</kbd> 68
    - <kbd>esc</kbd> 退出
- 屏幕上可以见到插入了 `ABCD` 四个字符
- 这就是使用ascii具体编码来插入字符
- 那我能用其他的进制
- 比如八进制和十六进制来插入ascii字符么？
- `:h i_ctrl-v`


![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210731-1627693873823)


### 用其他进制插入ascii字符

- 使用八进制
	- 输入<kbd>ctrl</kbd>+<kbd>v</kbd> o117 
	- 输入<kbd>ctrl</kbd>+<kbd>v</kbd> o101
	- 输入<kbd>ctrl</kbd>+<kbd>v</kbd> o105
	- 输入<kbd>ctrl</kbd>+<kbd>v</kbd> o123
	- 输入<kbd>ctrl</kbd>+<kbd>v</kbd> o131
- 使用十六进制
	- 输入<kbd>ctrl</kbd>+<kbd>v</kbd> x41
- 能否用这个方式插入中文么？
- 先查一下

### 插入unicode字符

- 输入<kbd>ctrl</kbd>+<kbd>v</kbd> u4e00
    - 可以看到插入了中文字符
    - 具体二进制是如何存储的呢？
- `:.!xxd`

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210731-1627694993749)

- 为什么会是这样呢？

### 编码模式

- 那这个东西怎么具体存储和操作呢？
    - 这个unicode编码是0x4e00
    - 存储的时候需要避免和ascii中的前127个字符混淆
    - 需要用utf-8的方式存储
    - 状态栏上也可以看出此文件的编码格式

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20211222-1640160285599)

- 默认是utf-8

### utf-8 编码格式

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210228-1614485114416)

- 一(4E00) 属于第三行的范围

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210403-1617424477908)

- 所以
	- `e4b880`对应着`一`
	- `0a` 对应着 `<LF>`
		- 也就是linefeed

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210731-1627694993749)

- 完美对应上了
- 除了utf-8之外还有别的编码格式么？

### 查看当前语言和编码

- `:language`可以查看当前编码格式

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210731-1627695548480)

### 修改编码
- `:set encoding=unicode`
- 设置之后再观察变化

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20211222-1640160556231)

- 这个ucs-2就是utf-16
- ucs-4就是utf-32
- 上面的编码变成了4e00
- 还有什么其他编码么？

### 其他编码

- `:set encoding=latin1`
	- 修改之后，状态栏的中文不认识了
	- 屏幕的字符也变了模样
	- 其实他这是用`latin1`来解释`e4b880`

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20211222-1640160708311)

- 这个是拉丁字符的编码
- 我们再试试支持中文的

### gb2312

- `:set encoding=gb2312`
	- 换成中文gb2312
	- 文件存储形式没变
	- 解释方式变了
	- 我们也无法理解

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20211222-1640160802347)

- `:set encoding=utf-8`
	- 改回来了
	- 我们又可以看到`一`

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20211222-1640160850108)

- `:set encoding=gb2312`
	- 又来
	- 硬要说他是`gb2312`的编码
- 然后就保存
- 以上都发生在内存里面
- 保存到硬盘里面
- 如何控制他的编码格式呢？

### 文件编码格式
![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210731-1627697869181)

- `fenc`其实就是
	- `fileencoding`
	- `fileencoding` 和 `coding` 有什么不同呢？
- `encoding`
	- 是在屏幕显示字符时的所用编码格式
    - 是读取内容并放入内存缓冲buffer中的编码
- `fileencoding` 是保存文件时
    - 在硬盘上保存二进制文件的编码格式
    - 是硬盘上的编码
    - 我们用`utf-8`写下了`一`
	- 内存中对应`e4b880`
- `:set encoding=gb2312` 
	- 这是在用gb2312编码格式解码utf-8编码的二进制序列
	- 把`e4b880`理解为`涓~`并显示在屏幕上
- `:set fileencoding=gb2312` 
	- 根据gb2312
	- 把`涓~`在硬盘上写成`e4b880`
-  `vi oeasy.txt`
	- 根据utf-8  
	- 把`e4b880`理解为`一`并显示在屏幕上
- 如果我就想写一个gb2312的`一`到硬盘
- 怎么办呢？

### 使用gb2312
- `:set encoding=gb2312`
    - 之后在插入模式下输入一
- `:set fileencoding=gb2312` 
    - 注意状态栏的变化
- 这是要在内存中和硬盘上都使用gb2312编码格式

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20210731-1627698696768)

- `:%!xxd`
	- 这次内存中的`d2bb`在`gb2312`格式下被理解为`一`
- `:%!xxd -r`
    - 恢复回来
- `:w! o2z.txt`
	- 这次把用`gb2312`编码`一`，也就是`d2bb`
	- 写到了硬盘里

### 打开文件

- `vi o2z.txt`
- 只能看到一个`h`
	- 好多不可见
	- 这是在用utf-8编码格式解释gb2312编码后的内容
- 因为是用`utf-8`解码`gb2312`编码的文件
- `:%!xxd `
	-  可以看到从硬盘读到缓存的内容为`d2bb`
-  `%!xxd -r`
    - 恢复回来
-  `:set encoding=gb2312`
	- 这次可以看到用`gb2312`解码`d2bb`为`一` 
- 那如果我们都用utf-8来编码呢？

### utf-8编码
- `:set encoding=utf-8`
- `:set fileencoding=utf-8`
- 这样都是utf-8编码的内容
- 其实还可以设置在内存中`unicode`
- 这样更省内存空间
- 但如果我我想要把文件编码设置为ascii
- 并保存会如何呢？


### E513
![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20211222-1640161644144)

- 出现了错误E513
- 因为ascii每个字节首位必须是0
- 所以这个汉字`一`无法保存
- 所以这个时候如果把filecoding置成空会如何呢？


### 修改

- `:set fileencoding=`
- `:set fileencoding?`
- `:set encoding?`
- 这个时候`encoding`是`ucs-2`
- 这是什么意思？

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20211222-1640162295146)

### 编码含义

- `help encoding-name`

![图片描述](https://doc.shiyanlou.com/courses/uid1190679-20211222-1640162325897)

- 看起来就是unicode编码
- 那其实我可以把当前文档保存为
	- utf-8 `:set fileencoding=utf-8`
	- gb2312 `:set fileencoding=gb2312`
- 当然反过来
- 我如果打开一个文件
- 可以通过
	- `:set encoding=utf-8`或者
	- `:set encoding=gb2312`
	- 来设置编码确保文件正常观看

## 总结

- 这次我们了解了编码格式
	- 屏幕显示的encoding
	- 文件保存的fileencoding
- 不能搞乱了
- 一般用什么编的就用什么解
- 解铃还须系铃人
- 打开不正确的话，就要切到正确的上面
- 保存成任何自己想要的编码方式
- vim还有什么能帮助我们快速操作的方法么？🤔
- 下次再说！✋



