rm -r ~/.PyCharm2018.3  删除PyCharm初始设置

打包 ：python3 setup.py bdist_egg

启动 ：scrapy crawl zhongguojingjiwang

ln -s  /data1/python3  /usr/local/opt





export LDFLAGS=”-L/usr/local/ssl/lib” 
export CPPFLAGS=”-I/usr/local/ssl/include” 
export PKG_CONFIG_PATH=”/usr/local/ssl/lib/pkgconfig”



ubantu 用aptget 而不是yum



记录步骤

/usr/bin 会优先于 /bin 下面的命令



1. /data1/hujia/tool/glibc-2.25/build yum -y install gcc
2. 原本 libc.so.6 -> libc-2.17.so
3. LD_PRELOAD=/lib64/libc-2.25.so ln -s /lib64/libc-2.25.so /lib64/libc.so.6
4. rm /lib64/libc.so.6
5. ln -s /lib64/libc-2.25.so /lib64/libc.so.6



LD_PRELOAD=/lib64/libc-2.17.so

LD_PRELOAD=/lib64/libc-2.17.so ln -s /lib64/libc-2.17.so /lib64/libc.so.6





原始：

librt.so.1 -> librt-2.17.so

 libpthread.so.0 -> libpthread-2.17.so

libm.so.6 -> libm-2.17.so

libc.so.6 -> /lib64/libc-2.17.so

执行：

LD_PRELOAD=/lib64/libc-2.25.so ln -sf libc-2.25.so libc.so.6







```
curl http://localhost:6800/listspiders.json?project=myproject
```



```
curl http://localhost:6800/schedule.json -d project=myproject -d spider=somespider
```



/usr/local/Cellar/python/3.7.2_2/Frameworks/Python.framework/Versions/3.7/lib/python3.7

/usr/local/Cellar/python/3.7.2_2/Frameworks/Python.framework/Versions/3.7/lib/python3.7/site-packages/



![image-20190423190828028](/note/zlinks/pic/image-20190423190828028.png)





```
self.jobs = [dict(zip(JOB_KEYS, job)) for job in re.findall(JOB_PATTERN, self.text)]
```

 github master分支上的scrapyd 超前pip3 install的版本 现回退到1.2.0

pip install GitPython==2.1.9





To have launchd start zookeeper now and restart at login:
  brew services start zookeeper
Or, if you don't want/need a background service you can just run:
  zkServer start
==> Summary
🍺  /usr/local/Cellar/zookeeper/3.4.13

