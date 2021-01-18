<!-- TOC -->

- [1. 一种Windows下个人觉得舒适的vscode编程环境](#1-一种windows下个人觉得舒适的vscode编程环境)
- [2. conda命令](#2-conda命令)
  - [2.1. 源](#21-源)
  - [2.2. 常见谜之问题](#22-常见谜之问题)
- [3. 其他命令](#3-其他命令)

<!-- /TOC -->

# 1. 一种Windows下个人觉得舒适的vscode编程环境
环境指的人的环境  

下载anaconda和cuda  
手动添加```$(anaconda安装路径)/script```至环境变量
> 注意: 卸载和重装的时候对环境变量的维护

在vscode中的powershell中使用conda  
> 查看执行策略 get-ExecutionPolicy
> | 策略名称 | 效果 |
> |-|-|  
> | Restricted | 默认的设置， 不允许任何script运行 |
> | AllSigned | 只能运行经过数字证书签名的script | 
> | RemoteSigned | 运行本地的script不需要数字签名，但是运行从网络上下载的> script就必须要有数字签名 |
> | Unrestricted | 允许所有的script运行 |
管理员执行：set-ExecutionPolicy RemoteSigned即可



# 2. conda命令
## 2.1. 源
更换清华源参考官网```https://mirror.tuna.tsinghua.edu.cn/help/anaconda/```

设置搜索时显示通道地址
>```
>conda config --set show_channel_urls yes
>```
删除源
>```
>conda config --remove channels <url>
>```

## 2.2. 常见谜之问题
conda命令创建环境或者下载包无报错，但所有操作都未成功（```Solving environment: done```）
解决方案：
>```python
>conda clean --all #清理无用的包或安装包
>```
>包括的选项还有：
>```
>--lock, --tarballs, --index-cache, --packages, --source-cache
>```  

问题分析：  
> 可能是因为以前在conda下载包的过程中被中止了，造成无向的缓存，需要清理之后才能操作（
> 找了很久没找到问题和解决方案/(ㄒoㄒ)/~~）

# 3. 其他命令
pip换源  
临时
> ```shell
> pip install numpy -i https://pypi.tuna.tsinghua.edu.cn/simple
> ```

查看cuda版本(windows)
> ```
> nvcc -v
> ```