---
title: Windows下libgo使用总结
date: 2016-10-11 22:33:44
categories:
	- C/C++/Qt
tags:
	- libgo
description: "libgo使用总结"
copyright: true
---

# Windows下libgo使用总结

参考 [github libgo](https://github.com/yyzybb537/libgo)


下载 [cmake](https://cmake.org/download/) ，并安装（我下载 的 Windows win32-x86 Installer: Installer tool has changed. Uninstall CMake 3.4 or lower first!）

## 编译

1. clone
    `git clone https://github.com/yyzybb537/libgo.git`
    
2. CMake编译参数
  	禁止hook syscall，开启此选项后，网络io相关的syscall将恢复系统默认的行为，
  	协程中使用阻塞式网络io将可能真正阻塞线程，如无特殊需求请勿开启此选项

    + `cd F:\workspace_Qt\libgo\libgo`
    + `cmake .. -DDISABLE_HOOK=ON`

3. 下载Hook子模块
    + `cd F:\workspace_Qt\libgo-master`
    + `git submodule update --init --recursive`

4. 使用CMake构建工程文件
    + `cd F:\workspace_Qt\libgo-master\libgo`
    + vs2015(x86)：`cmake .. -G"Visual Studio 14 2015"`

## 使用

+ 使用时需要添加两个include目录：src和src/windows, 或将这两个目录下的头文件拷贝出来使用
+ 执行测试代码, 需要依赖 [boost库](http://www.boost.org/) 

    且在cmake参数中设置BOOST_ROOT:
  	`cmake .. -G"Visual Studio 14 2015" -DBOOST_ROOT="D:\\boost_1_63_0"`


## 其他

### 执行命令截屏

+ 到 libgo 目录下 下载Hook子模块 `git submodule update --init --recursive`
	```
	u@DESKTOP-T4PHSAB MINGW64 /f/workspace_Qt/libgo (master)
	$ git submodule update --init --recursive
	Submodule 'xhook' (https://github.com/XBased/xhook.git) registered for path 'thi                                                                                                                                                 rd_party/xhook'
	Cloning into 'F:/workspace_Qt/libgo/third_party/xhook'...
	Submodule path 'third_party/xhook': checked out 'e18c450541892212ca4f11dc91fa269fabf9646f'
	```

+ 到 libgo/libgo 目录下 设置 CMake编译参数 `cmake .. -DDISABLE_HOOK=ON`
	```
	u@DESKTOP-T4PHSAB MINGW64 /f/workspace_Qt/libgo (master)
	$ ls
	boost.cmake     libgo/   README.md  third_party/  vs_proj/
	CMakeLists.txt  LICENSE  test/      tutorial/

	u@DESKTOP-T4PHSAB MINGW64 /f/workspace_Qt/libgo (master)
	$ cd libgo

	u@DESKTOP-T4PHSAB MINGW64 /f/workspace_Qt/libgo/libgo (master)
	$ cmake .. -DDISABLE_HOOK=ON
	-- Building for: Visual Studio 14 2015
	-- The C compiler identification is MSVC 19.0.24215.1
	-- The CXX compiler identification is MSVC 19.0.24215.1
	-- Check for working C compiler: C:/Program Files (x86)/Microsoft Visual Studio 14.0/VC/bin/cl.exe
	-- Check for working C compiler: C:/Program Files (x86)/Microsoft Visual Studio 14.0/VC/bin/cl.exe -- works
	-- Detecting C compiler ABI info
	-- Detecting C compiler ABI info - done
	-- Check for working CXX compiler: C:/Program Files (x86)/Microsoft Visual Studio 14.0/VC/bin/cl.exe
	-- Check for working CXX compiler: C:/Program Files (x86)/Microsoft Visual Studio 14.0/VC/bin/cl.exe -- works
	-- Detecting CXX compiler ABI info
	-- Detecting CXX compiler ABI info - done
	-- Detecting CXX compile features
	-- Detecting CXX compile features - done
	------------ Options -------------
	  CMAKE_BUILD_TYPE: RELEASE
	  BOOST_ROOT:
	  layer_context: fiber
	  use cares: no
	  use safe signal: no
	  single thread mode: no
	  enable_debugger: no
	  enable_hook: no
	  build_dynamic_lib: yes
	----------------------------------
	-------------- Env ---------------
	  CMAKE_SOURCE_DIR: F:/workspace_Qt/libgo
	  CMAKE_BINARY_DIR: F:/workspace_Qt/libgo/libgo
	----------------------------------
	------------ Cxx flags -------------
	  CMAKE_CXX_FLAGS_RELEASE: /MD /O2 /Ob2 /DNDEBUG /MD
	------------------------------------
	-- Configuring done
	-- Generating done
	-- Build files have been written to: F:/workspace_Qt/libgo/libgo
	```

+ 生成Visual Studio 2013/2015工程方法：

    1. 安装git
    2. 安装cmake, 并将cmake可执行文件加入系统目录
    3. 启动git bash, 并切换到 `F:\workspace_Qt\libgo\vs_proj` 目录
    4. 执行脚本  `./make_vs_projs.sh`
    5. 在当前目录下会生成不同版本的vs工程文件, 直接使用即可

    另外, 如果你想运行测试代码和教程代码, 需要安装boost,
    并且在执行脚本时给出boost的安装目录, 例如:
    `$ ./make_vs_projs.sh -DBOOST_ROOT="E:\\boost_1_61_0"`

+ 使用CMake构建工程文件 `cmake .. -G"Visual Studio 14 2015"`
	```
	u@DESKTOP-T4PHSAB MINGW64 /f/workspace_Qt/libgo/libgo (master)
	$ cmake .. -G"Visual Studio 14 2015"
	------------ Options -------------
	  CMAKE_BUILD_TYPE: RELEASE
	  BOOST_ROOT:
	  layer_context: fiber
	  use cares: no
	  use safe signal: no
	  single thread mode: no
	  enable_debugger: no
	  enable_hook: no
	  build_dynamic_lib: yes
	----------------------------------
	-------------- Env ---------------
	  CMAKE_SOURCE_DIR: F:/workspace_Qt/libgo
	  CMAKE_BINARY_DIR: F:/workspace_Qt/libgo/libgo
	----------------------------------
	------------ Cxx flags -------------
	  CMAKE_CXX_FLAGS_RELEASE: /MD /O2 /Ob2 /DNDEBUG /MD
	------------------------------------
	-- Configuring done
	-- Generating done
	-- Build files have been written to: F:/workspace_Qt/libgo/libgo
	```

+ 要执行测试代码, 需要依赖boost库. 且在cmake参数中设置BOOST_ROOT `cmake .. -G"Visual Studio 14 2015" -DBOOST_ROOT="D:\\boost_1_63_0"`
	```
	u@DESKTOP-T4PHSAB MINGW64 /f/workspace_Qt/libgo/libgo (master)
	$ cmake .. -G"Visual Studio 14 2015" -DBOOST_ROOT="D:\\boost_1_63_0"
	------------ Options -------------
	  CMAKE_BUILD_TYPE: RELEASE
	  BOOST_ROOT: D:\boost_1_63_0
	  layer_context: fiber
	  use cares: no
	  use safe signal: no
	  single thread mode: no
	  enable_debugger: no
	  enable_hook: no
	  build_dynamic_lib: yes
	----------------------------------
	-------------- Env ---------------
	  CMAKE_SOURCE_DIR: F:/workspace_Qt/libgo
	  CMAKE_BINARY_DIR: F:/workspace_Qt/libgo/libgo
	----------------------------------
	------------ Cxx flags -------------
	  CMAKE_CXX_FLAGS_RELEASE: /MD /O2 /Ob2 /DNDEBUG /MD
	------------------------------------
	CMake Error at D:/Program Files (x86)/CMake/share/cmake-3.8/Modules/FindBoost.cmake:905 (list):
	  Syntax error in cmake code at

	    D:/Program Files (x86)/CMake/share/cmake-3.8/Modules/FindBoost.cmake:905

	  when parsing string

	    D:\boost_1_63_0/lib${_arch_suffix}-msvc-14.0

	  Invalid escape sequence \b
	Call Stack (most recent call first):
	  D:/Program Files (x86)/CMake/share/cmake-3.8/Modules/FindBoost.cmake:1379 (_Boost_UPDATE_WINDOWS_LIBRARY_SEARCH_DIRS_WITH_PREBUILT_PATHS)
	  boost.cmake:7 (find_package)
	  test/gtest_unit/CMakeLists.txt:18 (include)


	-- Configuring incomplete, errors occurred!
	See also "F:/workspace_Qt/libgo/libgo/CMakeFiles/CMakeOutput.log".
	```
