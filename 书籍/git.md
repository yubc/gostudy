1、设置用户名

git config --global user.name xxx

2、添加到本地

git init //初始化本地仓储

git add . //添加所有文件到本地仓储

git commit -m "feat:add"  //提交到本地

3、在github上创建仓储

4、生成密钥

打开Git Shell 输入如下命令：ssh-keygen -C "your@email.address" -t rsa

密钥路径在命令下方显示

5、设置github密钥

打开密钥生成路径在.ssh文件夹中用记事本打开id_rsa.pub文件把文件内容拷贝。把拷贝的密钥添加到github密钥设置中(需勾选复选框)。设置后保存

6、连接到github

git remote add origin https://github.com/test/Test.git(服务器上的地址)

出现错误：

fatal: remote origin already exists

则执行以下语句：

git remote rm origin

再次执行git remote add origin https://github.com/test/Test.git即可。

在执行git push origin master时，报错：

error:failed to push som refs to.......

则执行以下语句：

git pull origin master

先把远程服务器github上面的文件拉先来，再push 上去