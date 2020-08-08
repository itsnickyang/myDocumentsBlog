# mac 本地配置多个 ssh 





## Github 生成RSA key（公钥+秘钥）

```shell
$ ssh-keygen -t rsa -b 4096 -C "yangchun.qdu@github.com"
```

引号内为自己的邮箱，可替换为自己的 github 邮箱

点击回车之后提示`Enter file in which to save the key (~/.ssh/id_rsa)`

此时可以在该行提示后边输入一个绝对地址，并且支持 rsa 文件的自定义名称，比如说我的是 /Users/nick/.ssh/id_rsa_github，我在最后加==github==用以区分此公钥私钥属于哪个网站。

## Gitlab 生成RSA key（公钥+秘钥）--公司

```shell
$ ssh-keygen -o -t rsa -b 4096 -C "yangchun@stargraph.com"
```

## Gitee 生成RSA key（公钥+秘钥）--图床

```shell
1.$ ssh-keygen -t rsa -C "uncle.young@foxmail.com" 
```

```shell
2.$ ssh-keygen -t rsa -C 'uncle.young@foxmail.com' -f ~/.ssh/id_rsa_gitee
```

可以用 2 直接指定地址与自定义名称

将以上生成的公钥分别添加到对应的网站

在~/.ssh/下新建一个 config 文件

```
###########################################
#个人 github 项目
Host github.com
	HostName github.com 
	User yangchun.qdu@gmail.com 
	PreferredAuthentications publickey 
	IdentityFile ~/.ssh/id_rsa_github 
    # Port 443 
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

