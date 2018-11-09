## 设置用户参数

安装完成后，还需要最后一步设置，在命令行输入：

```bash
$ git config --global user.name "kongxiangyan"
$ git config --global user.email "1145172089@qq.com"
```

因为Git是分布式版本控制系统，所以，每个机器都必须自报家门：你的名字和Email地址。你也许会担心，如果有人故意冒充别人怎么办？这个不必担心，首先我们相信大家都是善良无知的群众，其次，真的有冒充的也是有办法可查的。

注意`git config`命令的`--global`参数，用了这个参数，表示你这台机器上所有的Git仓库都会使用这个配置，当然也可以对某个仓库指定不同的用户名和Email地址。

## Link to Github

### 设置加密信息

由于你的本地Git仓库和GitHub仓库之间的传输是通过SSH加密的，所以，需要一点设置：

#### Check

检查当前是否可以连接到Github：

```bash
$ ssh -T git@github.com
```

#### 创建SSH Key

否则，创建SSH Key，在用户主目录下，看看有没有.ssh目录，如果有，再看看这个目录下有没有`id_rsa`和`id_rsa.pub`这两个文件，如果已经有了，可直接跳到下一步。如果没有，打开Shell（Windows下打开Git Bash），创建SSH Key：

```bash
# 备份并移除已经存在的ssh keys
$ mkdir key_backup
$ cp id_rsa* key_backup
$ rm id_rsa* 
# 将已经存在的id_rsa，id_rsa.pub文件备份到key_backup文件夹

$ ssh-keygen -t rsa -C "1145172089@qq.com"
```

你需要把邮件地址换成你自己的邮件地址，然后一路回车，使用默认值即可，由于这个Key也不是用于军事目的，所以也无需设置密码。

```bash
# 新生成密钥的时候，git clone或者push的时候，经常会报这样的错误：
The authenticity of host 'github.com (192.30.255.112)' can't be established.
RSA key fingerprint is SHA256:nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8.
# 原因：少一个known_hosts文件
# 解决方案：
Are you sure you want to continue connecting (yes/no)? //输入yes，回车
```

如果一切顺利的话，可以在用户主目录里找到`.ssh`目录，里面有`id_rsa`和`id_rsa.pub`两个文件，这两个就是SSH Key的秘钥对，`id_rsa`是私钥，不能泄露出去，`id_rsa.pub`是公钥，可以放心地告诉任何人。

#### Add to Github 

```bash
# 确认公钥
~/.ssh $ cat id_rsa.pub
```

打开`Account settings`，`SSH Keys`页面：

然后，点`Add SSH Key`，填上任意Title，在Key文本框里粘贴`id_rsa.pub`文件的内容：

![github-addkey-1](https://cdn.liaoxuefeng.com/cdn/files/attachments/001384908342205cc1234dfe1b541ff88b90b44b30360da000/0)

点`Add Key`，你就应该看到已经添加的Key：

![github-addkey-2](https://cdn.liaoxuefeng.com/cdn/files/attachments/0013849083502905a4caa2dc6984acd8e39aa5ae5ad6c83000/0)

为什么GitHub需要SSH Key呢？因为GitHub需要识别出你推送的提交确实是你推送的，而不是别人冒充的，而Git支持SSH协议，所以，GitHub只要知道了你的公钥，就可以确认只有你自己才能推送。

当然，GitHub允许你添加多个Key。假定你有若干电脑，你一会儿在公司提交，一会儿在家里提交，只要把每台电脑的Key都添加到GitHub，就可以在每台电脑上往GitHub推送了。

### 关联仓库

```bash
$ git remote add origin git@github.com:kongxiangyan/${GITREPO}.git
```

