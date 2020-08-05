### gitlab部署和使用文档



#### 一、在centos7上安装gitlab

##### 1.下载基础依赖

```
#安装技术依赖

yum -y install curl policycoreutils-python openssh-server

#启动ssh服务&设置开机启动

systemctl enable sshd

systemctl start sshd
```

##### 2.安装postfix

```
yum -y install postfix

#启动postfix&设置开机启动

systemctl enable postfix

systemctl start postfix
```

##### 3.设置防火墙，开放ssh、http服务

```
firewall-cmd --add-service=ssh --permanet

firewall-cmd --add-service=http --permanet

#重载防火墙规则

firewall-cmd --reload
```

##### 4.添加Gitlab软件包存储库并下载Gitlab-ce软件包

```
#添加Gitlab软件包储存库

curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.rpm.sh | sudo bash

#安装gitlab-ce软件包

yum -y install gitlab-ce
```

##### 5.修改本地host，将域名指向服务器ip

```
vim /etc/hosts

#添加内容：192.168.29.138
```

##### 6.修改gitlab配置文件中的站点Url

```
vim /etc/gitlab/gitlab.rb

#修改内容：将external_url后面的值改为：http：//gitlab.xkm.com
```

##### 7.重新配置并启动gitlab

```
gitlab-ctl reconfigure
```

##### 8.访问http：//gitlab.xkm.com

创建仓库



#### 二、配置仓库



##### 操作：

##### 1.生成ssh的钥匙

```
ssh-keygen -t rsa 
```

##### 2.将新生成的key添加到ssh-agent中

```
eval "$(ssh-agent -s)"

ssh-add ~/.ssh/id_rsa
```

##### 3.将公钥提取出来，加到新建成的仓库中

复制粘贴到gitlab project中的ssh key中

##### 4.创建个人信息

```
git config --global user.name "xkm"

git config --global user.email "xkm@192.168.29.138"
```



#### 三、git的用法

-**clone**：git clone 仓库的Url       克隆仓库

```
-eg：git clone http://gitlab.xkm.com/root/gitlab.git
```



-**init**：git init         初始化一个仓库

```
-eg：cd /root/xkm

mkdir newproject

cd ./newproject

git init
```



-**add**：git add .（.表示all），git add 1.txt 2.txt 把工作区的文件缓存到暂存区，为commit做好准备



-**commit**：git commit -m “附加的信息”  把暂存区的文件提交到本地仓库

```
-eg：git commit -m “new profiles”
```



-**push**：git push origin master 本地仓库把更新后的仓库上传到远程仓库，请求合并

```
-eg：git add .

git commit -m "new profiles"

git push origin master
```



-**branch**：

```
-git branch littlexkm  #（littlexkm是创建的分支名字）创建分支

-git branch  #查看分支，带*的是当前所处的分支名字
```



-**diff**：

```
-git diff #查看暂存器和工作区的差别

-git diff --cached #已经暂存起来的文件(staged)和上次提交时的快照之间(HEAD)的差异
```



-**log**：git log 查看版本更新情况

```
-可以用git diff commitID commitID查看两版本之间的差别

-可以用git reset --harder commitID回滚到之前的版本
```



-**reflog**：git reflog  查看历史操作