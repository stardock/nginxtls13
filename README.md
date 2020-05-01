# nginxtls13
Enable TLS1.3 and CHACHA20 support for Nginx

## 前言  

* 要先完成 openssltls13  
* 本例使用Centos7, kvm虚拟化  
* 请使用宝塔面板解决依赖问题：安装宝塔面板 > 快速安装nginx 1.17  
* 请每次执行一条，遇到错误请停下来排错


# 懒人版  

ssh输入`service nginx stop`  
将 /www/server/nginx/sbin 下的nginx文件改个名字  
把本库中的 nginx 贴过去  
`service nginx start`  


# 铁人版  

cd  
git clone https://github.com/stardock/nginxtls13  


## 下载 Nginx 的源码  

```  
cd 
wget http://nginx.org/download/nginx-1.17.2.tar.gz
tar zxf nginx-1.17.2.tar.gz
mv nginx-1.17.2 nginx
```  

## 给 Nginx 打补丁  

 * 补丁来自：https://github.com/kn007/patch  

 * nginx 补丁1  

     添加SPDY支持。  
     添加HTTP2 HPACK编码支持。  
     添加动态TLS记录支持。  
    
```  
cd
git clone https://github.com/stardock/patch nginx-patch
cd nginx
patch -p1 < ../nginx-patch/nginx.patch
```  
 * nginx补丁2  
   添加TLS1.3加密算法选择以及开启CHACHA20支持  
```  
cd
git clone https://github.com/stardock/nginx-tls13-chacha20-patch
mv nginx-tls13-chacha20-patch nginx-p2
cd nginx
patch -p1 < ../nginx-p2/nginx_tls13_chacha20_1_17_2.patch 	
```  
cd  
cd nginxtls13  
tar xvzf module.tar.tgz  
cd  


## 编译
```  
./configure \
--user=www --group=www --prefix=/www/server/nginx --with-openssl=../openssl \
--add-module=../nginxtls13/ngx_devel_kit --add-module=../nginxtls13/lua_nginx_module \
--add-module=../nginxtls13/ngx_cache_purge --add-module=../nginxtls13/nginx-sticky-module \
--add-module=../nginxtls13/ngx-pagespeed --add-module=../nginxtls13/ngx_brotli \
--with-http_v2_module --with-http_ssl_module --with-http_stub_status_module \
--with-compat --with-file-aio --with-threads \
--with-http_image_filter_module --with-http_gzip_static_module \
--with-http_gunzip_module --with-stream --with-stream_ssl_preread_module \
--with-stream_ssl_module --with-http_sub_module --with-http_flv_module \
--with-http_addition_module --with-http_realip_module \
--with-http_mp4_module --with-ld-opt=-Wl,-E \
--with-openssl-opt='enable-tls1_3 enable-weak-ssl-ciphers'

make
```  

Brotli:  
`https://github.com/google/ngx_brotli`  

## 检查结果
cd objs  
./nginx -V  
```  
[root@vml6xnph objs]# ./nginx -V
nginx version: nginx/1.17.2
built by gcc 4.8.5 20150623 (Red Hat 4.8.5-39) (GCC)
built with OpenSSL 1.1.1  11 Sep 2018
TLS SNI support enabled
configure arguments: --user=www --group=www --prefix=/www/server/nginx --with-openssl=../openssl --add-module=/home
/www/ngx_devel_kit --add-module=/home/www/lua_nginx_module --add-module=/home/www/ngx_cache_purge --add-module=/home/www/nginx-
sticky-module --add-module=/home/www/ngx-pagespeed --with-http_v2_module --with-http_ssl_module --with-http_stub_status_module 
--with-http_ssl_module --with-http_v2_module --with-http_image_filter_module --with-http_gzip_static_module --with-
http_gunzip_module --with-stream --with-stream_ssl_module --with-ipv6 --with-http_sub_module --with-http_flv_module --with-
http_addition_module --with-http_realip_module --with-http_mp4_module --with-ld-opt=-Wl,-E --with-openssl-opt='enable-tls1_3 
enable-weak-ssl-ciphers'
```  
ldd nginx  
```  
[root@vml6xnph objs]# ldd nginx
        linux-vdso.so.1 =>  (0x00007fff93174000)
        libdl.so.2 => /lib64/libdl.so.2 (0x00007f8196b30000)
        libpthread.so.0 => /lib64/libpthread.so.0 (0x00007f8196914000)
        libcrypt.so.1 => /lib64/libcrypt.so.1 (0x00007f81966dd000)
        liblua-5.1.so => /lib64/liblua-5.1.so (0x00007f81964af000)
        libm.so.6 => /lib64/libm.so.6 (0x00007f81961ad000)
        libstdc++.so.6 => /lib64/libstdc++.so.6 (0x00007f8195ea6000)
        librt.so.1 => /lib64/librt.so.1 (0x00007f8195c9e000)
        libuuid.so.1 => /lib64/libuuid.so.1 (0x00007f8195a99000)
        libpcre.so.1 => /lib64/libpcre.so.1 (0x00007f8195837000)
        libz.so.1 => /lib64/libz.so.1 (0x00007f8195621000)
        libgd.so.2 => /lib64/libgd.so.2 (0x00007f81953da000)
        libgcc_s.so.1 => /lib64/libgcc_s.so.1 (0x00007f81951c4000)
        libc.so.6 => /lib64/libc.so.6 (0x00007f8194df6000)
        /lib64/ld-linux-x86-64.so.2 (0x00007f8196d34000)
        libfreebl3.so => /lib64/libfreebl3.so (0x00007f8194bf3000)
        libXpm.so.4 => /lib64/libXpm.so.4 (0x00007f81949e1000)
        libX11.so.6 => /lib64/libX11.so.6 (0x00007f81946a3000)
        libjpeg.so.62 => /lib64/libjpeg.so.62 (0x00007f819444e000)
        libfontconfig.so.1 => /lib64/libfontconfig.so.1 (0x00007f819420c000)
        libfreetype.so.6 => /lib64/libfreetype.so.6 (0x00007f8193f4d000)
        libpng15.so.15 => /lib64/libpng15.so.15 (0x00007f8193d22000)
        libxcb.so.1 => /lib64/libxcb.so.1 (0x00007f8193afa000)
        libexpat.so.1 => /lib64/libexpat.so.1 (0x00007f81938d0000)
        libbz2.so.1 => /lib64/libbz2.so.1 (0x00007f81936c0000)
        libXau.so.6 => /lib64/libXau.so.6 (0x00007f81934bc000)
```  

把nginx文件拷贝过去即可  

# 最后的步骤  

* 将网站的ssl配置文件修改如下
```  
ssl_protocols TLSv1.2 TLSv1.3;
ssl_ciphers ECDHE-RSA-CHACHA20-POLY1305:ECDHE-RSA-AES128-GCM-SHA256;
ssl_ciphers_tls13 TLS_CHACHA20_POLY1305_SHA256:TLS_AES_128_GCM_SHA256;
ssl_prefer_server_ciphers on;
ssl_prefer_chacha20 on;
```  

Reference:  
给Nginx打CHACHA20补丁: https://www.v2ex.com/t/546906  
