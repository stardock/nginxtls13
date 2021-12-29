




准备工作 Openssl 1.1.1k  
```
cd /root/
wget https://github.com/openssl/openssl/archive/refs/tags/OpenSSL_1_1_1k.tar.gz
tar -xzf OpenSSL_1_1_1k.tar.gz
mv openssl* openssl
cd openssl
./configure
make -j5
```
无错误产生会在apps目录下生成openssl执行文件
```  
cd apps
./openssl version
```  
`OpenSSL 1.1.1k  25 Mar 2021`  

开始编译nginx v1.19

下载安装包
```  
cd /root
wget https://github.com/stardock/nginxtls13/raw/v1.19/src.tar.gz
tar -xzf src.tar.gz
cd src
```

打入补丁
```
curl https://raw.githubusercontent.com/kn007/patch/master/nginx.patch | patch -p1
curl https://raw.githubusercontent.com/kn007/patch/master/use_openssl_md5_sha1.patch | patch -p1
curl https://raw.githubusercontent.com/kn007/patch/master/Enable_BoringSSL_OCSP.patch | patch -p1
```

编译前的配置
```  
./configure --user=www --group=www --prefix=/www/server/nginx \
--add-module=./ngx_devel_kit --add-module=./lua_nginx_module \
--add-module=./ngx_cache_purge --add-module=./nginx-sticky-module \
--with-compat --with-file-aio --with-threads \
--with-openssl=../openssl --with-pcre=pcre-8.43 \
--with-http_v2_module --with-stream --with-stream_ssl_module \
--with-stream_ssl_preread_module --with-http_stub_status_module \
--with-http_ssl_module --with-http_image_filter_module --with-http_gzip_static_module \
--with-http_gunzip_module --with-ipv6 --with-http_sub_module --with-http_flv_module \
--with-http_addition_module --with-http_realip_module --with-http_mp4_module \
--with-ld-opt=-Wl,-E --with-cc-opt=-Wno-error --with-ld-opt=-ljemalloc \
--with-http_dav_module --add-module=./nginx-dav-ext-module
```  


执行编译
`make -j5`  

完成后会在objs目录找到编译好的nginx文件
```  
cd objs
./nginx -V
```  

```  
nginx version: nginx/1.19.8
built by gcc 4.8.5 20150623 (Red Hat 4.8.5-44) (GCC)
built with OpenSSL 1.1.1k  25 Mar 2021
TLS SNI support enabled
configure arguments: --user=www --group=www --prefix=/www/server/nginx --add-module=./ngx_devel_kit --add-module=./lua_nginx_module --add-module=./ngx_cache_purge --add-module=./nginx-sticky-module --with-compat --with-file-aio --with-threads --with-openssl=../openssl --with-pcre=pcre-8.43 --with-http_v2_module --with-stream --with-stream_ssl_module --with-stream_ssl_preread_module --with-http_stub_status_module --with-http_ssl_module --with-http_image_filter_module --with-http_gzip_static_module --with-http_gunzip_module --with-ipv6 --with-http_sub_module --with-http_flv_module --with-http_addition_module --with-http_realip_module --with-http_mp4_module --with-ld-opt=-Wl,-E --with-cc-opt=-Wno-error --with-ld-opt=-ljemalloc --with-http_dav_module --add-module=./nginx-dav-ext-module
```  


Ref: https://www.nange.cn/tls13-for-nginx.html  

配置文件中开启Chacha20:  
```
    ssl_protocols TLSv1.3 TLSv1.2;
    ssl_ciphers ECDHE-RSA-CHACHA20-POLY1305:ECDHE-RSA-AES128-GCM-SHA256;
    ssl_conf_command Options PrioritizeChaCha;
    ssl_conf_command Ciphersuites TLS_CHACHA20_POLY1305_SHA256;
    ssl_prefer_server_ciphers on;
    ssl_session_cache shared:SSL:10m;
    ssl_session_timeout 10m;
```
