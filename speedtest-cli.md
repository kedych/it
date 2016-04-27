# SpeedTest CLI

習慣在Server上把GUI都砍光，但偶而有要使用speedtest的需求，找了一下SpeedTest是否有命令列的版本，結果是有的。

CentOS 命令列版本speedtest安裝過程如下：

##更新epel-release
有兩種更新方式，看要使用 yum　還是取得 rpm回系統安裝。

	yum install epel-release

For RHEL 6.x and CentOS 6.x (x86_64)

    rpm -ivh http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm

For RHEL 6.x and CentOS 6.x (i386)

    rpm -ivh http://dl.fedoraproject.org/pub/epel/6/i386/epel-release-6-8.noarch.rpm
    
##安裝pip

上一步驟更新EPEL之後，就可以使用yum安裝PIP

    sudo yum install -y python-pip
    sudo pip install --upgrade pip

##透過pip安裝speedtest

    sudo pip install speedtest-cli 


這樣就裝好了，可以使用文字命令的Speedtest測速

##執行speedtest
在terminal輸入命令speedtest-cli即可，不帶參數、自動選擇測試伺服器，執行結果如下：

    [kedy@elkapp01 ~]$speedtest-cli
	
    Retrieving speedtest.net configuration...
	Retrieving speedtest.net server list...
	Testing from HiNet (60.251.132.249)...
	Selecting best server based on latency...
	Hosted by Asia Pacific Telecom (Taipei) [1.06 km]: 3.613 ms
	Testing download speed........................................
	Download: 286.99 Mbit/s
	Testing upload speed..................................................
	Upload: 93.55 Mbit/s
