---
title: Windows下Qt开发环境搭建及mysql乱码处理
date: 2016-10-11 22:33:44
categories:
	- C/C++/Qt
tags:
	- Qt
	- MySql
description: "Windows下Qt开发环境搭建及mysql乱码处理"
copyright: true
---

# window10 + Qt5.8 + MySql

我都使用的32位，你可以根据自己需要下载，（Mingw是32位）

+ Mingw
+ Visual Studio

## Mingw

到 [Qt官网](https://www.qt.io/download/) 下载 `qt-opensource-windows-x86-mingw530-5.8.0.exe`

傻瓜式安装（next-->next-->finish）
默认安装配置了 `Qt Creator`

### 配置

将

> `D:\ProgramFiles\Qt\Qt5.8.0\Tools\mingw530_32\bin`
>
> `D:\ProgramFiles\Qt\Qt5.8.0\5.8\mingw53_32\bin` 

配置到 `PATH`

### 问题

安装报错
```
Installer Error
Error during installation process(qt.tools.perl):
Execution failed: Could not start: "... QtPath/strawberry-perl-5.22.1.3-32bit.msi /quiet"(Process failed to start: No such file or directory)
```
解决方法
先运行 `strawberry-perl-5.22.1.3-32bit.msi`

## Visual Studio

### 下载

+ 到 [Visual Studio](https://www.visualstudio.com/zh-hans/downloads/) 下载 Visual Studio 2015 (我下载的 vs2015 3G,其他版本太大了7G)
+ 到 [Qt官网](https://www.qt.io/download/) 下载 `qt-opensource-windows-x86-msvc2015-5.8.0.exe` 这个msvc2015已经包含了 `qt-creator-opensource-windows-x86-4.2.1.exe`
+ 到 [Windows 调试工具](https://developer.microsoft.com/zh-cn/windows/hardware/download-windbg) 下载 Windows 10 调试工具 (WinDbg)

### 安装

+ 傻瓜式安装（next-->next-->finish）VS 和 Qt

    **Visual Studio 需要选择 `Visual C++`**

    Qt Creator --- 选项 --- 构建和运行 --- 编译器 可以自动识别 `Visual C++ Compiler MSVC`

    Qt Creator --- 选项 --- 构建和运行 --- 构建套件 --- 调试器 = **None**

+ 安装 WinDbg 调试器

    Qt Creator --- 选项 --- 构建和运行 --- Debuggers

可以自动识别 `CDB`

#### Qt Creator
此时就可以使用 Qt Creator 开发了

**自动补全快捷键** `工具` -> `选项` -> `环境` -> `键盘`
找到 `TextEdit` - `Complete this` 重新设置快捷键 我使用 `Alt + /` 默认是 `Ctrl + 空格`

#### Visual Studio 2015

+ 打开 `Visual Studio 2015` --- 工具 --- 扩展和更新 --- 联机搜索 Qt 
+ 安装 `Qt Visual Studio Tools` 后，重启，菜单栏出现 `Qt VS Tools`
+ 打开 `Qt VS Tools` --- `Qt Options` --- `Qt Version` --- `Add` --- `Path = D:\ProgramFiles\Qt\Qt5.8.0\5.8\msvc2015` 
+ 此时就可以使用 Visual Studio 2015 开发了

## 使用Mysql
将 Mysql 下 lib 文件夹中的 `*.lib *.dll` 复制到 `Qt/mingw/bin` 或 `Qt/msvc2015/bin` 下

在 `项目.pro` 文件中 添加 `QT += sql`

```
    // mysql 数据库使用
    QSqlDatabase db=QSqlDatabase::addDatabase("QMYSQL"); //QMYSQL代表是MYSQL数据库
    db.setHostName("127.0.0.1");     //数据库地址
    db.setUserName("root");          //用户名称
    db.setPassword("123");      //用户密码
    db.setDatabaseName("mydbname");      //数据库名称
    if(db.open()) {
        qDebug() << "database open suc";

        QSqlQuery treeQuery(QSqlDatabase::database("treeDatas"));
        // 默认只获取所有省
        QString sqlStr = "SELECT base_area.code area_code,base_area.name area_name FROM base_area WHERE 1=1 AND LENGTH(CODE) = 2 ORDER BY CODE ;";
        // 查询
        if (treeQuery.exec(sqlStr)){
            // 数据封装
            while (treeQuery.next()){
                QString code = treeQuery.value("AREA_CODE").toString();
                QString name = treeQuery.value("AREA_NAME").toString();
                qDebug() << "++++++++++++++++++ " << name;
            }
        }
    }else{
        qDebug() << "database open faild";
    }
```

# `Qt5.* + Mysql5.*` 和 `Qt5.*+vs2015+mysql5.*` 读取数据库乱码

在这个坑里挖了 48h++ 所以特别说明一下，注意编码格式

## 统一编码

1. 设置数据库编码，utf8
2. Qt Create —— 工具 —— 选项 —— 文本编辑器 —— 行为 —— 文件编码 = UTF-8

## QTextCodec 设置

```
QTextCodec *codec = QTextCodec::codecForName("UTF-8");
QTextCodec::setCodecForLocale(codec);
```
