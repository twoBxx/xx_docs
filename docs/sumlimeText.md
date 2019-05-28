# Sublime3插件配置
### 1.直接安装
> 安装Sublime text 3插件很方便，可以直接下载安装包解压缩到Packages目录（菜单->preferences->　　Browse Packages）。
### 2.使用Package Control组件安装
>按Ctrl+ ~(此符号为tab按键上面的按键) 调出console（注：避免热键冲突）
粘贴以下代码到命令行并回车：
```
import urllib.request,os; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); open(os.path.join(ipp, pf), 'wb').write(urllib.request.urlopen( 'http://sublime.wbond.net/' + pf.replace(' ','%20')).read())
```
### 3. 下载完成之后重启Sublime Text 3
### 4. 如果在Perferences->中看到package control这一项，则安装成功
### 5.用Package Control安装插件的方法：
>按下<font color=#0099ff>Ctrl+Shift+P</font>调出命令面板
输入install 调出 Install Package 选项并回车，然后在列表中选中要安装的插件。
注意：安装插件时保持网络畅通，避免插件由于网络原因奔溃
>#### html5
>#### JSFormat
>#### BracketHighlighter （代码匹配）
>#### Alignment （代码匹配  Ctrl+Alt+A）
>#### Ctags (函数跳转 Alt+点击 函数名称)
>#### Doc​Blockr(注释插件  使用方法见：http://www.cnblogs.com/huangtailang/p/4499988.html)
>#### SideBarEnhancements(侧栏右键功能增强 使用方法(参考链接内容)：http://www.w3cfuns.com/notes/13810/d9b9ed2fb80785dae88a5344ef0f30d4.html)

