参考：https://docs.docker.com/machine/install-machine/

----------

[toc]

- [直接安装Machine](#)
- [安装bash completion脚本](#)
- [如何卸载Docker Machine](#)

----------
在macOS和Windows上，当您安装Docker for Mac，Docker for Windows或Docker Toolbox时，Machine将与其他Docker产品一起安装。

如果您只需要Docker Machine，则可以按照下一节中的说明直接安装Machine二进制文件。 您可以在GitHub上的[docker / machine release页面](https://github.com/docker/machine/releases/)上找到最新版本的二进制文件。

# 直接安装Machine
1、安装docker
2、下载Docker Machine二进制文件并将其解压到PATH。

如果你在macOS上运行：

```
$ curl -L https://github.com/docker/machine/releases/download/v0.13.0/docker-machine-`uname -s`-`uname -m` >/usr/local/bin/docker-machine && \
  chmod +x /usr/local/bin/docker-machine
```
如果你在Linux上运行：

```
$ curl -L https://github.com/docker/machine/releases/download/v0.13.0/docker-machine-`uname -s`-`uname -m` >/tmp/docker-machine &&
chmod +x /tmp/docker-machine &&
sudo cp /tmp/docker-machine /usr/local/bin/docker-machine
```
如果你正在用Git BASH运行Windows：

```
$ if [[ ! -d "$HOME/bin" ]]; then mkdir -p "$HOME/bin"; fi && \
curl -L https://github.com/docker/machine/releases/download/v0.13.0/docker-machine-Windows-x86_64.exe > "$HOME/bin/docker-machine.exe" && \
chmod +x "$HOME/bin/docker-machine.exe"
```
3、通过显示Machine 版本检查安装：

```
$ docker-machine version
docker-machine version 0.13.0, build 9371605
```
# 安装bash completion脚本
Machine存储库提供了几个bash脚本，添加了如下功能：

 - command completion
 - 一个在shell提示符下显示活动machine的函数
 - 一个函数包装器，添加一个docker-machine use子命令来切换活动的机器

确认版本并将脚本保存到/etc/bash_completion.d或/usr/local/etc/bash_completion.d：

```
scripts=( docker-machine-prompt.bash docker-machine-wrapper.bash docker-machine.bash ); for i in "${scripts[@]}"; do sudo wget https://raw.githubusercontent.com/docker/machine/v0.13.0/contrib/completion/bash/${i} -P /etc/bash_completion.d; done
```

要启用docker-machine shell提示符，请在〜/ .bashrc中将$（__ docker_machine_ps1）添加到PS1设置中。

```
PS1='[\u@\h \W$(__docker_machine_ps1)]\$ '
```
您可以在[每个脚本顶部的注释](https://github.com/docker/machine/tree/master/contrib/completion/bash)中找到其他文档。

# 如何卸载Docker Machine

要卸载Docker Machine：

 - 删除可执行文件：rm $（docker-machine）
 - 或者，删除您创建的机器。
 要单独删除每台机器：docker-machine rm <machine-name>
 要删除所有机器：docker-machine rm -f $（docker-machine ls -q）（您可能需要在Windows上使用-force）

删除机器是一个可选步骤，因为有些情况下您可能想要将现有机器保存并迁移到Docker for Mac或Docker for Windows环境中。

注意：作为信息的一部分，与docker-machine创建的每个虚拟机相关的config.json，证书和其他数据存储在Mac和Linux上的〜/ .docker / machine / machines /和〜\ .docker中 \ machine \ machines \在Windows上。 我们建议您不要直接编辑或删除这些文件，因为这只会影响Docker CLI的信息，而不会影响实际的虚拟机，不管它们是本地还是远程服务器。
