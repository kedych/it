# Let's Encrypt

#安裝Let's encrypt client

首先確定域名是可以被連接的，不然沒辦法產生憑證。

確定系統套件都更新
    
    sudo yum update -y

確定git有安裝
    
    sudo yum install git -y 

使用git下載let's encrypt client
    
    sudo git clone https://github.com/letsencrypt/letsencrypt /opt/letsencrypt

接著進入目錄
    
    cd /opt/letsencrypt

因為我們要手動設定，取得let encrypt憑證的時候，選擇standalone模式就可以。

    sudo ./letsencrypt-auto certonly --standalone --email kedy@niu.edu.tw -d [your domain here]

成功就會看到以下訊息, 並有提示存放路徑與有效日期
IMPORTANT NOTES:
 - Congratulations! Your certificate and chain have been saved at
   /etc/letsencrypt/live/[your domain here]/fullchain.pem. Your
   cert will expire on 2016-03-30. To obtain a new version of the
   certificate in the future, simply run Let's Encrypt again.
 - If you like Let's Encrypt, please consider supporting our work by:

   Donating to ISRG / Let's Encrypt:   https://letsencrypt.org/donate
   Donating to EFF:                    https://eff.org/donate-le


每三個月就要更新，可以透過crontab排程

    sudo ./letsencrypt-auto certonly --standalone --email kedy@niu.edu.tw -d [your domain here]
就可以定期更新憑證

#設定Apache SSL憑證組態
vim /etc/httpd/conf.d/ssl.conf
SSLCertificateFile /etc/letsencrypt/live/[your domain here]/cert.pem
SSLCertificateKeyFile /etc/letsencrypt/live/[your domain here]/privkey.pem
SSLCertificateChainFile /etc/letsencrypt/live/[your domain here]/chain.pem
SSLCACertificateFile /etc/letsencrypt/live/[your domain here]/fullchain.pem

#強制走443
先確定rewrite_module有載入
grep mod_rewrite.so /etc/httpd/conf.modules.d/00-base.conf
要看到
LoadModule rewrite_module modules/mod_rewrite.so

接著編輯httpd.conf，加入IfModule有關rewrite_module的組態
vim /etc/httpd/conf/httpd.conf

#Enforce https connection
<IfModule rewrite_module>
        RewriteEngine On
        RewriteCond %{HTTPS} off
        RewriteRule (.*) https://%{HTTP_HOST}%{REQUEST_URI}
</IfModule>

#重開httpd服務
systemctl restart httpd.service

開個瀏覽器看看吧! 搞定:)


##參考來源
https://letsencrypt.readthedocs.org/en/latest/

https://www.sslshopper.com/apache-redirect-http-to-https.html