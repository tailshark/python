# python3操作数据库

centos7为例

[TOC]

## 创建虚拟环境

1、**安装`virtualenv`：**

如果尚未安装 `virtualenv`，您可以使用以下命令进行安装：

```shell
sudo pip3 install virtualenv
```

2、**创建虚拟环境：**

```shell
virtualenv myenv
```

3、**激活虚拟环境：**

```shell
source myenv/bin/activate
```

4、**在虚拟环境中安装依赖项：**

现在，您可以使用`pip`在虚拟环境中安装依赖项，而不会影响全局Python环境：

```shell
pip install package_name
```

5、**退出虚拟环境：**

当您完成工作时，可以通过以下命令退出虚拟环境：

```shell
deactivate
```

这将恢复到全局Python环境。

## **安装 OpenSSL 开发库**

 确保在你的系统上安装了 OpenSSL 开发库。在 CentOS 上，你可以运行以下命令来安装

```shell
sudo yum install openssl-devel
```

**检查 Python 版本：** 确保你的 Python 版本支持 SSL。运行以下命令来检查：

```shell
python3 --version
```

**更新 Pip：**

```shell
sudo pip3 install --upgrade pip
```

## **安装 `pip`：**

如果你使用的是 Python 3，请尝试使用以下命令：

```shell
sudo yum install python3-pip
```

## 缺失共享库 `libXtst.so.6`

**安装 libXtst 库：**

```shell
sudo yum install libXtst
```

## 缺少ODBC（Open Database Connectivity）库

错误：

```shell
  File "/opt/python_project/tailshark.py", line 1, in <module>
    import pyodbc
ImportError: libodbc.so.2: cannot open shared object file: No such file or directory
```

1、**安装 ODBC 库**

```shell
sudo yum install unixODBC
```

2、**检查 libodbc.so.2 是否存在：**

确保 `libodbc.so.2` 文件已经存在。你可以使用以下命令查找：

```shell
find / -name "libodbc.so.2" 2>/dev/null
```

3、**设置库路径：**

如果库文件在非系统标准路径中，你需要确保将其路径添加到系统库路径。可以通过以下方式之一实现：

- **使用 LD_LIBRARY_PATH：**

```shell
export LD_LIBRARY_PATH=/path/to/directory/containing/libodbc.so.2:$LD_LIBRARY_PATH
```

- 请将 `/path/to/directory/containing/` 替换为包含 `libodbc.so.2` 的目录的实际路径。

- **将路径添加到 /etc/ld.so.conf 文件：**

  编辑 `/etc/ld.so.conf` 文件，将库文件所在的目录添加到文件末尾，然后运行 `sudo ldconfig` 更新库缓存。

## FreeTDS驱动程序

错误：

```shell
Traceback (most recent call last):
  File "/opt/python_project/tailshark.py", line 9, in <module>
    conn = pyodbc.connect(conn_str)
pyodbc.Error: ('01000', "[01000] [unixODBC][Driver Manager]Can't open lib 'FreeTDS' : file not found (0) (SQLDriverConnect)")
```

1、**安装 FreeTDS 驱动程序：**

```shell
sudo yum install freetds
```

2、**检查 FreeTDS 是否正确安装：**

```shell
确保 FreeTDS 安装正常。你可以检查是否存在 FreeTDS 的配置文件（/etc/freetds.conf）以及 FreeTDS 的库文件路径。
```

3、**设置 ODBC 配置：**

请确保你的 ODBC 配置正确指向 FreeTDS。你可以在 `/etc/odbcinst.ini` 文件中查找配置。确保该文件包含类似以下内容：

```shell
[FreeTDS]
Description     = FreeTDS ODBC Driver
Driver          = /usr/lib64/libtdsodbc.so
```

请注意，`Driver` 行的路径应该指向 FreeTDS 的实际库文件路径。

