## firewalld
```sh
sudo firewall-cmd --list-all-zones
```
* 只有active模式表示有在運作
* 確認要連線的ip屬於哪個zone，主要看interface
* 就算有開啟port，如果沒有在監聽，telnet一樣是回覆Connection refused

## firewalld開啟port
```sh
sudo firewall-cmd --zone=public --add-port=8888/tcp
# sudo firewall-cmd --zone=public --remove-port=8888/tcp # 移除
```

## 確定port是否可連線
`netstat -tlnp`
```
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name    
tcp        0      0 127.0.0.1:9000          0.0.0.0:*               LISTEN      -                   
tcp        0      0 127.0.0.1:6379          0.0.0.0:*               LISTEN      -                   
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      -                   
tcp        0      0 127.0.0.1:25            0.0.0.0:*               LISTEN      -                   
tcp6       0      0 :::3306                 :::*                    LISTEN      -  
```
> Local Address – The IP address of the local computer and the port number being used. The name of the local computer that corresponds to the IP address and the name of the port is shown unless the -n parameter is specified. An asterisk (*) is shown for the host if the server is listening on all interfaces. If the port is not yet established, the port number is shown as an asterisk.


> Foreign Address – The IP address and port number of the remote computer to which the socket is connected. The names that corresponds to the IP address and the port are shown unless the -n parameter is specified. If the port is not yet established, the port number is shown as an asterisk (*).


看`Local Address` __127.0.0.1__ 表示只有本機可以連，__0.0.0.0__ 的才表示外部可以連  
至於`Foreign Address`的部分，
1. "0.0.0.0" means, that a server is listening on every available interface with the corresponding IP
2. If the port is not yet established, the port number is shown as an asterisk.

在能telnet接通port之後，Foreign Address依然是`0.0.0.0:*`  
目前的理解是該service可能是連接外部的某個端口並在Local Address產生接口  
算是中間的介接端口，實際上看Local Address即可



### 開啟port監聽
可以安裝 nc
```bash
sudo yum install nc
```
```bash
nc -lp 8888
```


### 測試
```sh
telnet [192.168.168.100] [8888]
```

參考:  
https://en.wikipedia.org/wiki/Netstat  
https://superuser.com/questions/1485342/0-0-0-0-foreign-adress-listening-at-loopback
