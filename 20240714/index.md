# jetbrains家的goland项目可用但老爆红


## 问题

目前新的go项目默认都是```go module```模式，由于我需要使用私有仓库的package，清除了mod的缓存后Goland就识别不到我后续在命令行下执行

```
go mod tidy
```

下载的依赖了。

goland爆红是因为没有在当前项目的vendor下找到这个玩意。(或根本没找到vendor目录)

## 解决

在项目根目录下执行以下命令：

```
go mod vendor
```

这会在当前项目目录下面建立一个vendor目录，用于存放当前项目的package。

然后goland会自动识别到这个目录，就不会再爆红了。

但由于上传Github是不需要上传这部分依赖

需要项目根目录的

```
.gitignore
```

文件内添加一行

```
vendor/
```

后在命令行执行

```
git add .gitignore
git commit -m "Add vendor to .gitignore"
```

然后执行

```
git rm -r --cached vendor
git commit -m "Remove vendor directory from version control"
```

进行测试

然后后续应该就不会上传vendor文件夹了

## 后言

goland的爆红问题，上面只是一个可能的原因和解决方式，不保证该方式通用。

