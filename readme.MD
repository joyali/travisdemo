Travis CI 是目前新兴的开源持续集成构建项目，它与jenkins，GO的很明显的特别在于采用yaml格式，简洁清新独树一帜。目前大多数的github项目都已经移入到Travis CI的构建队列中，据说Travis CI每天运行超过4000次完整构建。对于做开源项目或者github的使用者，如果你的项目还没有加入Travis CI构建队列，那么我真的想对你说out了。


1 首先访问Travis CI 官网 走你 
1.1.用github账号登陆Travis CI 
1.2.在右上角你的账户名点击进入 account，在Repositories tab页点击Sync now同步你的github项目
1.3.选中项目将默认的off改变为on开启项目的持续集成

2. 在你项目的根目录建立一个.travis.yml文件，内容如下
language: node_js
node_js:
//0.6为Node.js的版本
- 0.6

3. 提交code到github，打开tracivs ci等待其同步并build
//提交全部修改
git add .
git commit -am "提交.travis.yml配置文件"
git push -u origin master


4. 安装travis客户端 并登陆生成加密文件
gem install travis
//windows need : https://rubyinstaller.org/downloads/
//自动登陆travis
travis login --auto

//拷贝私钥到你的项目目录
cp ~/.ssh/id_rsa 项目目录/

//生成秘钥文件id_rsa.enc提交到服务器
travis encrypt-file id_rsa --add

5. 新建目录.travis 在该目录里建一个 ssh_config 文件
User root
Host tstoon
HostName www.xxx.site
Port 54322
StrictHostKeyChecking no
IdentityFile ~/.ssh/id_rsa
IdentitiesOnly yes
Protocol 2
Compression yes
ServerAliveInterval 60
ServerAliveCountMax 20
LogLevel INFO


6. 本地公钥拷贝到远程主机让实现无密码登陆
ssh-copy-id -i ~/.ssh/id_rsa.pub www.your.site

生成秘钥：ssh-keygen -t rsa -C "your email"

7. 完整的.travis.yml
//通过http://lint.travis-ci.org/校验完整性
language: node_js
node_js:
- 7.7.1
after_script:
//构建完毕自动将代码分发到远程主机
- scp -o StrictHostKeyChecking=no app.js root@www.zhuji.com:/home/www/yidengtest
before_install:
//此句去掉即可 travis encrypt-file自动生成
- openssl aes-256-cbc -K $encrypted_204d80cb93e1_key -iv $encrypted_204d80cb93e1_iv
-in .travis/id_rsa.enc -out id_rsa -d
- mv id_rsa ~/.ssh -f
- chmod 600 ~/.ssh/id_rsa
# 配置 ssh
- eval $(ssh-agent)
- ssh-add ~/.ssh/id_rsa
- cp .travis/ssh_config ~/.ssh/config

将你的开源项目加入Travis CI队列吧，很容易让你的项目加入持续集成，持续构建队列。



http://www.cnblogs.com/zqzjs/p/6119750.html