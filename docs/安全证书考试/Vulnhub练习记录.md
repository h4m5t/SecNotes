# Vulnhub练习记录
### kali配置

修改主机名：

```
chmod 644 /etc/hostname
vi /etc/hostname
修改内容为h4m5t
reboot
```

sudo su 很慢，进行优化：

```
echo '127.0.0.1 h4m5t.localdomain h4m5t' >> /etc/hosts
```

安装其他工具：

```
git clone https://hub.nuaa.cf/maurosoria/dirsearch.git --depth 1
```



### report

#### 配置报告环境

参考：https://github.com/noraj/OSCP-Exam-Report-Template-Markdown

https://www.youtube.com/watch?v=MQGozZzHUwQ

Ubuntu安装(可能需要换清华源)

```
apt install texlive-latex-recommended    
apt install texlive-fonts-extra
apt install texlive-latex-extra
apt install pandoc
apt install p7zip-full
```

下载eisvogel.latex

```
从https://github.com/Wandmalfarbe/pandoc-latex-template/releases下载

复制到模板目录下
cp '/home/h4m5t/Downloads/Eisvogel/eisvogel.latex' /usr/share/pandoc/data/templates/eisvogel.latex
```

下载生成脚本：

```
git clone https://github.com/noraj/OSCP-Exam-Report-Template-Markdown
```

#### 手动生成报告

手动生成报告：指定模板和输出文件名。

```
pandoc src/OSCP-exam-report-template_whoisflynn_v3.2.md \
-o output/OSCP-OS-h4m5t-Exam-Report.pdf \
--from markdown+yaml_metadata_block+raw_html \
--template eisvogel \
--table-of-contents \
--toc-depth 6 \
--number-sections \
--top-level-division=chapter \
--highlight-style breezedark \
--resource-path=.:src
```

参数highlight-style 可以修改语法高亮。参考：https://pandoc.org/MANUAL.html#option--highlight-style

查看支持的高亮主题：`pandoc --list-highlight-styles`

Specifies the coloring style to be used in highlighted source code. Options are `pygments` (the default), `kate`, `monochrome`, `breezeDark`, `espresso`, `zenburn`, `haddock`, and `tango`. 

#### 自动生成报告

安装ruby

```
sudo apt install ruby-full
```

Choose and copy a template that you will use to write your report:

```
ruby osert.rb init
```

Generate your PDF report and 7z archive

```
ruby osert.rb generate
```

#### bash脚本输出报告(推荐)

写脚本generate.sh

chmod +x generate.sh

```bash
#!/bin/bash

if [[ $# -ne 2 ]]; then
  echo "请提供两个参数：模板文件<input.md>和输出文件<output.pdf>"
  exit 1
fi

template_file=$1
output_file=$2

pandoc "$template_file" \
-o "$output_file" \
--from markdown+yaml_metadata_block+raw_html \
--template eisvogel \
--table-of-contents \
--toc-depth 6 \
--number-sections \
--top-level-division=chapter \
--highlight-style tango \
--resource-path=.:src
```





### 靶场

靶场文件：

\\file.tp-link.com.cn\网络管理课\5.信息安全运维与管理\98.学习资料\靶场

### 已完成：

DC-1







### 待完成：

Kioptrix

* 1.0
* 1.1
* 1.2
* 1.3



METASPLOITABLE1

METASPLOITABLE12

VulnOS2



待下载

FristiLeaks 1.3

Mr-Robot: 1

HackLAB

Hack Me

Hack Me Please

Digitalworld

