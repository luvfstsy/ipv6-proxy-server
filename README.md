## IPv6 Proxy Server [![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

Create your own IPv6 backconnect proxy server with only one script on any Linux distribution. Any number of random IPs on the subnet, ideal for parsing and traffic arbitrage (Google/Facebook/Youtube/Instagram and many others support IPv6).

Ask questions [in issues](https://github.com/Temporalitas/ipv6-proxy-server/issues), if you have some.

### Tutorial

Assuming you already have an entire IPv6 subnet (/48 or /64) routed to your server.

Just run:

```bash
#sudo su
wget https://raw.githubusercontent.com/Temporalitas/ipv6-proxy-server/master/ipv6-proxy-server.sh && chmod +x ipv6-proxy-server.sh
./ipv6-proxy-server.sh -s 64 -c 100 -u username -p password -t http -r 10
```

sudo如果您不是 root 用户，请取消注释第一行或运行所有命令。




如果脚本已经安装，您只需运行一个命令即可重新配置参数，例如：


```bash
./ipv6-proxy-server.sh -s 64 -c 20 -u username2 -p password2 -t socks5 -r 2
```

Old instance will be disabled and new starts without reinstallation very quickly.



If you want to uninstall proxy server, just run:

```bash
./ipv6-proxy-server.sh --uninstall
```

Proxy server will stopped, all configuration files, firewalls, shedulers and so on will be reset to initial state.



**Command line arguments:**

- `-s` or `--subnet` - IPv6 [subnet](https://docs.netgate.com/pfsense/en/latest/network/ipv6/subnets.html), fully allocated on your server. Any subnet divisible by 4 (for example, `48` or `56`), default `64`
- `-c` or `--proxy-count` - The total number of proxies you want to have (from 1 to 10000)
- `-t` or `--proxies-type` - Proxies type - `http` or `socks5`. Default `http`, if no value provided
- `-u` or `--username` - All proxies auth login
- `-p` or `--password` - All proxies auth password (if you specify neither username not password, proxy will run without authentication)
- `--rotate-every-request` - bool parameter without value, if used, for every request to each proxy you will get different gateway IP from your IPv6 subnet. Doesn't work on all VPS. If configuration isn't possible, it will exit with an error
- `--random` - bool parameter without value, if used, each backconnect proxy will have random username and password, that will be written in backconnect proxies file (`-f` argument)
- `--start-port` - backconnect IPv4 start port. If you create 1500 proxies and `start-port` is `20000`, and server external IPv4 is, e.g,`180.113.14.28` you can connect to proxies using `180.113.14.28:20000`, `180.113.14.28:20001` and so on until `180.113.14.28:21500`
- `-r` or `--rotating-interval` - rotation interval of entire proxy pool in minutes. At the end of each interval, output (external IPv6) addresses of all proxies are changed and proxy server is restarted, which breaks existing connections for a few seconds. From 0 to 59, default value - `0` (rotating disabled)
- `--allowed-hosts` - list of allowed hosts, to which user can connect via proxy (comma-separated, without spaces, for example - `"google.com,*.google.com,fb.com"`). All other hosts will be denied, if this parameter is provided
- `--denied-hosts` - list of denied hosts (comma-separated, without spaces, for example - `"google.com,*.google.com,fb.com"`). All others hosts will be allowed, if this parameter is provided
- `-l` or `--localhost` - bool parameter without value, if used, all backconnect proxy will be available only on localhost (`127.0.0.1:30000` instead of `180.113.14.28:30000`)
- `-b` or `--backconnect-ip` - server IPv4 backconnect address for proxies, use ONLY if script cannot parse IP correctly and your server has non-standard IP configuration
- `-f` or `--backconnect-proxies-file` - path to file, in which backconnect proxies list will be written when proxies start working (default `~/proxyserver/backconnect_proxies.list`). You can just copy all proxies from this file and use them in your soft as list of IPv6 proxies.
- `-m` or `--ipv6-mask` - first blocks on server subnet, unchanged part, use ONLY if script cannot parse ipv6 mask automatically. For example, if the external ipv6 address on server is `2a03:6f01:5::1da6` and you want to use entire /64 subnet, script cannot parse ipv6 gateway because of address zero-field replacement with `::`. Real mask for /64 subnet is first four blocks - `2a03:6f01:5:0`
- `-i` or `--interface` - ethernet interface name, to which IPv6 subnet is allocated and where all proxies will be raised. Automatically parsed from system info by default, use ONLY if you have non-standard/additional interfaces on your server.
- `--uninstall` - uninstall proxy server, you don't need to provide any other parameters with it.
- `--info` - get info about running proxy server (proxy count, rotating, auth, etc.)

命令行参数：

-s或--subnet- IPv6子网，在您的服务器上完全分配。任何能被 4 整除的子网（例如48或56），默认64
-c或--proxy-count- 您想要拥有的代理总数（从 1 到 10000）
-t或--proxies-type- 代理类型 -http或socks5.http如果没有提供值，则为默认值
-u或--username- 所有代理验证登录
-p或--password- 所有代理都验证密码（如果您既未指定用户名也未指定密码，则代理将在无需验证的情况下运行）
--rotate-every-request- bool 参数没有值，如果使用，对于每个代理的每个请求，您将从 IPv6 子网获得不同的网关 IP。不适用于所有 VPS。如果无法配置，它将退出并显示错误
--random- 没有值的 bool 参数，如果使用，每个反向连接代理将具有随机用户名和密码，将写入反向连接代理文件（-f参数）中
--start-port- 反向连接 IPv4 起始端口。如果您创建 1500 个代理，并且start-port是20000，并且服务器外部 IPv4 是，例如，180.113.14.28您可以使用 连接到代理180.113.14.28:20000，180.113.14.28:20001依此类推，直到180.113.14.28:21500
-r或--rotating-interval- 整个代理池的轮换间隔（分钟）。在每个间隔结束时，所有代理的输出（外部 IPv6）地址都会更改，并且代理服务器会重新启动，这会中断现有连接几秒钟。从 0 到 59，默认值 - 0（轮换已禁用）
--allowed-hosts- 允许通过代理连接的主机列表（以逗号分隔，不带空格，例如 - "google.com,*.google.com,fb.com"）。如果提供了此参数，则将拒绝所有其他主机
--denied-hosts- 拒绝的主机列表（以逗号分隔，不带空格，例如 - "google.com,*.google.com,fb.com"）。如果提供了此参数，则将允许所有其他主机
-l或--localhost- 没有值的 bool 参数，如果使用，所有反向连接代理将仅在 localhost 上可用（127.0.0.1:30000而不是180.113.14.28:30000）
-b或--backconnect-ip- 代理的服务器 IPv4 反向连接地址，仅当脚本无法正确解析 IP 且服务器具有非标准 IP 配置时才使用
-f或--backconnect-proxies-file- 文件路径，当代理开始工作时，反向连接代理列表将写入其中（默认~/proxyserver/backconnect_proxies.list）。您可以从此文件复制所有代理，并在您的软件中将它们用作 IPv6 代理列表。
-m或--ipv6-mask- 服务器子网上的第一个块，未更改的部分，仅当脚本无法自动解析 ipv6 掩码时使用。例如，如果服务器上的外部 ipv6 地址是，2a03:6f01:5::1da6而您想使用整个 /64 子网，则脚本无法解析 ipv6 网关，因为地址零字段被替换为::。/64 子网的实际掩码是前四个块 -2a03:6f01:5:0
-i或--interface- 以太网接口名称，IPv6 子网分配到该接口，所有代理都将在此启动。默认情况下，自动从系统信息解析，仅当您的服务器上有非标准/附加接口时才使用。
--uninstall- 卸载代理服务器，您不需要提供任何其他参数。
--info- 获取有关正在运行的代理服务器的信息（代理计数、旋转、身份验证等）

**Quick errors FAQ:**

- If your proxies simply don't work, do these few steps and write an issue **only** if none of this worked:
  - Check your logfile: `/var/tmp/ipv6-proxy-server-logs.log` for any error-like messages. If message is related to configuration/subnet allocation, ask your VPS provider. If error message is unclear, write an issue.
  - If there is no errors, run `./ipv6-proxy-server.sh --info`.
  - Check that backconnect addresses parsed correctly (the file name on your server in first step).
  - Check that outgoing IPv6 addresses generated correctly (in `proxyserver/3proxy/3proxy.cfg` file).
  - Check connection from proxy server: select one of outgoing IPv6 addresses from previous step and run command `curl --interface <ipv6-address> <website>`. If it works correctly, but proxy doesn't work - write an issue right now.
  - Check if the site you want to access has an AAAA record in DNS. If no, you cannot reach this site via any IPv6 proxies.
  - Verify that your VPS provider allocated a full IPv6 subnet to the server (DigitalOcean and many other providers don't do this).

### License

[MIT](https://opensource.org/licenses/MIT)
