---
layout: post
category: elasticsearch
title: elasticsearch工作随记
tagline: by CJ
tags: [elasticsearch]
---


## 0.两台机器

* 做elasticsearch集群使用    
ip1:192.168.1.126  
ip2:192.168.1.135
  
## 1.yum安装elasticsearch
* 下载安装公共密匙  

        [root@localhost ~]#rpm --import http://packages.elasticsearch.org/GPG-KEY-elasticsearch
* 配置/etc/yum.repos.d/elasticsearch.repo  

        [elasticsearch-1.2]
        name=Elasticsearch repository for 1.2.x packages
        baseurl=http://packages.elasticsearch.orgS/elasticsearch/1.2/centos
        gpgcheck=1
        gpgkey=http://packages.elasticsearch.org/GPG-KEY-elasticsearch
        enabled=1
 
 <!--more-->
 
* Install  

        [root@localhost ~]# yum install elasticsearch

* 配置/etc/elasticsearch/elasticsearch.yml  
初期主要设置cluster.name，node.name。去掉#号，设置成自己想要的值。

* 启动elasticsearch服务  

        [root@localhost ~]# service elasticsearch start  
        which: no java in (/sbin:/usr/sbin:/bin:/usr/bin)  
        Could not find any executable java binary. Please install java in your PATH or set JAVA_HOME

    如果遇到此问题，请查看jdk版本，可能已被变成服务器安装时自带的低版本jdk  
    解决方法如下（如本身服务器已经配置妥当，请略过）：

        [root@localhost ~]# cd /usr/bin/
        [root@localhost bin]# ln -s -f /usr/local/jdk1.7.0_71/bin/javac
        [root@localhost bin]# ln -s -f /usr/local/jdk1.7.0_71/jre/bin/java
        [root@localhost bin]# service elasticsearch start  
        Starting elasticsearch:                                    [  OK  ]

* 开启防火墙端口  
    /etc/sysconfig/iptables中添加9200端口（elasticsearch默认端口）：
    
        -A INPUT -p tcp -m tcp --dport 9200 -j ACCEPT  

    浏览器访问ip:9200  
    ip:9200中"status" : 200则表示OK

    安装另外一台服务器，两台安装完成后需要给两台服务器配置elasticsearch互访  
    官网说port是范围值9300-9400，所以这边配置互访ip不指定端口  
    192.168.1.126：   

        -A INPUT -s 192.168.1.135 -j ACCEPT  
        
    192.168.1.135：   

        -A INPUT -s 192.168.1.126 -j ACCEPT


### 2.elasticsearch-head安装
    [root@test ~]# cd /usr/share/elasticsearch/bin/
    [root@test bin]# ./plugin -install mobz/elasticsearch-head
    -> Installing mobz/elasticsearch-head...
    Trying https://github.com/mobz/elasticsearch-head/archive/master.zip...
    Downloading ..................................DONE
    Installed mobz/elasticsearch-head into /usr/share/elasticsearch/plugins/head
    Identified as a _site plugin, moving to _site structure ...

安装完成之后,在浏览器输入:http://ip:9200/_plugin/head/ ,可以查看显示效果

### 3.bigdesk安装
    [root@test bin]# ./plugin -install lukas-vlcek/bigdesk
    Trying https://github.com/lukas-vlcek/bigdesk/archive/master.zip...
    Downloading .............................................DONE
    Installed lukas-vlcek/bigdesk into /usr/share/elasticsearch/plugins/bigdesk
    Identified as a _site plugin, moving to _site structure ...

安装完成之后,在浏览器输入:http://ip:9200/_plugin/bigdesk/ ,可以查看显示效果

### 4.cluster.name相同，nodename不同，elasticsearch会自动集群。  
http://192.168.1.126:9200/_plugin/bigdesk 这样就能看到两个node

### 5.安装IK中文分词插件到服务器(elasticsearch自带的分词会把中文分成一个个字而不是词组)
* 获取分词的依赖包  
 
        [root@localhost caojian]# git clone https://github.com/medcl/elasticsearch-analysis-ik
* 打包生成elasticsearch-analysis-ik.jar  

	    [root@localhost caojian]# wget http://gitsea.qiniudn.com/elasticsearch-analysis-ik-1.2.6.jar
    或者把上面下载的源码包打成jar包  

	    [root@localhost caojian]# jar cvf elasticsearch-analysis-ik.jar elasticsearch-analysis-ik
* 切换到/usr/share/elasticsearch/plugins 目录  

        [root@localhost plugins]# mkdir analysis-ik
    把jar包放到analysis-ik目录下  
    
* 切换到 /etc/elasticsearch/ 目录  
把elasticsearch-analysis-ik\config  下的ik文件夹拷贝到该处  

*  编辑 /etc/elasticsearch/elasticsearch.yml,在文件末尾加上  

		index:
			analysis:
				analyzer:
					ik:
						alias: [news_analyzer_ik,ik_analyzer]
						type: org.elasticsearch.index.analysis.IkAnalyzerProvider
		index.analysis.analyzer.default.type : "ik"  
		
* 重启elasticsearch服务  

        [root@localhost elasticsearch]# service elasticsearch restart
        
* 创建名为index的索引  

		[root@localhost elasticsearch]# curl -XPUT http://localhost:9200/index  
* 为索引index创建mapping

		[root@localhost elasticsearch]# curl -XPOST http://localhost:9200/index/fulltext/_mapping -d'
		{
			"fulltext": {
				"_all": {
				"analyzer": "ik"
				},
				"properties": {
					"content": {
						"type" : "string",
						"boost" : 8.0,
						"term_vector" : "with_positions_offsets",
						"analyzer" : "ik",
						"include_in_all" : true
					}
				}
			}
		}'
* 测试分词结果

    	[root@localhost elasticsearch]# curl 'http://localhost:9200/index/_analyze?analyzer=ik&pretty=true' -d '
	    {
	    	"content":"中国人民共和国"
    	}'

* 完成  
	看看是不是按照词组来分词了
	
### 6.elasticsearch的java api调用demo
下面是最近做的demo  
https://github.com/CharlesCJ/es

	
### 7.spring-data-elasticsearch的demo
下面是最近做的demo  
https://github.com/CharlesCJ/springes

### 8.后记  
个人工作随记，希望对大家有帮助

