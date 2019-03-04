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




#### 错误修复

cat > /opt/go/src/github.com/fatih/vim-go-tutorial/main.go  << EOF
package main

import "fmt"

var b = foo()

func main() {
        fmt.Println("Hello GopherCon")
	        a
	}
EOF


:GoBuild

输出如下

/opt/go/src/github.com/fatih/vim-go-tutorial/main.go                                                                                                                        5,9            Bot
/opt/go/src/github.com/fatih/vim-go-tutorial/main.go|5 col 9| undefined: foo                                                                                                                  
/opt/go/src/github.com/fatih/vim-go-tutorial/main.go|9 col 2| undefined: a
[Quickfix List] :go 'build' '-tags' '' '.' 'errors'                           


遇到错误 vim-go 打开了 QuickFix 视图

定位到下一个错误
:cnext

定位到上一个错误
:cprevious

关闭
:cclose


再次打开这个文件
vim /opt/go/src/github.com/fatih/vim-go-tutorial/main.go

编译
:GoBuild

光标已经定位到第一个错误
定位到下一个错误

:cnext                                                                                                                         
dd 删除错误行

保存
:w

再次编译
这次出现一个错误
修复并保存
:w

编译
:GoBuild

vim-go: [build] SUCCESS                                                                                                        4,0-1         Bot
编译通过


设置当调用make 时自动写入 而不需要手动 :w  进行写入

echo "set autowrite"     >>  /root/.vimrc

修复错误后 直接运行GoBuild  即可


可以设置快捷键 操作 QuickFix

echo 'map <C-n> :cnext<CR>'  >> /root/.vimrc
echo 'map <C-m> :cprevious<CR>' >> /root/.vimrc
echo 'nnoremap <leader>a :cclose<CR>' >> /root/.vimrc


用法
ctrl n
ctrl m
\a


#####  测试
cat > /opt/go/src/github.com/fatih/vim-go-tutorial/main.go  << EOF
package main
import "fmt"

func main() {
        fmt.Println("Hello GopherCon")
        }

func Bar() string {
	return "bar"
}
EOF




cat  >  /opt/go/src/github.com/fatih/vim-go-tutorial/main_test.go  <<  EOF
package main

import (
	"testing"
)

func TestBar(t *testing.T) {
	result := Bar()
	if result != "bar" {
		t.Errorf("expecting bar, got %s", result)
	}
}
EOF


可以不打开 test 文件进行test
vim /opt/go/src/github.com/fatih/vim-go-tutorial/main.go


:GoTest
vim-go: [test] PASS

vim /opt/go/src/github.com/fatih/vim-go-tutorial/main_test.go 
:GoTest





仅仅运行指定函数得测试



package main

import "fmt"

func main() {
	fmt.Println("Hello GopherCon")
}

func Bar() string {
	return "bar"
}



package main

import (
	"testing"
)

func TestFoo(t *testing.T) {
	t.Error("intentional error 1")
}

func TestBar(t *testing.T) {
	result := Bar()
	if result != "bar" {
		t.Errorf("expecting bar, got %s", result)
	}
}

func TestQuz(t *testing.T) {
	t.Error("intentional error 2")
}




vim /opt/go/src/github.com/fatih/vim-go-tutorial/main.go
:GoTest

/opt/go/src/github.com/fatih/vim-go-tutorial/main_test.go
main_test.go|8| intentional error 1
main_test.go|19| intentional error 2
[Quickfix List] :    cgetexpr a:items
"main_test.go" 20L, 282C


因为测试文件 测试函数到 main.go 中不村子啊

光标移动到 TestBar

:GoTestFunc

测试通过







或者 直接打开测试文件
vim /opt/go/src/github.com/fatih/vim-go-tutorial/main_test.go 
光标移动到 TestBar

:GoTestFunc

测试通过



为了方便使用可以设置快捷键
autocmd FileType go nmap <leader>b  <Plug>(go-build)
autocmd FileType go nmap <leader>r  <Plug>(go-run)
autocmd FileType go nmap <leader>t  <Plug>(go-test)



build_go_files is a custom function that builds or compiles the test file

build_go_files 是一个自定义函数 用户 build  或者 compiles 测试文件


It calls :GoBuild if its a Go file, or :GoTestCompile if it's a test file

如果是go 文件会执行 GoBuild
如果是测试文件 会执行 GoTestCompile


Cover it

package main

import (
	"testing"
)

func TestBar(t *testing.T) {
	result := Bar()
	if result != "bar" {
		t.Errorf("expecting bar, got %s", result)
	}
}



package main

func Bar() string {
	return "bar"
}

func Foo() string {
	return "foo"
}

func Qux(v string) string {
	if v == "foo" {
		return Foo()
	}

	if v == "bar" {
		return Bar()
	}

	return "INVALID"
}



覆盖测试

Go有一个非常好的方法来显示源代码的覆盖范围。vim go使查看代码覆盖率变得容易，而不会以非常优雅的方式离开vim


调用 :GoCoverage。在引擎盖下，
这将调用go test-coverprofile tempfile。
它解析概要文件中的行，然后动态更改源代码的语法以反映覆盖率。
正如您所看到的，因为我们只有对bar（）函数的测试，这是唯一一个绿色的函数。

vim /opt/go/src/github.com/fatih/vim-go-tutorial/main.go 

:GoCoverage




要清除语法高亮 ，可以调用
:GoCoverageClear

让我们添加一个测试用例，看看覆盖率是如何变化的。
将以下内容添加到main_test.go：



func TestQuz(t *testing.T) {
	result := Qux("bar")
	if result != "bar" {
		t.Errorf("expecting bar, got %s", result)
	}

	result = Qux("qux")
	if result != "INVALID" {
		t.Errorf("expecting INVALID, got %s", result)
	}
}




如果我们再次调用

:GoCoverage
您将看到quz函数现在也被测试了，它有更大的覆盖范围。

:GoCoverageClear
再次清除语法突出显示。


因为调用：gocoverage和：gocoverage clear经常一起使用，所以还有另一个命令可以很容易地调用和清除结果。
您还可以使用：gocoveragetoggle。这将用作切换并显示覆盖范围，当再次调用时，它将清除覆盖范围。
如何使用它们取决于您的工作流程。

:GoCoverageToggle





最后，如果您不喜欢VimGo的内部视图,也可以调用 
:GoCoverageBrowser

在引擎盖下，它使用Go工具盖创建一个HTML页面，然后在默认浏览器中打开它。有些人更喜欢这个。
测试 没有生效




使用
:GoCoverageXXX
命令不会创建任何类型的临时文件，也不会污染您的工作流。所以你不必每次都删除不需要的文件。



编辑 文件


package main

     import "fmt"

func main() {
 fmt.Println("gopher"     )
}



让我们从我们已经知道的事情开始。如果我们保存文件，您将看到它将被自动格式化。
它在默认情况下是启用的，但是如果需要的话可以禁用
让g:go-fmt-u-autosave=0。
我们还可以选择提供：gofmt命令，它在引擎盖下运行gofmt。

让我们用大写字母打印“gopher”字符串。为此，我们将使用字符串包。将定义更改为：

fmt.Println(strings.ToUpper("gopher"))


:GoBuild

main.go
main.go|6 col 14| undefined: strings
[Quickfix List] :go 'build' '-tags' '' '.' 'errors'


您将看到我们收到一个错误，因为字符串包没有导入。VimGo有几个命令，可以方便地操作导入声明。
我们可以很容易地去编辑文件，但是我们将使用vim命令：goimport。
此命令将给定的包添加到导入路径。通过
:GoImport 
字符串运行它
您将看到正在添加字符串包。这个命令的好处在于它还支持补全
所以你只需输入：goimport s和 敲击tab

:GoImport stri   tab






使用
:GoImportAs
:GoDrop 

来编辑导入路径。
：goimport as与：goimport相同，\]\
但它允许更改包名称。
例如：


:GoDrop strings
:GoImportAs str strings



最后：godrop使从导入声明中删除任何导入路径变得容易。
：godrop字符串将从导入声明中删除它。

当然，操纵导入路径是2010年的事情。我们有更好的工具来处理这个案子。如果你还没听说，
那就叫Goimports。
goimports是gofmt的替代品。
你有两种使用方法。第一种（也是推荐的）方法是告诉Vim在保存文件时使用它：



让g:go_fmt_command=“goimports”

现在，无论何时保存文件，goimports都会自动格式化并重写导入声明。
有些人不喜欢goimports，因为它在很大的代码基础上可能很慢。
在本例中，我们还有：goimports命令（注意末尾的s）。
有了这个，您可以显式地调用goimports


:GoImports


文本对象

Text objects


package main

import "fmt"

func main() {
	fmt.Println(1)
	fmt.Println(2)
	fmt.Println(3)
	fmt.Println(4)
	fmt.Println(5)
}


normal 模式下
在函数上执行  
dif
撤销
u

您将看到函数体被移除。因为我们用了D运算符。使用u撤消更改。
最重要的是，光标可以位于任何位置，从func关键字开始，直到右大括号。

它使用引擎盖下的工具运动。我为VimGo显式地编写了运动来支持这样的特性。
它具有AST感知能力，因此它的功能非常好。就像你可能问的那样？更改main.go 为：



package main

import "fmt"

func Bar() string {
	fmt.Println("calling bar")

	foo := func() string {
		return "foo"
	}

	return foo()
}


以前我们使用基于regexp的文本对象，这会导致问题。
例如，在本例中，将光标放在匿名函数的func关键字上，
并在正常模式下执行dif。您将看到只删除了匿名函数的主体。

dif


到目前为止，我们只使用了d操作符（delete）。
但这取决于你。例如，您可以通过
vif   选择
yank（copy）和
yif选择它。


我们还有af，这意味着一个函数。
此文本对象包含整个函数声明 更改 main.go  文件为

package main

import "fmt"

// bar returns a the string "foo" even though it's named as "bar". It's an
// example to be used with vim-go's tutorial to show the 'if' and 'af' text
// objects.
func bar() string {
	fmt.Println("calling bar")

	foo := func() string {
		return "foo"
	}

	return foo()
}








































































































































































































































