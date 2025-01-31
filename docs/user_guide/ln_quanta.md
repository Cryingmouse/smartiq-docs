# **LN-Quanta**

## **What is Ln-Quanta**
It provides the library to ***abstract hardware and software layers*** used by Magnascale™ Software application. It also
provides some commands to help developers to work with the library.

## **Project layout**

    docs/
        index.md  # The documentation homepage.
        ...       # Other markdown pages, images and other files.
    lnquanta  - - - - - 
        |__ commandd - - - - - - 命令行接口目�?
        |__ lib - - - - - - 通用工具封装
        |  |__ hook.py  - - 插件�?插件装饰�?
        |
        |__ driver  - - - - 对南向接口的直接封装:数据面命令行、OS命令、OS配置文件、TFS cpython接口、RGW http接口、TDS 类库接口
        |  |__ __init__.py  插件化：执行环境加载
        |  |__ context.py - 插件化：环境变量定义与获�?
        |
        |__ interface - - - 北向接口声明和复合接口封�?
        |  |__ __init__.py  插件化：插件注册
        |
        |__ main.py - - - - 命令行入�?
    mkdocs.yml    # The configuration file.
    
