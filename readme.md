## FIRE(Financial Independent & Retire Early)数据爬虫
- 项目主要解决个人日常理财平台的数据抓取功能, 方便后续自动化记账及数据分析工作

### 且慢
- mock login的设计实现
爬取数据和个人账户相关，在调用data api之前，需要拿到access_token，通过chrome抓包使用登录api的形式，伪装登录请求
详见`qieman/model/craw_assert.QiemanCraw._get_access_token()`实现

- x-sign的捕获及更新
在调用登录API时，服务端进行接口加密操作, 
使用chrome webdriver伪造请求, 拿到服务端的sign值, 
详见`qieman/model/craw_assert.QiemanCraw._get_sign()`实现


### deploy & automation

- 安装设置python 依赖
使用PY3，pip install -r requirements.txt

- 安装chrome webdriver
```
1, 设置yum repo, /etc/yum.repos.d/google-chrome.repo
[google-chrome]
name=google-chrome
baseurl=http://dl.google.com/linux/chrome/rpm/stable/x86_64
enabled=1
gpgcheck=1
gpgkey=https://dl.google.com/linux/linux_signing_key.pub

2, 安装chrome浏览器, yum install -y google-chrome-stable --nogpgcheck

3，从官网下载和浏览器版本匹配的驱动driver，解压安装，并设置到环境可访问
wget -N https://chromedriver.storage.googleapis.com/2.35/chromedriver_linux64.zip -P ~/

unzip ~/chromedriver_linux64.zip -d ~/
mv -f ~/chromedriver /usr/local/bin/chromedriver
chmod 0755 /usr/local/bin/chromedriver
export PATH="/usr/local/bin:$PATH";
```

- 安装数据库相关
使用Mysql进行存储, 具体安装可自行查找.

ddl 可参考`scripts/db.sql`

- 设置环境变量及配置文件
目前的设计比较粗糙，从一个配置文件中进行资源性参数的读取, 默认读取`${project_home}/config.cfg`文件

同时，支持服务器指定路径方式: 
1, 设置 export IS_PROD=1;
2, 配置文件路径为: ~/.fire/craw_config.cfg


- 设置自动化运行脚本
设置crontab自动运行
```shell script
0 8 * * 6 source /root/.zshrc; export IS_PROD=1; export PATH="/usr/local/bin:$PATH"; cd /root/codebase/fire_craw; python crawer.py >> /data/logs/qieman.log 2>&1
```


### changelog

- 2021-09-28 完成v1.0的且慢每日收益抓取入库

