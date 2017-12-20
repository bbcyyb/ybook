## 使用Flask和Bootstrap构建博客系统

### 技术栈

- macOS10.12.5
- Python2.7.13
- Bootstrap4.0.0-beta.2
- virtualenv
- virtualenvwrapper

#### 安装Python2.7.13

#### 下载Bootstrap4.0.0-beta.2

#### 安装virtualenv

1. 用pip进行安装：

```shell
$ pip install virtualenv
```

我采用virtualenvwrapper来管理虚拟环境，所以可以不用配置virtualenv。

#### 安装virtualenvwrapper

1. 用pip进行安装：

```shell
$ pip install virtualenvwrapper
```

2. 创建目录用来存放虚拟环境：

```shell
$ mkdir ~/.virtualenvs
```

3. 在.bashrc或是.zshrc中添加相关配置信息：

```shell
export WORKON_HOME=~/.virtualenvs
source /usr/local/bin/virtualenvwrapper.sh
```

4. 运行`source .bashrc`或是`source .zshrc`

此时virtualenvwrapper就可以使用了。

#### virtualenvwrapper命令列表

- workon:列出虚拟环境列表
- lsvirtualenv:同上
- mkvirtualenv :新建虚拟环境
- workon [虚拟环境名称]:切换虚拟环境
- rmvirtualenv :删除虚拟环境
- deactivate: 离开虚拟环境

