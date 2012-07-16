# tmux 使用笔记  
- category: code  
- date: 2012-02-27

---------------

#### 什么是tmux？
tmux 的[维基百科链接](http://en.wikipedia.org/wiki/Tmux)，想要详细了解的筒子请移步。  
简单来说，tmux 可以通过一个 ssh 会话建立多个窗口，每个窗口各自运行不同的程序，互不打扰。对于喜欢YY自己是运维的我来说这玩意还是挺有用的。  
和 tmux 类似的软件还有 [screen](http://www.gnu.org/software/screen/)。

#### 安装tmux
tmux 的[官方网站链接](http://tmux.sourceforge.net/)，其实各大 linux、bad 发行版的包管理工具的源中应该都已经存在了，可以很方便的安装。

#### tmux的窗口
tmux 可以分为会话（session）、窗口（window）、窗格（panes）：  
可以启动多个tmux会话，每个会话可以建立多个窗口，每个窗口可以建立多个窗格。

#### tmux的主要快捷键
`C-b ?` 显示快捷键帮助  
`C-b t` 显示时钟  

###### tmux 和主 shell 之间的切换
`C-b d` 返回主shell，tmux依然在后台运行  
`tmux attach` 恢复tmux

###### tmux会话操作
在命令行直接 `tmux` 建立会话  
`C-b s` 以菜单方式显示和选择会话

###### tmux窗口操作
`C-b c` 创建新窗口  
`C-b w` 以菜单方式显示和选择窗口  
`C-b n` 后一个窗口  
`C-b p` 前一个窗口  
`C-b l` 最后使用的窗口  
`C-b 0~9` 按窗口编号选择窗口  
`C-b "` 横向分隔窗口  
`C-b %` 纵向分隔窗口  
`C-b ,` 重命名当前窗口  
`C-b &` 退出窗口(`exit` 也是可以的)  

###### tmux窗格操作
`C-b 上下左右` 窗格之间切换  
`C-b o` 跳到下一个窗格  
`C-b q` 显示窗格编号  
`C-b 空格` 使用下一个内置窗格布局  
`C-b C-o` 调换窗口位置  
