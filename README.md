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
nginx补丁  
  添加TLS1.3加密算法选择以及开启CHACHA20支持

cd
git clone https://github.com/stardock/nginx-tls13-chacha20-patch
cd nginx
patch -p1 < ../nginx_tls13_chacha20_1_17_2.patch 	


编译安装

cd /usr/src/nginx
./configure \
--user=www-data --group=www-data \
--prefix=/usr/local/nginx \
--sbin-path=/usr/sbin/nginx \
--with-compat --with-file-aio --with-threads \
--with-http_v2_module --with-http_v2_hpack_enc \
--with-http_spdy_module --with-http_realip_module \
--with-http_flv_module --with-http_mp4_module \
--with-openssl=../openssl --with-http_ssl_module \
--with-pcre=../pcre-8.42 --with-pcre-jit \
--with-zlib=../zlib --with-http_gzip_static_module \
--add-module=../ngx_brotli \
--with-ld-opt=-ljemalloc

make 


Reference:  
给Nginx打CHACHA20补丁: https://www.v2ex.com/t/546906  
