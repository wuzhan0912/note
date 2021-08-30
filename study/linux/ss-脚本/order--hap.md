    1  ls
    2  yum -y install haproxy
    3  yum install gcc-c++
    4  yum install -y pcre pcre-devel
    5  yum install -y zlib zlib-devel
    6  yum install -y openssl openssl-devel
    7  wget -c https://nginx.org/download/nginx-1.10.1.tar.gz
    8  wget http://nginx.org/download/nginx-1.4.2.tar.gz
    9  tar -xvf nginx-1.4.2.tar.gz
   10   cd nginx-1.4.2/
   11  ./configure
   12  make && make install
   13  cd /usr/local/nginx/sbin/
   14  ./nginx
   15  chmod 755 /etc/rc.local
   16  vi /etc/rc.local 
   17  vi /etc/haproxy/haproxy.cfg 
   18  mv /etc/haproxy/haproxy.cfg /etc/haproxy/haproxy.cfg.bak
   19  cd
   20  pwd
   21  ls
   22  cp haproxy.cfg /etc/haproxy/
   23  vi /etc/haproxy/haproxy.cfg
   24  systemctl start haproxy.service
   25  systemctl enable haproxy.service
   26  ps -ef | grep hap
   27   systemctl stop haproxy.service
   28  ps -ef | grep hap
   29  vi /etc/haproxy/haproxy.cfg
   30  ping 103.85.24.239
   31  systemctl start haproxy.service
   32  service iptable status
   33  firewall-cmd --state
   34  systemctl status firewalld.service
   35  firewall-cmd --list-ports
   36  systemctl stop firewalld.service
   37  ls
   38  cd 
   39  ps -ef | grep hap
   40  ifconfig
   41  ping 103.85.24.239
   42  ls
   43  rm order.txt 
   44  history > order.txt
