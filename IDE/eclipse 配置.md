# eclipse 配置

[TOC]

## 快捷键

| 功能           | 快捷键                                              | 参考资料                                                     |
| -------------- | --------------------------------------------------- | ------------------------------------------------------------ |
| 代码格式化     | `ctrl` + `shift` + `f`                              |                                                              |
| 注释代码       | `ctrl` + `/`   或   `ctrl` + `shift` + `c`          | [eclipse中java的单行注释、多行注释、文档注释的快捷键](https://blog.csdn.net/qq_41101701/article/details/81866705) |
| 倒入 jar       | `ctrl`+`shift`+`o`                                  | [eclipse导入类中所需要的jar包的快捷键](https://blog.csdn.net/wzn1054162229/article/details/80703560) |
| 类名称联想     | `alt`+`/`                                           |                                                              |
| 调用关系       | `Ctrl`+`Alt`+`H`                                    |                                                              |
| 字母大小写转换 | `ctrl`+`shift`+`x` 转大写 `ctrl`+`shift`+`y` 转小写 | [eclipse中字母大小写转换快捷键](https://blog.csdn.net/ocean20/article/details/2447157) |
|                |                                                     |                                                              |



## 配置

### 替换 tab 为空格

[Eclipse 4个空格替换Tab设置方法](https://blog.csdn.net/su749520/article/details/78662032)

> windows > preferences 
>
> + general > editors > text editors
>   + insert spaces for tabs
>
> + Java > Code style > Formatter
>   + Indentation > tab policy > space only
> + XML > XML File > Editor
>   + Indent using spaces
>   + indentation size: 2

### activiti

[Activiti --- Eclipse插件安装](https://blog.csdn.net/ka_ka314/article/details/79431197)

> + Help ---> Install new SoftWare
> + http://activiti.org/designer/update/

[eclipse中添加插件的时候经常会遇到 读取不到的报错情况Unable to read repository at http:](https://blog.csdn.net/xiaofanren1111/article/details/80997991)

> + 删除eclipse下面的缓存文件夹及文件  \eclipse\p2\org.eclipse.equinox.p2.repository\cache
> + 进入eclipse中，windows > Preferences -> Install Update -> Available Software Sites => select the entry
> + 选中需要的链接，点击"Reload"
> + 重启 eclipse

## STS (spring tools)

[eclipse使用插件快速搭建新的springboot项目](https://jingyan.baidu.com/article/e73e26c06d31b924adb6a704.html) -- 这种方法好像有问题，安装了 spring tools 4 以后，无法 new spring boot 项目

> + help > eclipse marketplace
> + 查找 **STS**
>   + Spring Tools 3 Add-On
>   + Spring Tools 4 - for Spring Boot

[github/spring-projects/sts4/wiki](https://github.com/spring-projects/sts4/wiki)

> eclipse 2019-6 (4.12) 》help 》 Install New Software 》 https://download.springsource.com/release/TOOLS/sts4/update/e4.12/

### lombok

[install lombok on Eclipse, Spring Tool Suite, (Red Hat) JBoss Developer Studio, MyEclipse](https://www.projectlombok.org/setup/eclipse)

## 问题排除

### eclipse tomcat unable to start within 45 seconds

[eclipse unable to start within 45 seconds](https://jingyan.baidu.com/article/6525d4b150cf72ac7c2e9441.html)

### tomcat 输入日志太短

[【Tomcat】 Eclipse 中 Tomcat 控制台日志长度修改](https://blog.csdn.net/qq934235475/article/details/84393631)

### properties 展示 unicode 

[[Eclipse] - Unicode properties editor](https://www.cnblogs.com/HD/p/4073221.html)

eclipse中的：help -> Install New SoftwareWork with

地址：http://propedit.sourceforge.jp/eclipse/updates/选择安装propertiesEditor最新版本。

重启eclipse即可

### 修改字体

[eclipse怎么设置字体大小](https://jingyan.baidu.com/article/f96699bb9442f3894e3c1b15.html)

### ubuntu 增加eclipse桌面快捷方式

[ubuntu 增加eclipse桌面快捷方式](https://blog.csdn.net/wuzuyu365/article/details/51527633)， 摘要如下：

```sh
sudo gedit /usr/share/applications/eclipse.desktop

# 然后在弹出的文件中输入
[Desktop Entry]
Encoding=UTF-8
Name=Eclipse Platform
Comment=Eclipse IDE
Exec=/home/siyu/softwares/eclipse/eclipse
Icon=/home/siyu/softwares/eclipse/icon.xpm
Terminal=false
StartupNotify=true
Type=Application
Categories=Application;Development;
```

然后，进入到目录 `/usr/share/applications` 复制 `eclipse.desktop` 到桌面即可

注意：

在 `/usr/share/applications/` 创建快捷方式，意味着在 `search`  中可以被搜索到

