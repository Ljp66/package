# `package`制作

## `module`与`package`

* 一个`module`常常对应一个`.py`文件
* `package`对应一个文件夹，里面有子文件夹，有`.py`文件

## `import`使用

1. 同级目录

```
- src
    |-- mod1.py
    |-- index.py
```
使用方式如下：
```python
from mod1 import ...
import mod1
```

2. 调用子目录

```
- src
    |--package
        |--mod1.py
        |--mod2.py
    |--index.py
```
使用方式如下：
```python
from package import mod1
import package.mod1
```

## 基本制作

1. 首先按照规定组织文件，之后创建`setup.py`文件。`setup.py`是`setuptools`的构建脚本。它告诉`setuptools`你的包（例如名称和版本）以及要包含的代码文件。

打开`setup.py`并输入以下内容：
```python
import setuptools

with open("README.md", "r", encoding="utf8") as fh:
    long_description = fh.read()

setuptools.setup(
    name="example-pkg-your-username",
    version="0.0.1",
    author="Example Author",
    author_email="author@example.com",
    description="A small example package",
    long_description=long_description,
    long_description_content_type="text/markdown",
    url="https://github.com/pypa/sampleproject",
    packages=setuptools.find_packages(),
    classifiers=[
    "Programming Language :: Python :: 3",
    "License :: OSI Approved :: MIT License",
    "Operating System :: OS Independent"
    ]
)
```

* `name`是包的分发名称。它不能在`pypi.org`上使用。
* `version`是包版本，看`PEP 440`有关版本的更多详细信息。
* `author`并`author_email`用于识别包的作者。
* `description`是一个简短的，一句话的包的总结。
* `long_description`是包的详细说明。这显示在`Python Package Index`的包详细信息包中。在这种情况下，加载长描述`README.md`是一种常见模式。
* `long_description_content_type`告诉索引什么类型的标记用于长描述。在这种情况下，它是`Markdown`。
* `url`是项目主页的URL。对于许多项目，这只是一个指向`GitHub`，`GitLab`或类似代码托管服务的链接。
* `packages`是应包含在分发包中的所有`Python`导入包的列表。我们可以使用`find_packages()`自动发现所有包和子包，而不是手动列出每个包。
* `classifiers`告诉索引并点一些关于你的包的其他元数据。在这种情况下，该软件包仅与`Python 3`兼容，根据`MIT`许可证进行许可，并且与操作系统无关。您应始终至少包含您的软件包所使用的`Python`版本，软件包可用的许可证以及您的软件包将使用的操作系统。有关分类器的完整列表，请参阅 `https://pypi.org/classifiers/`。

2. 生成分发档案
```python
# 安装或更新依赖库
pip install -U setuptools
# 构建
python setup.py sdist bdist_wheel
```
此命令应输出大量文本，一旦完成，应在`dist`目录中生成两个文件

3. 上传到`pypi`
`Twine` 是用于在 `PyPI` 上发布 `Python` 软件包的实用程序。`Twine` 库可以帮助我们，在新建的项目或者已有的项目中，打包、上传二进制程序包或者源码包，以便于我们分发我们的应用程序。

```python
# 安装依赖库
pip install twine
# 上传包到pypi
twine upload dist/*
```
将软件包上传至`pypi`需要每次都输入用户名密码，这样很麻烦，一个简单地方法是将用户名密码写入`~/.pypirc`，
文件的内容如下：
```
[distutils]
index-servers = pypi

[pypi]
username = username
password = password
```
