##	go配置

*	**在linux（Mac）下，为了方便，一般配置在~/.bash_profile中**

vi ~/.bash_profile //编辑

source ~/.bash_profile //编辑完成后，使立即生效

例如：我的GOPATH设置（MAC下）

export GOPATH=$HOME/workspace/go

export PATH=$PATH:${GOPATH//://bin:}/bin

export GOBIN=

其中，“export PATH=$PATH:${GOPATH//://bin:}/bin”为Linux（Mac）下把每个GOPATH下的bin都加入到PATH中。

*	**在ubuntu下一般保存在~/.bashrc文件**

$ gedit ~/.bashrc

然后加入

export GOROOT=~/go

export GOARCH=386

export GOOS=linux

export GOBIN=$GOROOT/bin/

export GOTOOLS=$GOROOT/pkg/tool/

export PATH=$PATH:$GOBIN:$GOTOOLS