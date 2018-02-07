---
title: pypi私有服务器
date: 2017-09-08 18:57:18
tags:
---
## pypi-server搭建
### [pypi官方说明](http://wiki.python.org/moin/PyPiImplementations)
### 1.基础安装和启动
```
$ pip install pypiserver
$ mkdir ~/pypi/packages
# copy some source packages or eggs to this directory
$ pypi-server -p 3141 ~/pypi/packages
$ pip install -i http://localhost:8001/simple/ ...
```
### 2.添加上传密码
```
$ pip install passlib
$ apt-get install apache2-utils     # Mac OS不用安装
$ htpasswd -sc ~/pypi/.htaccess user   # 回车后会提示输入密码，输入123
```
### 3.启动pypi server时加载密钥文件
```
pypi-server -p 3141 -P ~/pypi/.htaccess ./packages
```
### 4.在用户根目录下创建.pypirc文件
```
[distutils]
index-servers =
  privatepypi

[privatepypi]
repository:http://127.0.0.1:3141
username:user
password:123
```
### 5.上传package
```
python setup.py sdist upload -r privatepypi
```
### 6.下载package
```
$ pip install -i http://localhost:3134/simple/ package-name
```
### 7.setup.py文件示例
```
import sys
import importlib
from setuptools import setup, find_packages


PROJECT = 'libs'
requires = []
# 读取requirements.txt文件，获得需要安装基础包
with open('requirements.txt') as f:
    for line in f.readlines():
        requires.append(line.strip())

# 打包时替换version.py文件
if sys.argv[1] == 'sdist':
    version_py = importlib.import_module('{project}.version'.format(project=PROJECT))
    version_py.update_version_file('{project}/version.py'.format(project=PROJECT))
# reload version模块，确保在替换version.py文件后获得最新的version号
version = reload(importlib.import_module('{pro}.version'.format(pro=PROJECT))).get_version()

with open('README.md', 'r') as f:
    readme = f.read()

setup(
    name='libs',
    version=version,
    description='libs is the common lib for Django project',
    long_description=readme,
    author='xxx',
    author_email='xxx@gmail.com',
    url='',
    packages=find_packages(),
    include_package_data=True,
    install_requires=requires,
    test_suite='nose.collector',
    license='All rights reserved by xxx.',
    zip_safe=False,
    classifiers=(
        'Development Status :: 5 - Production/Stable',
        'Intended Audience :: Developers',
        'Natural Language :: English',
        'Programming Language :: Python',
        'Programming Language :: Python :: 2.7',
    ),
)

```
### [setup.py官方文档](https://docs.python.org/2/distutils/setupscript.html)

## 使用supervisor管理pypi-server
### 1.生成run-pypi.sh文件，supervisor执行命令文件
```
#!/bin/sh
# 启动virtualenv
. ~/pypi/pypienv/bin/activate
exec pypi-server -p 3141 ~/pypi/packages
```
### 2.生成supervisor配置文件，/etc/supervisor/conf.d/pypi-server.conf
```
[program:pypi-server]
  directory=~/pypi/
  command=sh run-pypi.sh
  autostart=true
  autorestart=true
  redirect_stderr=true
```
### 3.启动和停止supervisor
```
$ sudo /etc/init.d/supervisor start
$ sudo /etc/init.d/supervisor stop
```
## 公有云pypi
### 1.将包的相关信息, 根据PyPI账户的注册信息, 在PyPI主页上注册。
```
$ python setup.py register -r pypi
```
### 2.将包的源代码内容打包上传。
```
$ python setup.py sdist upload -r pypi
```
