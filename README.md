# proxy
linux 代理软件

go的执行文件已经编译好

1. 购买云服务器  Ubuntu系统 1CPU 2G内存就好，带宽按使用流量付费，带宽上限拉满

2. 安装 supervior 和 git

 apt update
 apt instll supervior git
 
3.  git clone https://github.com/quanjunqi/proxy.git  拉取代码

4. 添加supervior进程配置文件index.conf，index.conf只是随便取的，具体命名规则看supervisord.conf的 [include] 规则。
    [include]
    files = /etc/supervisor/conf.d/*.conf

index.conf:
   [program:go-shadowsocks2] ; 进程名字
   command=./root/go-shadowsocks2 -s ss://aes-128-gcm:xxxx@0.0.0.0:3389 -verbose  ;代理启动命令 xxxx为密码
   user=root ; 启动进程的用户
   autostart=true                ; 随着supervisord的启动而启动
   autorestart=true              ; 自动重启
   startsecs=4                   ; 启动10秒后没有异常退出，就表示进程正常启动了，默认为1秒
   stderr_logfile=/var/log/go-shadowsocks2.err.log.  ;错误日志
   stdout_logfile=/var/log/go-shadowsocks2.out.log。 ;info 日志


5. 把index.conf 拷贝到supervior的配置目录下

  启动supervior
  supervisord -c /etc/supervisor/supervisord.conf   或  supervisord 

  重新加载配置文件
  supervisorctl reload
  
  查看进程状态
  supervisorctl status
  
    test                          STOPPED   Mar 13 09:31 AM
    go-shadowsocks2               RUNNING   pid 4933, uptime 35 days, 5:49:04
  
6. 修改云主机安全组开放3389端口




