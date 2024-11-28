

[FRP](https://github.com/fatedier/frp)

setting up vhost to forward domain to local machine [Link](https://github.com/fatedier/frp#:~:text=own%20domain%20name.-,Unfortunately,-%2C%20we%20cannot%20resolve).

make sure have the same auth token in client and sever, file extensions are in .toml

FRP server config

```
bindPort = 7000
auth.method = "token"
auth.token = ""
vhostHTTPPort = 80
vhostHTTPSPort = 443
```


FRP client config

```
serverAddr = "orc.webcap.site"
serverPort = 7000
auth.method = "token"
auth.token = ""

[[proxies]]
name = "web"
type = "http"
localPort = 80
customDomains = ["*.home.webcap.site"]

[[proxies]]
name = "secure-web"
type = "https"
localPort = 443
customDomains = ["*.home.webcap.site"]
```

Following the elow guide to have the frps and frpc as a service in linux
```
# 1. put frpc and frpc.ini under /usr/local/frpc/
# 2. put this file (frpc.service) at /etc/systemd/system
# 3. run `sudo systemctl daemon-reload && sudo systemctl enable frpc && sudo systemctl start frpc`
# Then we can manage frpc with `sudo service frpc {start|stop|restart|status}`
# See also: https://nosame.net/use-frp-to-reverse-proxy-your-nas/

# Alternative for server:
# - Offical: https://github.com/fatedier/frp/blob/a4cfab6/conf/systemd/frpc%40.service

[Unit]
Description=frp client
Wants=network-online.target
After=network.target network-online.target

[Service]
ExecStart=/usr/local/frpc/frpc -c /usr/local/frpc/frpc.ini

[Install]
WantedBy=multi-user.target
```
