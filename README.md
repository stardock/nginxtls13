# nginxtls13
Enable TLS1.3 and CHACHA20 support for Nginx


懒人版：  
宝塔快速安装nginx1.17，然后service nginx stop
将 /www/server/nginx/sbin 下的nginx文件改个名字
把本库中的 nginx 贴过去即可.



铁人版：  

下载 Nginx 的源码  

cd 
wget http://nginx.org/download/nginx-1.17.2.tar.gz  
tar zxf nginx-1.17.2.tar.gz  
mv nginx-1.17.2 nginx  


给 Nginx 打补丁  

补丁来自：https://github.com/kn007/patch  

nginx 补丁  
 
    添加SPDY支持。  
    添加HTTP2 HPACK编码支持。  
    添加动态TLS记录支持。  
    
```  
cd
git clone https://github.com/stardock/patch nginx-patch
cd /nginx
patch -p1 < ../nginx-patch/nginx.patch
```  



Reference:  
给Nginx打CHACHA20补丁: https://www.v2ex.com/t/546906  
