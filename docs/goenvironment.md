# Setting up your go environment

## Do Once
Setup your golang environment by setting the following in your ~/.bash_profile or ~/.bashrc
```bash
export GOPATH=$HOME/go # don't forget to change your path correctly!
export GOROOT=/usr/local/opt/go/libexec
export PATH=$PATH:$GOPATH/bin
export PATH=$PATH:$GOROOT/bin
```

Setup your the workspace directory tree:
```bash
mkdir -p $GOPATH $GOPATH/src $GOPATH/pkg $GOPATH/bin
mkdir -p $GOPATH/src/github.com/wexinc
```

Clone the service
```bash
cd $GOPATH/src/github.com/wexinc/
git clone git@github.com:wexinc/ps-cbs-notification_service.git
```

Setup pre-commit hooks: [`goimports`](https://godoc.org/golang.org/x/tools/cmd/goimports), [`go lint`](https://github.com/golang/lint), [`go vet`](https://godoc.org/github.com/golang/go/src/cmd/vet)
```bash
go get golang.org/x/tools/cmd/goimports
go get -u golang.org/x/lint/golint
cp $GOPATH/src/github.com/wexinc/ps-cbs-notification_service/script/hooks/pre-commit $GOPATH/src/github.com/wexinc/ps-cbs-notification_service/.git/hooks/pre-commit
```

Setup your local git environment to use ssh instead of HTTPS

Add the following lines to your `~/.gitconfig` file. Create it if it doesn't exist.
```bash
[url "ssh://git@github.com/"]
	insteadOf = https://github.com/
```

## Iterate
Add import statements to .go code as needed. Build the binary.
```bash
cd $GOPATH/src/github.com/wexinc/<project>

# avoid external lookups of our private modules
export GOPRIVATE="github.com/wexinc"

go build
```

## go mod dependency management
This has already been done, but I thought I'd document it.  Go build does `go get` for you now.
```bash
cd $GOPATH/src/github.com/wexinc/notification_service
go mod init github.com/wexinc/notification_service
```

### More information
For more information about code organization and local environments, see the following:

* https://golang.org/doc/code.html#Organization
* https://www.digitalocean.com/community/tutorials/how-to-install-go-and-set-up-a-local-programming-environment-on-macos
* https://medium.com/@radlinskii/writing-the-pre-commit-git-hook-for-go-files-810f8d5f1c6f

---
