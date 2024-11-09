---
title: Jumpserver部署文档官方版
author: Kubehan
type: post
date: 2020-03-30T05:17:07+08:00
url: /2057.html
views:
  - 874
  - 874
categories:
  - Linux运维

---
    环境：
    

    <span style="color:#404040; font-family:Consolas; font-size:9pt; background-color:yellow"># CentOS 7
    </span>

<span style="color:#404040; font-size:9pt"><span style="font-family:Consolas; background-color:yellow">$ setenforce 0 # </span><span style="font-family:宋体; background-color:yellow">临时关闭，重启后失效</span><span style="font-family:Consolas; background-color:yellow"><br /> </span></span>

<span style="color:#404040; font-size:9pt"><span style="font-family:Consolas; background-color:yellow">$ systemctl stop firewalld.service # </span><span style="font-family:宋体; background-color:yellow">临时关闭，重启后失效</span><span style="font-family:Consolas; background-color:yellow"><br /> </span></span>

 

<span style="color:#404040; font-size:9pt"><span style="font-family:Consolas; background-color:yellow"># </span><span style="font-family:宋体; background-color:yellow">修改字符集，否则可能报</span><span style="font-family:Consolas; background-color:yellow"> input/output error</span><span style="font-family:宋体; background-color:yellow">的问题，因为日志里打印了中文</span><span style="font-family:Consolas; background-color:yellow"><br /> </span></span>

<span style="color:#404040; font-family:Consolas; font-size:9pt; background-color:yellow">$ localedef -c -f UTF-8 -i zh_CN zh_CN.UTF-8<br /> </span>

<span style="color:#404040; font-family:Consolas; font-size:9pt; background-color:yellow">$ export LC_ALL=zh_CN.UTF-8<br /> </span>

<span style="color:#404040; font-family:Consolas; font-size:9pt"><span style="background-color:yellow">$ echo 'LANG="zh_CN.UTF-8"' > /etc/locale.conf</span><br /> </span>

准备Python3和Python虚拟环境 

    安装依赖包：
    

    <span style="color:#404040; font-family:Consolas; font-size:9pt"><span style="background-color:yellow">yum -y install wget sqlite-devel xz gcc automake zlib-devel openssl-devel epel-release git</span>
    				</span>

安装Python3.6： 

    <span style="color:#404040; font-family:Consolas; font-size:9pt"><span style="background-color:yellow">yum -y install python36 python36-devel</span>
    				</span>

建立Python虚拟环境 

因为 CentOS 6/7 自带的是 Python2，而 Yum 等工具依赖原来的 Python，为了不扰乱原来的环境我们来使用 Python 虚拟环境 

    <span style="color:#404040; font-family:Consolas; font-size:9pt; background-color:yellow">$ cd /opt
    </span>

    <span style="color:#404040; font-family:Consolas"><span style="font-size:9pt">$ python3 -m </span><span style="font-size:14pt">venv </span><span style="font-size:9pt">py3
    </span></span>

    <span style="color:#404040; font-family:Consolas; font-size:9pt; background-color:yellow">$ source /opt/py3/bin/activate
    </span>

 

    <span style="color:#404040; font-size:9pt"><span style="font-family:Consolas; background-color:yellow"># </span><span style="font-family:宋体; background-color:yellow">看到下面的提示符代表成功，以后运行</span><span style="font-family:Consolas; background-color:yellow"> Jumpserver </span><span style="font-family:宋体; background-color:yellow">都要先运行以上</span><span style="font-family:Consolas; background-color:yellow"> source </span><span style="font-family:宋体; background-color:yellow">命令，以下所有命令均在该虚拟环境中运行</span><span style="font-family:Consolas; background-color:yellow">
    					</span></span>

    <span style="color:#404040; font-family:Consolas; font-size:9pt"><span style="background-color:yellow">(py3) [root@localhost py3]</span>
    				</span>

懒癌解决办法：自动载入虚拟环境 

<span style="color:#404040"><span style="font-family:等线; background-color:#fcfcfc">此项仅为懒癌晚期的人员使用，防止运行</span><span style="font-family:Arial; background-color:#fcfcfc"> Jumpserver </span><span style="font-family:等线; background-color:#fcfcfc">时忘记载入</span><span style="font-family:Arial; background-color:#fcfcfc"> Python </span><span style="font-family:等线; background-color:#fcfcfc">虚拟环境导致程序无法运行。使用</span><span style="font-family:Arial; background-color:#fcfcfc">autoenv<br /> </span></span>

    <span style="color:#404040; font-family:Consolas; font-size:9pt; background-color:yellow">$ cd /opt
    </span>

    <span style="color:#404040; font-family:Consolas; font-size:9pt; background-color:yellow">$ git clone https://github.com/kennethreitz/autoenv.git
    </span>

    <span style="color:#404040; font-family:Consolas; font-size:9pt; background-color:yellow">$ echo 'source /opt/autoenv/activate.sh' >> ~/.bashrc
    </span>

    <span style="color:#404040; font-family:Consolas; font-size:9pt"><span style="background-color:yellow">$ source ~/.bashrc</span>
    				</span>

安装jumpserver 

    <span style="color:#404040; font-family:Consolas; font-size:9pt; background-color:yellow">$ cd /opt/
    </span>

    <span style="color:#404040; font-family:Consolas; font-size:9pt; background-color:yellow">$ git clone https://github.com/jumpserver/jumpserver.git && cd jumpserver && git checkout master
    </span>

    <span style="color:#404040; font-size:9pt"><span style="font-family:Consolas; background-color:yellow">$ echo "source /opt/py3/bin/activate" > /opt/jumpserver/.env  # </span><span style="font-family:宋体; background-color:yellow">进入</span><span style="font-family:Consolas; background-color:yellow"> jumpserver </span><span style="font-family:宋体; background-color:yellow">目录时将自动载入</span><span style="font-family:Consolas; background-color:yellow"> python </span><span style="font-family:宋体; background-color:yellow">虚拟环境</span><span style="font-family:Consolas; background-color:yellow">
    					</span></span>

    <span style="color:#404040; font-size:9pt"><span style="font-family:Consolas; background-color:yellow"># </span><span style="font-family:宋体; background-color:yellow">首次进入</span><span style="font-family:Consolas; background-color:yellow"> jumpserver </span><span style="font-family:宋体; background-color:yellow">文件夹会有提示，按</span><span style="font-family:Consolas; background-color:yellow"> y </span><span style="font-family:宋体; background-color:yellow">即可</span><span style="font-family:Consolas; background-color:yellow">
    					</span></span>

    <span style="color:#404040; font-family:Consolas; font-size:9pt"><span style="background-color:yellow"># Are you sure you want to allow this? (y/N) y</span>
    				</span>

安装依赖RPM包 

    <span style="color:#404040; font-family:Consolas; font-size:9pt; background-color:yellow">$ cd /opt/jumpserver/requirements
    </span>

    <span style="color:#404040; font-size:9pt"><span style="font-family:Consolas; background-color:yellow">$ yum -y install $(cat rpm_requirements.txt)  # </span><span style="font-family:宋体; background-color:yellow">如果没有任何报错请继续</span><span style="font-family:Consolas">
    					</span></span>

<span style="color:#404040"><strong><span style="font-family:Arial; background-color:#fcfcfc"> </span><span style="font-family:等线; background-color:#fcfcfc">安装</span><span style="font-family:Arial; background-color:#fcfcfc"> Python </span><span style="font-family:等线; background-color:#fcfcfc">库依赖</span></strong></span> 

    <span style="color:#404040; font-family:Consolas; font-size:9pt"><span style="background-color:yellow">$ pip install -r requirements.txt</span>
    				</span>

<span style="color:#404040"><strong><span style="font-family:等线; background-color:#fcfcfc">安装</span><span style="font-family:Arial; background-color:#fcfcfc"> Redis, Jumpserver </span><span style="font-family:等线; background-color:#fcfcfc">使用</span><span style="font-family:Arial; background-color:#fcfcfc"> Redis </span><span style="font-family:等线; background-color:#fcfcfc">做</span><span style="font-family:Arial; background-color:#fcfcfc"> cache </span><span style="font-family:等线; background-color:#fcfcfc">和</span><span style="font-family:Arial; background-color:#fcfcfc"> celery broke</span></strong></span> 

    <span style="color:#404040; font-family:Consolas; font-size:9pt; background-color:yellow">$ yum -y install redis
    </span>

    <span style="color:#404040; font-family:Consolas; font-size:9pt; background-color:yellow">$ systemctl enable redis
    </span>

    <span style="color:#404040; font-family:Consolas; font-size:9pt"><span style="background-color:yellow">$ systemctl start redis</span>
    				</span>

<span style="color:#404040"><strong><span style="font-family:等线; background-color:#fcfcfc">安装</span><span style="font-family:Arial; background-color:#fcfcfc"> MySQL</span></strong></span> 

安装过程就过了，用原来的就行 

<p style="background: #fcfcfc">
  <span style="color:#404040; font-size:12pt"><span style="font-family:宋体">本教程使用</span><span style="font-family:Arial"> Mysql </span><span style="font-family:宋体">作为数据库，如果不使用</span><span style="font-family:Arial"> Mysql </span><span style="font-family:宋体">可以跳过相关</span><span style="font-family:Arial"> Mysql </span><span style="font-family:宋体">安装和配置</span><span style="font-family:Arial"><br /> </span></span>
</p>

<p style="background: #eeffcc">
  <span style="color:#404040; font-family:Consolas; font-size:9pt"># centos7<br /> </span>
</p>

<p style="background: #eeffcc">
  <span style="color:#404040; font-size:9pt"><span style="font-family:Consolas">$ yum -y install mariadb mariadb-devel mariadb-server # centos7</span><span style="font-family:宋体">下安装的是</span><span style="font-family:Consolas">mariadb<br /> </span></span>
</p>

<p style="background: #eeffcc">
  <span style="color:#404040; font-family:Consolas; font-size:9pt">$ systemctl enable mariadb<br /> </span>
</p>

<p style="background: #eeffcc">
  <span style="color:#404040; font-family:Consolas; font-size:9pt">$ systemctl start mariadb<br /> </span>
</p>

<p style="background: #eeffcc">
   
</p>

<p style="background: #eeffcc">
  <span style="color:#404040; font-size:9pt"><span style="font-family:Consolas"># centos6 </span><span style="font-family:宋体">自带的</span><span style="font-family:Consolas"> mysql5.1 </span><span style="font-family:宋体">不支持，请在其他服务器上创建</span><span style="font-family:Consolas"> jumpserver </span><span style="font-family:宋体">数据库连接</span><span style="font-family:Consolas"><br /> </span></span>
</p>

 

<span style="color:#404040"><strong><span style="font-family:等线; background-color:#fcfcfc">创建数据库</span><span style="font-family:Arial; background-color:#fcfcfc"> Jumpserver </span><span style="font-family:等线; background-color:#fcfcfc">并授权</span></strong></span> 

    <span style="color:#404040; font-family:Consolas; font-size:9pt; background-color:yellow">$ mysql
    </span>

    <span style="color:#404040; font-family:Consolas; font-size:9pt; background-color:yellow">> create database jumpserver default charset 'utf8';
    </span>

    <span style="color:#404040; font-family:Consolas; font-size:9pt; background-color:yellow">> grant all on jumpserver.* to 'jumpserver'@'127.0.0.1' identified by 'weakPassword';
    </span>

    <span style="color:#404040; font-family:Consolas; font-size:9pt"><span style="background-color:yellow">> flush privileges;</span>
    				</span>

<span style="color:#404040"><strong><span style="font-family:Arial; background-color:#fcfcfc"> </span><span style="font-family:等线; background-color:#fcfcfc">修改</span><span style="font-family:Arial; background-color:#fcfcfc"> Jumpserver </span><span style="font-family:等线; background-color:#fcfcfc">配置文件</span></strong></span> 

    <span style="color:#404040; font-family:Consolas; font-size:9pt; background-color:yellow">$ cd /opt/jumpserver
    </span>

    <span style="color:#404040; font-family:Consolas; font-size:9pt; background-color:yellow">$ cp config_example.py config.py
    </span>

    <span style="color:#404040; font-family:Consolas; font-size:9pt"><span style="background-color:yellow">$ vi config.py</span>
    				</span>

<span style="background-color:red">这里特别需要注意：更改的地方要完全对整齐，不能用tab建，只能用空格键，并且要完全对齐，</span> 

注意更改的地方如下 

<span style="color:#408090; font-size:9pt"><em><span style="font-family:Consolas; background-color:yellow"># Jumpserver </span><span style="font-family:宋体; background-color:yellow">使用</span><span style="font-family:Consolas; background-color:yellow"> SECRET_KEY </span><span style="font-family:宋体; background-color:yellow">进行加密，请务必修改以下设置</span></em><span style="color:#404040; font-family:Consolas"><br /> </span></span>

<span style="color:#404040; font-family:Consolas; font-size:9pt"><br /> <span style="color:#408090"><em># SECRET_KEY = os.environ.get('SECRET_KEY') or '2vym+ky!997d5kkcc64mnz06y1mmui3lut#(^wd=%s_qj$1%x'</em><span style="color:#404040"><br /> </span></span></span>

<span style="color:#404040; font-size:9pt"><span style="font-family:Consolas"> SECRET_KEY <span style="color:#666666">=<span style="color:#404040"><br /> <span style="color:#4070a0">'</span></span></span></span><span style="font-family:宋体">请随意输入随机字符串（推荐字符大于等于</span><span style="color:#4070a0"><span style="font-family:Consolas"> 50</span><span style="font-family:宋体">位）</span><span style="font-family:Consolas">'<span style="color:#404040"><br /> </span></span></span></span>

 

<span style="color:#404040; font-size:9pt"><span style="font-family:Consolas"><br /> <span style="color:#408090; background-color:yellow"><em># DEBUG </em></span></span><span style="font-family:宋体; background-color:yellow"><em>模式</em></span><span style="color:#408090"><em><span style="font-family:Consolas; background-color:yellow"> True</span><span style="font-family:宋体; background-color:yellow">为开启</span><span style="font-family:Consolas; background-color:yellow"> False</span><span style="font-family:宋体; background-color:yellow">为关闭，默认开启，生产环境推荐关闭</span></em><span style="color:#404040; font-family:Consolas; background-color:yellow"><br /> </span></span></span>

<p style="margin-left: 27pt">
  <span style="color:#404040; font-size:9pt"><span style="font-family:Consolas; background-color:yellow"><br /> <span style="color:#408090"><em># </em></span></span><span style="font-family:宋体; background-color:yellow"><em>注意：如果设置了</em></span><span style="color:#408090"><em><span style="font-family:Consolas; background-color:yellow">DEBUG = False</span><span style="font-family:宋体; background-color:yellow">，访问</span><span style="font-family:Consolas; background-color:yellow">8080</span><span style="font-family:宋体; background-color:yellow">端口页面会显示不正常，需要搭建</span><span style="font-family:Consolas; background-color:yellow"> nginx </span><span style="font-family:宋体; background-color:yellow">代理才可以正常访问</span></em><span style="color:#404040; font-family:Consolas"><br /> </span></span></span>
</p>

     <span style="color:#404040; font-family:Consolas; font-size:9pt">DEBUG <span style="color:#666666">=<span style="color:#404040"> os<span style="color:#666666">.<span style="color:#404040">environ<span style="color:#666666">.<span style="color:#404040">get(<span style="color:#4070a0">"DEBUG"<span style="color:#404040">) <span style="color:#007020"><strong>or</strong><span style="color:#404040"><br /> <span style="color:#007020"><strong>True</strong><span style="color:#404040"><br /> </span></span></span></span></span></span></span></span></span></span></span></span></span>

<span style="color:#408090; font-size:9pt"><em><span style="font-family:Consolas; background-color:yellow"># </span><span style="font-family:宋体; background-color:yellow">日志级别，默认为</span><span style="font-family:Consolas; background-color:yellow">DEBUG</span><span style="font-family:宋体; background-color:yellow">，可调整为</span><span style="font-family:Consolas; background-color:yellow">INFO, WARNING, ERROR, CRITICAL</span><span style="font-family:宋体; background-color:yellow">，默认</span></em><span style="font-family:Consolas"><span style="background-color:yellow"><em>INFO</em></span><span style="color:#404040"><br /> </span></span></span>

    <span style="color:#404040; font-family:Consolas; font-size:9pt">LOG_LEVEL <span style="color:#666666">=<span style="color:#404040"> os<span style="color:#666666">.<span style="color:#404040">environ<span style="color:#666666">.<span style="color:#404040">get(<span style="color:#4070a0">"LOG_LEVEL"<span style="color:#404040">) <span style="color:#007020"><strong>or</strong><span style="color:#404040"><br /> <span style="color:#4070a0">'WARNING'<span style="color:#404040"><br /> </span></span></span></span></span></span></span></span></span></span></span></span></span>

    <span style="color:#404040; font-family:Consolas; font-size:9pt">LOG_DIR <span style="color:#666666">=<span style="color:#404040"> os<span style="color:#666666">.<span style="color:#404040">path<span style="color:#666666">.<span style="color:#404040">join(BASE_DIR, <span style="color:#4070a0">'logs'<span style="color:#404040">)<br /> </span></span></span></span></span></span></span></span></span>

 

<span style="color:#408090; font-size:9pt"><em><span style="font-family:Consolas; background-color:yellow"># </span><span style="font-family:宋体; background-color:yellow">默认使用</span><span style="font-family:Consolas; background-color:yellow">SQLite3</span><span style="font-family:宋体; background-color:yellow">，如果使用其他数据库请注释下面两行</span></em><span style="color:#404040; font-family:Consolas"><br /> </span></span>

<span style="color:#404040; font-family:Consolas; font-size:9pt"><br /> <span style="color:#408090"><em># DB_ENGINE = 'sqlite3'</em><span style="color:#404040"><br /> </span></span></span>

<span style="color:#404040; font-family:Consolas; font-size:9pt"><br /> <span style="color:#408090"><em># DB_NAME = os.path.join(BASE_DIR, 'data', 'db.sqlite3')</em><span style="color:#404040"><br /> </span></span></span>

 

<p style="background: yellow">
  <span style="color:#404040; font-size:9pt"><span style="font-family:Consolas"><br /> <span style="color:#408090; background-color:yellow"><em># </em></span></span><span style="font-family:宋体; background-color:yellow"><em>如果需要使用</em></span><span style="color:#408090"><em><span style="font-family:Consolas; background-color:yellow">mysql</span><span style="font-family:宋体; background-color:yellow">或</span><span style="font-family:Consolas; background-color:yellow">postgres</span><span style="font-family:宋体; background-color:yellow">，请取消下面的注释并输入正确的信息</span><span style="font-family:Consolas; background-color:yellow">,</span><span style="font-family:宋体; background-color:yellow">本例使用</span><span style="font-family:Consolas; background-color:yellow">mysql</span><span style="font-family:宋体; background-color:yellow">做演示</span><span style="font-family:Consolas; background-color:yellow">(mariadb</span><span style="font-family:宋体; background-color:yellow">也是</span></em><span style="font-family:Consolas"><span style="background-color:yellow"><em>mysql)</em></span><span style="color:#404040"><br /> </span></span></span></span>
</p>

<p style="background: yellow">
  <span style="color:#404040; font-family:Consolas; font-size:9pt"> DB_ENGINE <span style="color:#666666">=<span style="color:#404040"> os<span style="color:#666666">.<span style="color:#404040">environ<span style="color:#666666">.<span style="color:#404040">get(<span style="color:#4070a0">"DB_ENGINE"<span style="color:#404040">) <span style="color:#007020"><strong>or</strong><span style="color:#404040"><br /> <span style="color:#4070a0">'mysql'<span style="color:#404040"><br /> </span></span></span></span></span></span></span></span></span></span></span></span></span>
</p>

<p style="background: yellow">
  <span style="color:#404040; font-family:Consolas; font-size:9pt"> DB_HOST <span style="color:#666666">=<span style="color:#404040"> os<span style="color:#666666">.<span style="color:#404040">environ<span style="color:#666666">.<span style="color:#404040">get(<span style="color:#4070a0">"DB_HOST"<span style="color:#404040">) <span style="color:#007020"><strong>or</strong><span style="color:#404040"><br /> <span style="color:#4070a0">'127.0.0.1'<span style="color:#404040"><br /> </span></span></span></span></span></span></span></span></span></span></span></span></span>
</p>

<p style="background: yellow">
  <span style="color:#404040; font-family:Consolas; font-size:9pt"> DB_PORT <span style="color:#666666">=<span style="color:#404040"> os<span style="color:#666666">.<span style="color:#404040">environ<span style="color:#666666">.<span style="color:#404040">get(<span style="color:#4070a0">"DB_PORT"<span style="color:#404040">) <span style="color:#007020"><strong>or</strong><span style="color:#404040"><br /> <span style="color:#208050">3306<span style="color:#404040"><br /> </span></span></span></span></span></span></span></span></span></span></span></span></span>
</p>

<p style="background: yellow">
  <span style="color:#404040; font-family:Consolas; font-size:9pt"> DB_USER <span style="color:#666666">=<span style="color:#404040"> os<span style="color:#666666">.<span style="color:#404040">environ<span style="color:#666666">.<span style="color:#404040">get(<span style="color:#4070a0">"DB_USER"<span style="color:#404040">) <span style="color:#007020"><strong>or</strong><span style="color:#404040"><br /> <span style="color:#4070a0">'jumpserver'<span style="color:#404040"><br /> </span></span></span></span></span></span></span></span></span></span></span></span></span>
</p>

<p style="background: yellow">
  <span style="color:#404040; font-family:Consolas; font-size:9pt"> DB_PASSWORD <span style="color:#666666">=<span style="color:#404040"> os<span style="color:#666666">.<span style="color:#404040">environ<span style="color:#666666">.<span style="color:#404040">get(<span style="color:#4070a0">"DB_PASSWORD"<span style="color:#404040">) <span style="color:#007020"><strong>or</strong><span style="color:#404040"><br /> <span style="color:#4070a0">'weakPassword'<span style="color:#404040"><br /> </span></span></span></span></span></span></span></span></span></span></span></span></span>
</p>

<p style="background: yellow">
  <span style="color:#404040; font-family:Consolas; font-size:9pt"> DB_NAME <span style="color:#666666">=<span style="color:#404040"> os<span style="color:#666666">.<span style="color:#404040">environ<span style="color:#666666">.<span style="color:#404040">get(<span style="color:#4070a0">"DB_NAME"<span style="color:#404040">) <span style="color:#007020"><strong>or</strong><span style="color:#404040"><br /> <span style="color:#4070a0">'jumpserver'<span style="color:#404040"><br /> </span></span></span></span></span></span></span></span></span></span></span></span></span>
</p>

 

<span style="color:#404040; font-size:9pt"><span style="font-family:Consolas"><br /> <span style="color:#408090; background-color:yellow"><em># Django </em></span></span><span style="font-family:宋体; background-color:yellow"><em>监听的</em></span><span style="color:#408090"><em><span style="font-family:Consolas; background-color:yellow">ip</span><span style="font-family:宋体; background-color:yellow">和端口，生产环境推荐把</span><span style="font-family:Consolas; background-color:yellow">0.0.0.0</span><span style="font-family:宋体; background-color:yellow">修改成</span><span style="font-family:Consolas; background-color:yellow">127.0.0.1</span><span style="font-family:宋体; background-color:yellow">，这里的意思是允许</span><span style="font-family:Consolas; background-color:yellow">x.x.x.x</span><span style="font-family:宋体; background-color:yellow">访问，</span><span style="font-family:Consolas; background-color:yellow">127.0.0.1</span><span style="font-family:宋体; background-color:yellow">表示仅允许自身访问</span></em><span style="color:#404040; font-family:Consolas"><br /> </span></span></span>

<p style="background: #92d050">
  <span style="color:#404040; font-family:Consolas; font-size:9pt"><br /> <span style="color:#408090"><em># ./manage.py runserver 127.0.0.1:8080</em><span style="color:#404040"><br /> </span></span></span>
</p>

<p style="background: #92d050">
  <span style="color:#404040; font-family:Consolas; font-size:9pt"> HTTP_BIND_HOST <span style="color:#666666">=<span style="color:#404040"><br /> <span style="color:#4070a0">'0.0.0.0'<span style="color:#404040"><br /> </span></span></span></span></span>
</p>

<p style="background: #92d050">
  <span style="color:#404040; font-family:Consolas; font-size:9pt">HTTP_LISTEN_PORT <span style="color:#666666">=<span style="color:#404040"><br /> <span style="color:#208050">8080<span style="color:#404040"><br /> </span></span></span></span></span>
</p>

 

<span style="color:#404040"><strong><span style="font-family:Arial; background-color:#fcfcfc"> </span><span style="font-family:等线; background-color:#fcfcfc">生成数据库表结构和初始化数据</span><span style="font-family:Arial; background-color:#fcfcfc"><br /> </span></strong></span>

    <span style="color:#404040; font-family:Consolas; font-size:9pt; background-color:yellow">$ cd /opt/jumpserver/utils
    </span>

    <span style="color:#404040; font-family:Consolas; font-size:9pt"><span style="background-color:yellow">$ sh make_migrations.sh</span>
    				</span>

<span style="color:#404040"><strong><span style="font-family:等线; background-color:#fcfcfc">运行</span><span style="font-family:Arial; background-color:#fcfcfc"> Jumpserver</span></strong></span> 

    <span style="color:#404040; font-family:Consolas; font-size:9pt; background-color:yellow">$ cd /opt/jumpserver
    </span>

    <span style="color:#404040; font-size:9pt"><span style="font-family:Consolas; background-color:yellow">$ ./jms start all  # </span><span style="font-family:宋体; background-color:yellow">后台运行使用</span><span style="font-family:Consolas; background-color:yellow"> -d </span><span style="font-family:宋体; background-color:yellow">参数</span><span style="font-family:Consolas"><span style="background-color:yellow">./jms start all -d</span>
    					</span></span>

到这里登录jumpserver时可能会会无法登录。需要手动设置超级用户如下设置 

<span style="color:#404040"><span style="font-family:等线; background-color:#fcfcfc">运行不报错，请浏览器访问</span><span style="font-family:Arial; background-color:#fcfcfc"> <a href="http://192.168.244.144:8080/"><span style="color:#9b59b6">http://192.168.244.144:8080/</span></a> </span><span style="font-family:等线; background-color:#fcfcfc">默认账号</span><span style="font-family:Arial; background-color:#fcfcfc">: admin </span><span style="font-family:等线; background-color:#fcfcfc">密码</span><span style="font-family:Arial; background-color:#fcfcfc">: admin </span><span style="font-family:等线; background-color:#fcfcfc">页面显示不正常先不用处理，继续往下操作，后面搭建</span><span style="font-family:Arial; background-color:#fcfcfc"> nginx </span><span style="font-family:等线; background-color:#fcfcfc">代理后即可正常访问，原因是因为</span><span style="font-family:Arial; background-color:#fcfcfc"> django </span><span style="font-family:等线; background-color:#fcfcfc">无法在非</span><span style="font-family:Arial; background-color:#fcfcfc"> debug </span><span style="font-family:等线; background-color:#fcfcfc">模式下加载静态资源</span></span> 

  1. <div style="background: #fcfcfc">
      <span style="color:#404040; font-size:12pt"><span style="font-family:宋体">在终端修改管理员密码及新建超级用户</span><span style="font-family:Arial"><br /> </span></span>
    </div>
    
    <p style="background: #eeffcc">
      <span style="color:#404040; font-size:9pt"><span style="font-family:Consolas"># </span><span style="font-family:宋体">管理密码忘记了或者重置管理员密码</span><span style="font-family:Consolas"><br /> </span></span>
    </p>
    
    <p style="background: #eeffcc">
      <span style="color:#404040; font-family:Consolas; font-size:9pt">$ source /opt/py3/bin/activate<br /> </span>
    </p>
    
    <p style="background: #eeffcc">
      <span style="color:#404040; font-family:Consolas; font-size:9pt">$ cd /opt/jumpserver/apps<br /> </span>
    </p>
    
    <p style="background: #eeffcc">
      <span style="color:#404040; font-family:Consolas; font-size:9pt">$ python manage.py changepassword <user_name><br /> </span>
    </p>
    
    <p style="background: #eeffcc">
       
    </p>
    
    <p style="background: #eeffcc">
      <span style="color:#404040; font-size:9pt"><span style="font-family:Consolas"># </span><span style="font-family:宋体">新建超级用户的命令如下命令</span><span style="font-family:Consolas"><br /> </span></span>
    </p>
    
    <p style="background: #eeffcc">
      <span style="color:#404040; font-family:Consolas; font-size:9pt">$ python manage.py createsuperuser --username=user --email=user@domain.com<br /> </span>
    </p>
    
    <p style="text-align: justify; background: #fcfcfc">
      <h2>
        <span style="color:#404040"><span style="font-family:等线 Light">三</span><span style="font-family:Georgia">. </span><span style="font-family:等线 Light">安装</span><span style="font-family:Georgia"> SSH Server </span><span style="font-family:等线 Light">和</span><span style="font-family:Georgia"> WebSocket Server: Coco<br /> </span></span>
      </h2>
    </p>
    
    为啥要安装？ 
    
    Coco 
    
    实现了 SSH Server 和 Web Terminal Server 的组件，提供 SSH 和 WebSocket 接口, 使用 Paramiko 和 Flask 开发。 
    
    <span style="color:#404040"><span style="font-family:等线">新开一个终端，别忘了</span><span style="font-family:Arial"> source /opt/py3/bin/activate</span></span> 
    
    <p style="background: #00b050">
      <pre><code>&lt;span style="color:#404040; font-family:Consolas; font-size:9pt; background-color:yellow">$ cd /opt
&lt;/span></code></pre>
</p>

<p style="background: #00b050">
  <pre><code>&lt;span style="color:#404040; font-family:Consolas; font-size:9pt; background-color:yellow">$ source /opt/py3/bin/activate
&lt;/span></code></pre>
</p>

<p style="background: #00b050">
  <pre><code>&lt;span style="color:#404040; font-family:Consolas; font-size:9pt; background-color:yellow">$ git clone https://github.com/jumpserver/coco.git && cd coco && git checkout master
&lt;/span></code></pre>
</p>

<p style="background: #00b050">
  <pre><code>&lt;span style="color:#404040; font-size:9pt">&lt;span style="font-family:Consolas; background-color:yellow">$ echo "source /opt/py3/bin/activate" &gt; /opt/coco/.env  # &lt;/span>&lt;span style="font-family:宋体; background-color:yellow">进入&lt;/span>&lt;span style="font-family:Consolas; background-color:yellow"> coco &lt;/span>&lt;span style="font-family:宋体; background-color:yellow">目录时将自动载入&lt;/span>&lt;span style="font-family:Consolas; background-color:yellow"> python &lt;/span>&lt;span style="font-family:宋体; background-color:yellow">虚拟环境&lt;/span>&lt;span style="font-family:Consolas; background-color:yellow">
							&lt;/span>&lt;/span></code></pre>
</p>

<p style="background: #00b050">
  <pre><code>&lt;span style="color:#404040; font-size:9pt">&lt;span style="font-family:Consolas; background-color:yellow"># &lt;/span>&lt;span style="font-family:宋体; background-color:yellow">首次进入&lt;/span>&lt;span style="font-family:Consolas; background-color:yellow"> coco &lt;/span>&lt;span style="font-family:宋体; background-color:yellow">文件夹会有提示，按&lt;/span>&lt;span style="font-family:Consolas; background-color:yellow"> y &lt;/span>&lt;span style="font-family:宋体; background-color:yellow">即可&lt;/span>&lt;span style="font-family:Consolas; background-color:yellow">
							&lt;/span>&lt;/span></code></pre>
</p>

    <span style="color:#404040; font-family:Consolas; font-size:9pt"><span style="background-color:yellow"># Are you sure you want to allow this? (y/N) y</span>
    						</span>

安装依赖包 

    <span style="color:#404040; font-family:Consolas; font-size:9pt; background-color:yellow">$ cd /opt/coco/requirements
    </span>

    <span style="color:#404040; font-family:Consolas; font-size:9pt; background-color:yellow">$ <em>yum -y  install $(cat rpm_requirements.txt)</em>
    						</span>

    <span style="color:#404040; font-family:Consolas; font-size:9pt"><span style="background-color:yellow">$ pip install -r requirements.txt</span>
    						</span>

    <span style="color:#404040; font-size:9pt"><span style="font-family:宋体">修改配置文件并运行</span><span style="font-family:Consolas">
    							</span></span>

    <span style="color:#404040; font-family:Consolas; font-size:9pt; background-color:yellow">$ cd /opt/coco
    </span>

    <span style="color:#404040; font-family:Consolas; font-size:9pt; background-color:yellow">$ mkdir keys logs
    </span>

    <span style="color:#404040; font-size:9pt"><span style="font-family:Consolas; background-color:yellow">$ cp conf_example.py conf.py  # </span><span style="font-family:宋体; background-color:yellow">如果</span><span style="font-family:Consolas; background-color:yellow"> coco </span><span style="font-family:宋体; background-color:yellow">与</span><span style="font-family:Consolas; background-color:yellow"> jumpserver </span><span style="font-family:宋体; background-color:yellow">分开部署，请手动修改</span><span style="font-family:Consolas; background-color:yellow"> conf.py
    </span></span>

    <span style="color:#404040; font-family:Consolas; font-size:9pt"><span style="background-color:yellow">$ vi conf.py</span>
    						</span>

到这里如果是根据文档安装的就不需要更改内容直接启动就行 

    <span style="color:#404040; font-size:9pt"><span style="font-family:Consolas">$ ./cocod start  # </span><span style="font-family:宋体">后台运行使用</span><span style="font-family:Consolas"> -d </span><span style="font-family:宋体">参数</span><span style="font-family:Consolas">./cocod start -d
    </span></span>

<span style="color:#404040"><span style="font-family:等线; background-color:#fcfcfc">启动成功后去</span><span style="font-family:Arial; background-color:#fcfcfc">Jumpserver </span><span style="font-family:等线; background-color:#fcfcfc">会话管理</span><span style="font-family:Arial; background-color:#fcfcfc">-</span><span style="font-family:等线; background-color:#fcfcfc">终端管理接受</span><span style="font-family:Arial; background-color:#fcfcfc">coco</span><span style="font-family:等线; background-color:#fcfcfc">的注册</span></span> 

注册后就可以使用2222端口登录跳板机 

<p style="text-align: justify; background: #fcfcfc">
  <h2>
    <span style="color:#404040"><span style="font-family:等线 Light">安装</span><span style="font-family:Georgia"> Web Terminal </span><span style="font-family:等线 Light">前端</span><span style="font-family:Georgia">: Luna<br /> </span></span>
  </h2>
</p>

如果不需要web端这里可以不安装 

    <span style="color:#404040; font-family:Consolas; font-size:9pt">$ cd /opt
    </span>

    <span style="color:#404040; font-family:Consolas; font-size:9pt">$ wget https://github.com/jumpserver/luna/releases/download/1.4.3/luna.tar.gz
    </span>

    <span style="color:#404040; font-family:Consolas; font-size:9pt">$ tar xvf luna.tar.gz
    </span>

    <span style="color:#404040; font-family:Consolas; font-size:9pt">$ chown -R root:root luna
    </span>

配置nginx整合组建 

安装nginx省略 

<span style="color:#404040"><strong><span style="font-family:等线; background-color:#fcfcfc">准备配置文件</span><span style="font-family:Arial; background-color:#fcfcfc"><br /> </span><span style="font-family:等线; background-color:#fcfcfc">修改</span><span style="font-family:Arial; background-color:#fcfcfc"> /etc/nginx/conf.d/jumpserver.conf<br /> </span></strong></span>

    <span style="color:#404040; font-family:Consolas; font-size:9pt">$ vim /etc/nginx/conf.d/jumpserver.conf
    </span>

<p style="background: #92d050">
  <pre><code>&lt;span style="color:#404040; font-size:9pt">&lt;span style="font-family:Consolas"># &lt;/span>&lt;span style="font-family:宋体">注意注释&lt;/span>&lt;span style="font-family:Consolas"> nginx.conf &lt;/span>&lt;span style="font-family:宋体">里面的&lt;/span>&lt;span style="font-family:Consolas"> server {} &lt;/span>&lt;span style="font-family:宋体">内容&lt;/span>&lt;span style="font-family:Consolas">
							&lt;/span>&lt;span style="font-family:宋体">，&lt;/span>&lt;span style="font-family:Consolas">CentOS 6 &lt;/span>&lt;span style="font-family:宋体">需要修改文件&lt;/span>&lt;span style="font-family:Consolas"> /etc/nginx/cond.f/default.conf
&lt;/span>&lt;/span></code></pre>
</p>

<p style="background: #92d050">
   
</p>

<p style="background: #92d050">
  <pre><code>&lt;span style="color:#404040; font-family:Consolas; font-size:9pt">server {
&lt;/span></code></pre>
</p>

<p style="background: #92d050">
  <pre><code>&lt;span style="color:#404040; font-size:9pt">&lt;span style="font-family:Consolas">    listen 80;  # &lt;/span>&lt;span style="font-family:宋体">代理端口，以后将通过此端口进行访问，不再通过&lt;/span>&lt;span style="font-family:Consolas">8080&lt;/span>&lt;span style="font-family:宋体">端口&lt;/span>&lt;span style="font-family:Consolas">
							&lt;/span>&lt;/span></code></pre>
</p>

<p style="background: #92d050">
  <pre><code>&lt;span style="color:#404040; font-size:9pt">&lt;span style="font-family:Consolas">    server_name demo.jumpserver.org;  # &lt;/span>&lt;span style="font-family:宋体">修改成你的域名&lt;/span>&lt;span style="font-family:Consolas">
							&lt;/span>&lt;/span></code></pre>
</p>

<p style="background: #92d050">
   
</p>

<p style="background: #92d050">
  <pre><code>&lt;span style="color:#404040; font-size:9pt">&lt;span style="font-family:Consolas">    client_max_body_size 100m;  # &lt;/span>&lt;span style="font-family:宋体">录像及文件上传大小限制&lt;/span>&lt;span style="font-family:Consolas">
							&lt;/span>&lt;/span></code></pre>
</p>

<p style="background: #92d050">
   
</p>

<p style="background: #92d050">
  <pre><code>&lt;span style="color:#404040; font-family:Consolas; font-size:9pt">    location /luna/ {
&lt;/span></code></pre>
</p>

<p style="background: #92d050">
  <pre><code>&lt;span style="color:#404040; font-family:Consolas; font-size:9pt">        try_files $uri / /index.html;
&lt;/span></code></pre>
</p>

<p style="background: #92d050">
  <pre><code>&lt;span style="color:#404040; font-size:9pt">&lt;span style="font-family:Consolas">        alias /opt/luna/;  # luna &lt;/span>&lt;span style="font-family:宋体">路径，如果修改安装目录，此处需要修改&lt;/span>&lt;span style="font-family:Consolas">
							&lt;/span>&lt;/span></code></pre>
</p>

<p style="background: #92d050">
  <pre><code>&lt;span style="color:#404040; font-family:Consolas; font-size:9pt">    }
&lt;/span></code></pre>
</p>

<p style="background: #92d050">
   
</p>

<p style="background: #92d050">
  <pre><code>&lt;span style="color:#404040; font-family:Consolas; font-size:9pt">    location /media/ {
&lt;/span></code></pre>
</p>

<p style="background: #92d050">
  <pre><code>&lt;span style="color:#404040; font-family:Consolas; font-size:9pt">        add_header Content-Encoding gzip;
&lt;/span></code></pre>
</p>

<p style="background: #92d050">
  <pre><code>&lt;span style="color:#404040; font-size:9pt">&lt;span style="font-family:Consolas">        root /opt/jumpserver/data/;  # &lt;/span>&lt;span style="font-family:宋体">录像位置，如果修改安装目录，此处需要修改&lt;/span>&lt;span style="font-family:Consolas">
							&lt;/span>&lt;/span></code></pre>
</p>

<p style="background: #92d050">
  <pre><code>&lt;span style="color:#404040; font-family:Consolas; font-size:9pt">    }
&lt;/span></code></pre>
</p>

<p style="background: #92d050">
   
</p>

<p style="background: #92d050">
  <pre><code>&lt;span style="color:#404040; font-family:Consolas; font-size:9pt">    location /static/ {
&lt;/span></code></pre>
</p>

<p style="background: #92d050">
  <pre><code>&lt;span style="color:#404040; font-size:9pt">&lt;span style="font-family:Consolas">        root /opt/jumpserver/data/;  # &lt;/span>&lt;span style="font-family:宋体">静态资源，如果修改安装目录，此处需要修改&lt;/span>&lt;span style="font-family:Consolas">
							&lt;/span>&lt;/span></code></pre>
</p>

<p style="background: #92d050">
  <pre><code>&lt;span style="color:#404040; font-family:Consolas; font-size:9pt">    }
&lt;/span></code></pre>
</p>

<p style="background: #92d050">
   
</p>

<p style="background: #92d050">
  <pre><code>&lt;span style="color:#404040; font-family:Consolas; font-size:9pt">    location /socket.io/ {
&lt;/span></code></pre>
</p>

<p style="background: #92d050">
  <pre><code>&lt;span style="color:#404040; font-size:9pt">&lt;span style="font-family:Consolas">        proxy_pass       http://localhost:5000/socket.io/;  # &lt;/span>&lt;span style="font-family:宋体">如果&lt;/span>&lt;span style="font-family:Consolas">coco&lt;/span>&lt;span style="font-family:宋体">安装在别的服务器，请填写它的&lt;/span>&lt;span style="font-family:Consolas">ip
&lt;/span>&lt;/span></code></pre>
</p>

<p style="background: #92d050">
  <pre><code>&lt;span style="color:#404040; font-family:Consolas; font-size:9pt">        proxy_buffering off;
&lt;/span></code></pre>
</p>

<p style="background: #92d050">
  <pre><code>&lt;span style="color:#404040; font-family:Consolas; font-size:9pt">        proxy_http_version 1.1;
&lt;/span></code></pre>
</p>

<p style="background: #92d050">
  <pre><code>&lt;span style="color:#404040; font-family:Consolas; font-size:9pt">        proxy_set_header Upgrade $http_upgrade;
&lt;/span></code></pre>
</p>

<p style="background: #92d050">
  <pre><code>&lt;span style="color:#404040; font-family:Consolas; font-size:9pt">        proxy_set_header Connection "upgrade";
&lt;/span></code></pre>
</p>

<p style="background: #92d050">
  <pre><code>&lt;span style="color:#404040; font-family:Consolas; font-size:9pt">        proxy_set_header X-Real-IP $remote_addr;
&lt;/span></code></pre>
</p>

<p style="background: #92d050">
  <pre><code>&lt;span style="color:#404040; font-family:Consolas; font-size:9pt">        proxy_set_header Host $host;
&lt;/span></code></pre>
</p>

<p style="background: #92d050">
  <pre><code>&lt;span style="color:#404040; font-family:Consolas; font-size:9pt">        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
&lt;/span></code></pre>
</p>

<p style="background: #92d050">
  <pre><code>&lt;span style="color:#404040; font-family:Consolas; font-size:9pt">        access_log off;
&lt;/span></code></pre>
</p>

<p style="background: #92d050">
  <pre><code>&lt;span style="color:#404040; font-family:Consolas; font-size:9pt">    }
&lt;/span></code></pre>
</p>

<p style="background: #92d050">
   
</p>

<p style="background: #92d050">
  <pre><code>&lt;span style="color:#404040; font-family:Consolas; font-size:9pt">    location /coco/ {
&lt;/span></code></pre>
</p>

<p style="background: #92d050">
  <pre><code>&lt;span style="color:#404040; font-size:9pt">&lt;span style="font-family:Consolas">        proxy_pass       http://localhost:5000/coco/;  # &lt;/span>&lt;span style="font-family:宋体">如果&lt;/span>&lt;span style="font-family:Consolas">coco&lt;/span>&lt;span style="font-family:宋体">安装在别的服务器，请填写它的&lt;/span>&lt;span style="font-family:Consolas">ip
&lt;/span>&lt;/span></code></pre>
</p>

<p style="background: #92d050">
  <pre><code>&lt;span style="color:#404040; font-family:Consolas; font-size:9pt">        proxy_set_header X-Real-IP $remote_addr;
&lt;/span></code></pre>
</p>

<p style="background: #92d050">
  <pre><code>&lt;span style="color:#404040; font-family:Consolas; font-size:9pt">        proxy_set_header Host $host;
&lt;/span></code></pre>
</p>

<p style="background: #92d050">
  <pre><code>&lt;span style="color:#404040; font-family:Consolas; font-size:9pt">        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
&lt;/span></code></pre>
</p>

<p style="background: #92d050">
  <pre><code>&lt;span style="color:#404040; font-family:Consolas; font-size:9pt">        access_log off;
&lt;/span></code></pre>
</p>

<p style="background: #92d050">
  <pre><code>&lt;span style="color:#404040; font-family:Consolas; font-size:9pt">    }
&lt;/span></code></pre>
</p>

<p style="background: #92d050">
   
</p>

<p style="background: #92d050">
  <pre><code>&lt;span style="color:#404040; font-family:Consolas; font-size:9pt">    location /guacamole/ {
&lt;/span></code></pre>
</p>

<p style="background: #92d050">
  <pre><code>&lt;span style="color:#404040; font-size:9pt">&lt;span style="font-family:Consolas">        proxy_pass       http://localhost:8081/;  # &lt;/span>&lt;span style="font-family:宋体">如果&lt;/span>&lt;span style="font-family:Consolas">guacamole&lt;/span>&lt;span style="font-family:宋体">安装在别的服务器，请填写它的&lt;/span>&lt;span style="font-family:Consolas">ip
&lt;/span>&lt;/span></code></pre>
</p>

<p style="background: #92d050">
  <pre><code>&lt;span style="color:#404040; font-family:Consolas; font-size:9pt">        proxy_buffering off;
&lt;/span></code></pre>
</p>

<p style="background: #92d050">
  <pre><code>&lt;span style="color:#404040; font-family:Consolas; font-size:9pt">        proxy_http_version 1.1;
&lt;/span></code></pre>
</p>

<p style="background: #92d050">
  <pre><code>&lt;span style="color:#404040; font-family:Consolas; font-size:9pt">        proxy_set_header Upgrade $http_upgrade;
&lt;/span></code></pre>
</p>

<p style="background: #92d050">
  <pre><code>&lt;span style="color:#404040; font-family:Consolas; font-size:9pt">        proxy_set_header Connection $http_connection;
&lt;/span></code></pre>
</p>

<p style="background: #92d050">
  <pre><code>&lt;span style="color:#404040; font-family:Consolas; font-size:9pt">        proxy_set_header X-Real-IP $remote_addr;
&lt;/span></code></pre>
</p>

<p style="background: #92d050">
  <pre><code>&lt;span style="color:#404040; font-family:Consolas; font-size:9pt">        proxy_set_header Host $host;
&lt;/span></code></pre>
</p>

<p style="background: #92d050">
  <pre><code>&lt;span style="color:#404040; font-family:Consolas; font-size:9pt">        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
&lt;/span></code></pre>
</p>

<p style="background: #92d050">
  <pre><code>&lt;span style="color:#404040; font-family:Consolas; font-size:9pt">        access_log off;
&lt;/span></code></pre>
</p>

<p style="background: #92d050">
  <pre><code>&lt;span style="color:#404040; font-family:Consolas; font-size:9pt">    }
&lt;/span></code></pre>
</p>

<p style="background: #92d050">
   
</p>

<p style="background: #92d050">
  <pre><code>&lt;span style="color:#404040; font-family:Consolas; font-size:9pt">    location / {
&lt;/span></code></pre>
</p>

<p style="background: #92d050">
  <pre><code>&lt;span style="color:#404040; font-size:9pt">&lt;span style="font-family:Consolas">        proxy_pass http://localhost:8080;  # &lt;/span>&lt;span style="font-family:宋体">如果&lt;/span>&lt;span style="font-family:Consolas">jumpserver&lt;/span>&lt;span style="font-family:宋体">安装在别的服务器，请填写它的&lt;/span>&lt;span style="font-family:Consolas">ip
&lt;/span>&lt;/span></code></pre>
</p>

<p style="background: #92d050">
  <pre><code>&lt;span style="color:#404040; font-family:Consolas; font-size:9pt">        proxy_set_header X-Real-IP $remote_addr;
&lt;/span></code></pre>
</p>

<p style="background: #92d050">
  <pre><code>&lt;span style="color:#404040; font-family:Consolas; font-size:9pt">        proxy_set_header Host $host;
&lt;/span></code></pre>
</p>

<p style="background: #92d050">
  <pre><code>&lt;span style="color:#404040; font-family:Consolas; font-size:9pt">        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
&lt;/span></code></pre>
</p>

<p style="background: #92d050">
  <pre><code>&lt;span style="color:#404040; font-family:Consolas; font-size:9pt">    }
&lt;/span></code></pre>
</p>

<p style="background: #92d050">
  <pre><code>&lt;span style="color:#404040; font-family:Consolas; font-size:9pt">}
&lt;/span></code></pre>
</p>

    <span style="color:#404040; font-size:9pt"><span style="font-family:宋体">注意域名的更改</span><span style="font-family:Consolas">
    							</span></span>

运行nginx 

 

<p style="background: #fcfcfc">
  <span style="color:#404040; font-size:12pt"><span style="font-family:宋体"><strong>测试连接</strong></span><span style="font-family:Arial"><br /> </span></span>
</p>

<p style="background: #eeffcc">
  <pre><code>&lt;span style="color:#404040; font-size:9pt">&lt;span style="font-family:宋体">如果登录客户端是&lt;/span>&lt;span style="font-family:Consolas"> macOS &lt;/span>&lt;span style="font-family:宋体">或&lt;/span>&lt;span style="font-family:Consolas"> Linux &lt;/span>&lt;span style="font-family:宋体">，登录语法如下&lt;/span>&lt;span style="font-family:Consolas">
							&lt;/span>&lt;/span></code></pre>
</p>

<p style="background: #eeffcc">
  <pre><code>&lt;span style="color:#404040; font-family:Consolas; font-size:9pt">$ ssh -p2222 admin@192.168.244.144
&lt;/span></code></pre>
</p>

<p style="background: #eeffcc">
  <pre><code>&lt;span style="color:#404040; font-family:Consolas; font-size:9pt">$ sftp -P2222 admin@192.168.244.144
&lt;/span></code></pre>
</p>

<p style="background: #eeffcc">
  <pre><code>&lt;span style="color:#404040; font-size:9pt">&lt;span style="font-family:宋体">密码&lt;/span>&lt;span style="font-family:Consolas">: admin
&lt;/span>&lt;/span></code></pre>
</p>

<p style="background: #eeffcc">
   
</p>

<p style="background: #eeffcc">
  <pre><code>&lt;span style="color:#404040; font-size:9pt">&lt;span style="font-family:宋体">如果登录客户端是&lt;/span>&lt;span style="font-family:Consolas"> Windows &lt;/span>&lt;span style="font-family:宋体">，&lt;/span>&lt;span style="font-family:Consolas">Xshell Terminal &lt;/span>&lt;span style="font-family:宋体">登录语法如下&lt;/span>&lt;span style="font-family:Consolas">
							&lt;/span>&lt;/span></code></pre>
</p>

<p style="background: #eeffcc">
  <pre><code>&lt;span style="color:#404040; font-family:Consolas; font-size:9pt">$ ssh admin@192.168.244.144 2222
&lt;/span></code></pre>
</p>

<p style="background: #eeffcc">
  <pre><code>&lt;span style="color:#404040; font-family:Consolas; font-size:9pt">$ sftp admin@192.168.244.144 2222
&lt;/span></code></pre>
</p>

<p style="background: #eeffcc">
  <pre><code>&lt;span style="color:#404040; font-size:9pt">&lt;span style="font-family:宋体">密码&lt;/span>&lt;span style="font-family:Consolas">: admin
&lt;/span>&lt;/span></code></pre>
</p>

<p style="background: #eeffcc">
  <pre><code>&lt;span style="color:#404040; font-size:9pt">&lt;span style="font-family:宋体">如果能登陆代表部署成功&lt;/span>&lt;span style="font-family:Consolas">
							&lt;/span>&lt;/span></code></pre>
</p>

<p style="background: #eeffcc">
   
</p>

<p style="background: #eeffcc">
  <pre><code>&lt;span style="color:#404040; font-size:9pt">&lt;span style="font-family:Consolas"># sftp&lt;/span>&lt;span style="font-family:宋体">默认上传的位置在资产的&lt;/span>&lt;span style="font-family:Consolas"> /tmp &lt;/span>&lt;span style="font-family:宋体">目录下&lt;/span>&lt;span style="font-family:Consolas">
							&lt;/span>&lt;/span></code></pre>
</p>

<p style="background: #eeffcc">
  <pre><code>&lt;span style="color:#404040; font-size:9pt">&lt;span style="font-family:Consolas"># windows&lt;/span>&lt;span style="font-family:宋体">拖拽上传的位置在资产的&lt;/span>&lt;span style="font-family:Consolas"> Guacamole RDP&lt;/span>&lt;span style="font-family:宋体">上的&lt;/span>&lt;span style="font-family:Consolas"> G &lt;/span>&lt;span style="font-family:宋体">目录下&lt;/span>&lt;/span></code></pre>
</p>