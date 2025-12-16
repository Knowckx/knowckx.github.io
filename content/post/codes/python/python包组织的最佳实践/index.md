+++
title = "Python包导出最佳实践与VSCode智能提示修复"
slug = "python-package-export-best-practices-vscode-fix-import-suggestions"
description = "探讨Python中利用__init__.py进行包导出的最佳实践，并解决VSCode (Pylance) 默认无法给出正确的自动导入提示(import suggestions)的问题。"
categories = ["python", "vscode"]
tags = ["python", "Best Practices", "VS Code", "Pylance", "import suggestions"]
keywords = ["python", "vscode", "pylance", "import suggestions", "__init__.py", "__all__", "includeAliasesFromUserFiles"]
weight = 3
date = "2025-12-16 22:01:17+0800"
+++

# 中文

## 长话短说

假如你使用`vscode`写`python`，在导入一个模块时希望 vscode 给出的导包建议，  
是建议从包的根路径（`__init__.py`）导入类，而不是从深层文件路径导入，请记住修改以下配置：  
在设置里搜索`includeAliasesFromUserFiles`，设置为`true`

```
keywords: "python", "import suggestions", "__init__.py", "__all__"
```

## 背景

今天在实践 Python 包导出的最佳方式时，遇到了 VS Code 无法正确给出“短路径”导入提示的问题。
研究了一个小时才解决，这里记录一下，希望能帮大家避坑。（这个问题连 Gemini 3.0 和 Claude 都没有第一时间给出正确原因，大多以为是路径配置问题）。）


### Python组织包导出的最佳实践


1. 假如我们有以下的项目结构，其中`pkg_a`是我们自己写的包
```plaintext
📁 /project_root
├── 📁 pkg_a
│   ├── 📄 __init__.py
│   └── 📄 user_define.py
└── 📄 start.py
```

2. 我们在`user_define.py`中定义了一个类型

```python
# pkg_a/user_define.py

class MyUser():
    name: str = ""
```

3. 通过`__init__.py`来定义导出  
在这个文件夹的`__init__.py`中，我们可以通过`__all__`来定义这个包有下面什么内容是对外导出的。  
这是许多优秀开源项目（如 Requests, Pandas）的标准做法。

```python
# pkg_a/__init__.py
from pkg_a.user_define import MyUser


__all__ = [
    "MyUser"
]
```


4. 当我们想引用这个`MyUser`定义时，导入定义会有两种方式，目前社区的最佳实践是更推荐第二种导入方式

```python
# start.py

# Method 1 ❌-> Use file import
# from pkg_a.user_define import MyUser

# Method 2 ✅ -> Using the Package Import 
from pkg_a import MyUser

user_tom :MyUser | None = None
```

**为什么第二种方式更好呢**

1. 更好的封装/抽象  
包的调用者无需了解包内部的文件结构。  
假如你稍后将 `user_define.py` 重命名为 `models.py`，只需要更改 `__init__.py` 中的导入语句 ， 外部的调用方`start.py`不用修改也不会出错，他是无感知的。

2. 更简洁的导入 API  
更短的导入语句通常更容易阅读。  
`from pkg_a import MyUser` 比 ` from pkg_a.sub_folder.user_files.definitions import MyUser` 更简洁 。

3. 精确控制导出内容  
通过 __all__，你可以严格控制当用户使用 from pkg_a import * 时会污染命名空间的内容。

### 问题：VS Code 默认不支持提示

这就引出了我今天遇到的问题：VS Code 无法提示文件级导入路径。  
当我输入 MyUser 尝试自动导入时，IDE 往往建议 from pkg_a.user_define import MyUser，而不是我希望的 from pkg_a import MyUser。  

然后我排查了一圈，发现这是 VS Code 的 Python 语言服务器 Pylance 的默认行为。  
配置项 python.analysis.includeAliasesFromUserFiles 默认是 false（为了性能考虑，默认不索引用户文件中的别名导出）。

解决方法：
将该配置改为 true 后，VS Code 就能理解你在 __init__.py 中做的“别名导出”是为了给外部使用的，从而正确地给出简短的包导入建议。

又花了我1个多小时，希望对你们有用！

---


# English

## Best Practices for Python Package Exports & Fixing VS Code Intellisense

### TL;DR

If you use `VS Code` for Python development and want the IDE to suggest imports from the package root (`__init__.py`) instead of the specific file path, you need to tweak a setting.

Search for **`includeAliasesFromUserFiles`** in your settings (or set `python.analysis.includeAliasesFromUserFiles` in `settings.json`) and set it to **`true`**.

### Background

While practicing the best way to organize Python package exports today, I encountered an issue where VS Code failed to suggest the "clean" import path. I spent an hour troubleshooting this, so I'm documenting it here to verify it works for you. (Even AI assistants like Gemini 3.0 and Claude didn't pinpoint the exact configuration cause immediately).

### Best Practices for Python Package Organization

1. Project Structure
Suppose we have the following structure, where `pkg_a` is our custom package:

```text
📁 /project_root
├── 📁 pkg_a
│   ├── 📄 __init__.py
│   └── 📄 user_define.py
└── 📄 start.py
```

1. Defining a Class
We define a class inside `user_define.py`:

```python
# pkg_a/user_define.py

class MyUser:
    name: str = ""
```

1. Exporting via `__init__.py`
In the package's `__init__.py`, we expose the content using `__all__`. This pattern is widely used in open-source projects.

```python
# pkg_a/__init__.py
from pkg_a.user_define import MyUser

__all__ = [
    "MyUser"
]
```

1. Importing the Class
When importing `MyUser` in `start.py`, there are two methods. The community **strongly recommends Method 2**.

```python
# start.py

# Method 1 ❌ -> File Import (Implementation detail leak)
# from pkg_a.user_define import MyUser

# Method 2 ✅ -> Package Import (Facade Pattern)
from pkg_a import MyUser

user_tom: MyUser | None = None
```

**Why is Method 2 better?**

1.  **Better Encapsulation/Abstraction**
    The consumer of the package doesn't need to know the internal file structure.
    If you later rename `user_define.py` to `models.py`, you only need to update the import in `pkg_a/__init__.py`. The external code in `start.py` remains unchanged and unbroken.

2.  **Cleaner Import API**
    Shorter import statements are generally more readable.
    `from pkg_a import MyUser` is much cleaner than `from pkg_a.sub_folder.user_files.definitions import MyUser`.

3.  **Control Over Exports**
    It allows you to control exactly what is exported when someone uses `from pkg_a import *` via the `__all__` list.

### The Issue: VS Code Default Behavior

Here is the problem I faced: **VS Code defaults to suggesting the file import path.**

When I typed `MyUser` hoping for an auto-import, the IDE suggested `from pkg_a.user_define import ...` instead of the cleaner package import.

It turns out this is a setting in **Pylance**. The setting `python.analysis.includeAliasesFromUserFiles` defaults to `false` (likely for performance reasons).

**The Fix:**
Setting this to `true` allows Pylance to recognize that the alias created in `__init__.py` is intended for public use, enabling it to suggest the correct, shorter import path.






















