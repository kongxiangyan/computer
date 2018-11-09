## 推送本地内容到github上新建立的仓库

### 建立本地仓库

```bash
# 本地仓库初始化
cd /path/to/LocalRepo
git init
```

### 更改本地仓库并提交。

==此步骤不可省略==

```bash
# 举个例子
touch Readme
git add Readme # git add .
git commit -m 'add readme file'
```

### 关联Remote Repo

```bash
git remote add origin https://github.com/你的github用户名/你的github仓库.git  
```

### 推送修改

```bash
git push origin master
# OR
git push -u origin master
```

## 克隆Remote Repo

```bash
git clone https://github.com/你的github用户名/github仓库名.git
# OR
git clone https://github.com/你的github用户名/github仓库名.git 本地文件夹名
```

## 强制覆盖本地内容

```bash
# 下载代码到本地，不进行合并操作
git fetch --all
# 清理提交的更改
git clean -f
# 把HEAD指向刚刚下载的最新的版本
git reset --hard origin/master
# 取回远程主机某个分支的更新，再与本地的指定分支合并
git pull # 此处为合并更新
```

```bash
# 强制恢复版本到前一个稳定版
git reset --hard origin/master 
# 清理提交的更改
git clean -f 
# 拉取最新的版本到本地
git pull 2>&1
```

