

https://www.nange.cn/tls13-for-nginx.html








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



curl https://raw.githubusercontent.com/kn007/patch/master/nginx.patch | patch -p1

curl https://raw.githubusercontent.com/kn007/patch/master/use_openssl_md5_sha1.patch | patch -p1

curl https://raw.githubusercontent.com/kn007/patch/master/Enable_BoringSSL_OCSP.patch | patch -p1
