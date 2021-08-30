1  sudo apt update
2  sudo apt install shadowsocks-libev
3  sudo vim /etc/shadowsocks-libev/config.json
4  service shadowsocks-libev start
5  ps -ef | grep shadow
6  vi /etc/shadowsocks-libev
7  vi /etc/shadowsocks-libev/config.json 
8  kill 3910
9  kill 3901 
10  service shadowsocks-libev start
   11  sudo apt-get install ufw
   12  sudo ufw status 
   13  sudo ufw allow 22
   14  ufw allow from 118.25.139.156 to any port 60010
   15  sudo ufw default deny
   16  sudo ufw active
   17  sudo ufw enable
   18  sudo ufw allow 80
   19  sudo ufw enable
   20  sudo apt-get install nginx
   21  nginx
   22  sudo apt-get install nginx
   23  nginx -t
   24  sudo /etc/init.d/nginx start
   25  vi /etc/shadowsocks-libev/config.json 
   26  history > init.txt