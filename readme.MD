1 首先访问Travis CI 官网 走你
1.用github账号登陆Travis CI

2.在右上角你的账户名点击进入 account，在Repositories tab页点击Sync now同步你的github项目

3.选中项目将默认的off改变为on开启项目的持续集成


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