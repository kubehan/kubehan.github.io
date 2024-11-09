---
title: 查看进程的 inotify watch 情况
author: Kubehan
type: post
date: 2022-10-21T07:06:04+08:00
url: /3257.html
post_style:
  - sidebar
cao_vip_rate:
  - 1
views:
  - 811
categories:
  - Linux运维

---
<pre><code class="language-bash">#!/usr/bin/env bash
#
# Copyright 2019 (c) roc
#
# This script shows processes holding the inotify fd, alone with HOW MANY directories each inotify fd watches(0 will be ignored).
total=0
result="EXE PID FD-INFO INOTIFY-WATCHES\n"
while read pid fd; do \
  exe="$(readlink -f /proc/$pid/exe || echo n/a)"; \
  fdinfo="/proc/$pid/fdinfo/$fd" ; \
  count="$(grep -c inotify "$fdinfo" || true)"; \
  if [ $((count)) != 0 ]; then
    total=$((total+count)); \
    result+="$exe $pid $fdinfo $count\n"; \
  fi
done &lt;&lt;&lt; "$(lsof +c 0 -n -P -u root|awk &#039;/inotify$/ { gsub(/[urw]$/,"",$4); print $2" "$4 }&#039;)" && echo "total $total inotify watches" && result="$(echo -e $result|column -t)\n" && echo -e "$result" | head -1 && echo -e "$result" | sed "1d" | sort -k 4rn;
</code></pre>