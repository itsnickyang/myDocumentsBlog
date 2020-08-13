# mac 本地配置多个 ssh 



## 1.先生成公钥秘钥

### Github 生成RSA key（公钥+秘钥）

```shell
$ ssh-keygen -t rsa -b 4096 -C "yangchun.qdu@github.com"
```

引号内为自己的邮箱，可替换为自己的 github 邮箱

- 点击回车之后提示`Enter file in which to save the key (~/.ssh/id_rsa)`

  此时可以在该行提示后边输入一个绝对地址，并且支持 rsa 文件的自定义名称，比如说我的是 /Users/nick/.ssh/id_rsa_github，我在最后加==github==用以区分此公钥私钥属于哪个网站

- 当然也可以直接在后边加 `-f + 地址`

  ```shell
  ssh-keygen -t rsa -b 4096 -C "yangchun.qdu@github.com" -f  ~/.ssh/id_rsa_github
  ```

  

### Gitlab 生成RSA key（公钥+秘钥）--公司

```shell
$ ssh-keygen -o -t rsa -b 4096 -C "yangchun@stargraph.com"
```

### Gitee 生成RSA key（公钥+秘钥）--图床

```shell
1.$ ssh-keygen -t rsa -C "uncle.young@foxmail.com" 
```

## 2.将以上生成的公钥分别添加到对应的网站

## 3.在~/.ssh/下新建一个 config 文件

```
###########################################
#个人 github 项目
Host github.com
	HostName github.com 
	User yangchun.qdu@gmail.com 
	PreferredAuthentications publickey 
	IdentityFile ~/.ssh/id_rsa_github 
############################################

############################################
#公司
Host git.sz.haizhi.com
	HostName git.sz.haizhi.com
	User yangchun@stargraph.com
	Preferredauthentications publickey
	IdentityFile ~/.ssh/id_rsa_gitlab 
############################################

############################################
# gitee图床项目
Host gitee.com
    HostName gitee.com
    User uncle.young@foxmail.com
    PreferredAuthentications publickey
    IdentityFile ~/.ssh/id_rsa_gitee
############################################
```

以上是我的配置，可根据相应的字段本地化自己的字段值

shell 中检查 ssh 连接是否成功

```
ssh -T git@github.com
Hi itsnickyang! You've successfully authenticated, but GitHub does not provide shell access.

ssh -T git@git.haizhi.com
Welcome to GitLab, @yangchun!

ssh -T git@gitee.com
Hi itsnick! You've successfully authenticated, but GITEE.COM does not provide shell access.
```

## 4.修改本地与 gitlab 通信方式为 git

这里仅针对连接方式是 http 的修改，如果本身为 git，则不需要修改

首先终端进入一个本地仓库，使用以下命令查看连接方式

- 查看

  ```shell
  git remote -v
  ```

  >
  >
  >终端返回了如下内容
  >
  >origin	https://github.com/itsnickyang/myDocumentsBlog.git (fetch)
  >origin	https://github.com/itsnickyang/myDocumentsBlog.git (push)

- 删除原连接

```shell
git remote rm orgin
```

- 重新建立连接

```shell
git remote add origin git@github.com:itsnickyang/myDocumentsBlog.git
```

origin 后边地址可根据自己的仓库名字自行修改