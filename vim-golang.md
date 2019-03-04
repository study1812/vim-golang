环境
CentOS7 

1 安装 vim8

卸载 系统自带vim
yum remove vim-* -y

安装 带有vim8 的yum 仓库
cd /root
wget http://mirror.ghettoforge.org/distributions/gf/gf-release-latest.gf.el7.noarch.rpm

rpm -ivh gf-release-latest.gf.el7.noarch.rpm


安装vim8

yum --enablerepo=gf-plus install vim-enhanced   -y


确认 有如下输出
vim --version
VIM - Vi IMproved 8.0 (2016 Sep 12, compiled Sep 18 2016 14:42:40)


安装 golang
cd /root
wget https://studygolang.com/dl/golang/go1.12.linux-amd64.tar.gz

tar xzvf go1.12.linux-amd64.tar.gz

mv   /root/go        /usr/local/


export GOPATH=/opt/go
export GOROOT=/usr/local/go
export GOBIN=$GOROOT/bin/
export GOTOOLS=$GOROOT/pkg/tool/
export PATH=$PATH:$GOBIN:$GOTOOLS


source  /etc/profile
mkdir /opt/go 


go version
go version go1.12 linux/amd64


config  vimrc

rm -rf /root/.vimrc

cat > /root/.vimrc << EOF
call plug#begin()
Plug 'fatih/vim-go'
call plug#end()
EOF


mkdir ~/.vim/autoload/
cd ~/.vim/autoload/
wget https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim



curl -fLo /root/.vim/autoload/plug.vim --create-dirs https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim


vim
:PlugInstall


或者手动下载
git clone https://github.com/fatih/vim-go.git  /root/.vim/plugged/vim-go


vim  +GoInstallBinaries

ls  /opt/go/src/
github.com


很多包国内无法 go  get 


换一个方法


mkdir /opt/go/src/golang.org/x/


git clone https://github.com/golang/tools.git         /opt/go/src/golang.org/x/tools


git clone https://github.com/golang/lint.git          /opt/go/src/golang.org/x/lint




go install golang.org/x/tools/cmd/guru

go install golang.org/x/lint/golint

go install golang.org/x/tools/go/packages

go install golang.org/x/tools/cmd/gorename
go install golang.org/x/tools/cmd/goimports


honnef.co/go/tools/cmd/keyify






vim  +GoInstallBinaries
vim-go: installing finished!

mkdir  /opt/go/src/github.com/study1812


vim /opt/go/src/github.com/study1812/test.go




https://github.com/fatih/vim-go-tutorial

go get github.com/fatih/vim-go-tutorial

vim /opt/go/src/github.com/fatih/vim-go-tutorial/main.go


######  运行程序

运行 程序

运行当前 .go 文件

:GoRun %

运行当前package 

:GoRun

##### 编译程序

:GoBuild

没有创建二进制文件；您可以调用：gobuild多次，而不会污染您的工作区。

它会自动CD到源程序包的目录中

它分析任何错误并将其显示在快速修复列表中

它会自动检测gopath并在需要时对其进行修改（检测诸如gb、godeps等项目）。

如果在VIM 8.0.XXX或NEOVIM中使用，则运行异步


















































































































