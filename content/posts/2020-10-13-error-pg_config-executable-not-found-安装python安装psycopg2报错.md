---
title: 'Error: pg_config executable not found 安装python安装psycopg2报错'
author: Kubehan
type: post
date: 2020-10-13T03:48:52+08:00
url: /2948.html
post_style:
  - sidebar
cao_vip_rate:
  - 1
views:
  - 1351
bigger_cover:
  - https://www.kubehan.cn/wp-content/uploads/posterimg/poster-2948.png
categories:
  - Python

---
报错信息：

<pre><code class="language-bash">    ERROR: Command errored out with exit status 1:
     command: /bin/python -c &#039;import sys, setuptools, tokenize; sys.argv[0] = &#039;"&#039;"&#039;/tmp/pip-install-bPTUmV/psycopg2/setup.py&#039;"&#039;"&#039;; __file__=&#039;"&#039;"&#039;/tmp/pip-install-bPTUmV/psycopg2/setup.py&#039;"&#039;"&#039;;f=getattr(tokenize, &#039;"&#039;"&#039;open&#039;"&#039;"&#039;, open)(__file__);code=f.read().replace(&#039;"&#039;"&#039;\r\n&#039;"&#039;"&#039;, &#039;"&#039;"&#039;\n&#039;"&#039;"&#039;);f.close();exec(compile(code, __file__, &#039;"&#039;"&#039;exec&#039;"&#039;"&#039;))&#039; egg_info --egg-base /tmp/pip-pip-egg-info-htz3hz
         cwd: /tmp/pip-install-bPTUmV/psycopg2/
    Complete output (23 lines):
    running egg_info
    creating /tmp/pip-pip-egg-info-htz3hz/psycopg2.egg-info
    writing /tmp/pip-pip-egg-info-htz3hz/psycopg2.egg-info/PKG-INFO
    writing top-level names to /tmp/pip-pip-egg-info-htz3hz/psycopg2.egg-info/top_level.txt
    writing dependency_links to /tmp/pip-pip-egg-info-htz3hz/psycopg2.egg-info/dependency_links.txt
    writing manifest file &#039;/tmp/pip-pip-egg-info-htz3hz/psycopg2.egg-info/SOURCES.txt&#039;

    Error: pg_config executable not found.

    pg_config is required to build psycopg2 from source.  Please add the directory
    containing pg_config to the $PATH or specify the full executable path with the
    option:

        python setup.py build_ext --pg-config /path/to/pg_config build ...

    or with the pg_config option in &#039;setup.cfg&#039;.

    If you prefer to avoid building psycopg2 from source, please install the PyPI
    &#039;psycopg2-binary&#039; package instead.

    For further information please check the &#039;doc/src/install.rst&#039; file (also at
    &lt;https://www.psycopg.org/docs/install.html&gt;).

    ----------------------------------------
ERROR: Command errored out with exit status 1: python setup.py egg_info Check the logs for full command output.
</code></pre>

原因一：  
需要用到插件却没有安装 没有安装postgresql插件

解决办法：  
安装插件

<pre><code class="language-shell">yum install postgresql-devel*</code></pre>

原因二：  
插件版本冲突  
解决办法：卸载原来的版本再安装新版本

<pre><code class="language-python">pip uninstall  psycopg2
pip install  psycopg2</code></pre>

再次安装就能成功了