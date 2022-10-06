# 在Ubuntu 20.04上安装Pyenv 管理多版本Python和结合pyenv-virtualenv使用

* 2022-07-20

### 参考链接
* [Simple Python Version Management: pyenv](https://github.com/pyenv/pyenv#getting-pyenv)
* [pyenv-virtualenv 是一个pyenv插件，它提供了在类 UNIX 系统上为 Python 管理 virtualenvs 和 conda 环境的功能。](https://github.com/pyenv/pyenv-virtualenv)

 ### 先安裝 dependency package
 ```powershell
 $ sudo apt-get update; sudo apt-get install -y --no-install-recommends make build-essential libssl-dev zlib1g-dev libbz2-dev libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev xz-utils tk-dev libxml2-dev libxmlsec1-dev libffi-dev liblzma-dev
 ```
 ### 安装 pyenv
 1. 从Github中安装
 ```powershell
 $ git clone https://github.com/pyenv/pyenv.git ~/.pyenv
 ```
2. 为 Pyenv 设置你的 shell 环境

主要看你用哪个 shell，不同的 shell 设定也不同，而 Ubuntu 自带的是 bash，故先以 bash 为例。
**bash 设定`.bashrc`、`.profile`**
>> 这部分很容易出错
最好的方式是参考官方的例程复制以下脚本：
```powershell
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bashrc
echo 'command -v pyenv >/dev/null || export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(pyenv init -)"' >> ~/.bashrc
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.profile
echo 'command -v pyenv >/dev/null || export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.profile
echo 'eval "$(pyenv init -)"' >> ~/.profile

# 重启shell
exec $SHELL
```

3. zsh：设定 zsh 的.zprofile、.zshrc
```powershell
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.zshrc
echo 'command -v pyenv >/dev/null || export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.zshrc
echo 'eval "$(pyenv init -)"' >> ~/.zshrc

# 重启shell
exec $SHELL
```
4. 验证安装
```powershell
$ pyenv --version 
```
5. `pyenv`使用命令
```powershell
$ pyenv install --list #列出当前所有的可安装python版本
$ pyenv install x.x.x #安装pythonx.x.x版本
$ pyenv global x.x.x #全局切换到指定的python版本
$ pyenv versions #列出pyenv下的所有python版本，*表示当前所使用版本
$ pyenv version #只显示当前使用的python版本
$ pyenv uninstall x.x.x #卸载某个版本
```

### 安装与设置 pyenv-virtualenv
1. 从 GitHub 安装 pyenv-virtualenv
```powershell
$ git clone https://github.com/pyenv/pyenv-virtualenv.git $(pyenv root)/plugins/pyenv-virtualenv
```
2. 添加pyenv virtualenv-init到您的 shell以启用 virtualenvs 的自动激活。
```powershell
$ echo 'eval "$(pyenv virtualenv-init -)"' >> ~/.bashrc
```
**Zsh 注意**：修改你的~/.zshrc文件而不是~/.bashrc.
3. 重新启动你的 shell 以启用 pyenv-virtualenv
```powershell
$ exec "$SHELL"
```
4. 创建虚拟环境
```powershell
$ pyenv virtualenv 3.10.4 venv-3.10.4 #基于python3.10.4创建一个虚拟环境
```
5. 创建当前版本的虚拟环境
```powershell
$ pyenv version
3.10.4 (set by /home/lemu-devops/.pyenv/version)
$ pyenv virtualenv venv-3.10.4
$ pyenv activate <name> #激活虚拟环境
$ pyenv deactivate #切换回系统环境
$ pyenv uninstall <name> #卸载某个虚拟环境，也可以直接删除所在的目录即可
```
