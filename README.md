# nginxtls13
Enable TLS1.3 and CHACHA20 support for Nginx

1.22可以直接启用TLS1.3 chacha20  
```  
    ssl_protocols TLSv1.3 TLSv1.2;
    ssl_ciphers ECDHE-RSA-CHACHA20-POLY1305:ECDHE-RSA-AES128-GCM-SHA256;
    ssl_conf_command Options PrioritizeChaCha;
    ssl_conf_command Ciphersuites TLS_CHACHA20_POLY1305_SHA256;
    ssl_prefer_server_ciphers on;
    ssl_session_cache shared:SSL:10m;
```  
