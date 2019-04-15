from local/c7-systemd

RUN yum update -y && \
    yum install expect wget yum-utils -y && \
    yum install epel-release -y && \
    wget http://rpms.remirepo.net/enterprise/remi-release-7.rpm && \
    rpm -Uvh remi-release-7.rpm && \
    yum-config-manager --enable remi-php72 -y && \
    yum --enablerepo=remi,remi-php72 install php-fpm php-common -y && \
    yum --enablerepo=remi,remi-php72 install php-opcache php-pecl-apcu php-cli php-bcmath php-imap \
    php-xmlrpc php-soap  php-zip php-pear php-pdo php-mysqlnd php-pecl-redis php-pecl-memcache php-pecl-memcached php-gd php-mbstring php-mcrypt php-xml -y && \
    yum install deltarpm unzip gcc gcc-c++ make pcre-devel openssl-devel.x86_64 nano -y

WORKDIR /root/make_nginx
RUN wget -c https://nginx.org/download/nginx-1.13.2.tar.gz && \
    tar -xzvf nginx-1.13.2.tar.gz && \
    ln -s /root/make_nginx/nginx-1.13.2 /root/make_nginx/nginx-1.13.2-stable && \
    wget -c https://github.com/pagespeed/ngx_pagespeed/archive/v1.12.34.2-stable.zip && \
    unzip v1.12.34.2-stable.zip && \
    ln -s /root/make_nginx/incubator-pagespeed-ngx-1.12.34.2-stable /root/make_nginx/ngx_pagespeed-1.12.34.2-stable && \
    cd incubator-pagespeed-ngx-1.12.34.2-stable && \
    wget -c https://dl.google.com/dl/page-speed/psol/1.12.34.2-x64.tar.gz && \
    tar -xvzf 1.12.34.2-x64.tar.gz
WORKDIR  /root/make_nginx/nginx-1.13.2
    RUN ./configure \
    --with-http_ssl_module \
    --with-http_v2_module \
    --conf-path=/etc/nginx/nginx.conf \
    --with-http_stub_status_module \
    --without-mail_pop3_module \
    --without-mail_imap_module \
    --without-mail_smtp_module \
    --add-module=$HOME/make_nginx/ngx_pagespeed-1.12.34.2-stable/ ${PS_NGX_EXTRA_FLAGS}
   
    RUN make && make install && \
    ln -s /usr/local/nginx/sbin/nginx /usr/sbin/nginx && \ 
    ln -s /usr/local/nginx/conf /etc/nginx
    
COPY nginx.service /lib/systemd/system/nginx.service
RUN systemctl enable nginx
RUN systemctl enable php-fpm.service
RUN useradd -u 1010 -U  wordpress
COPY www.conf /etc/php-fpm.d/www.conf
COPY php.ini /etc/php.ini
RUN mkdir /etc/letsencrypt
COPY nginx.conf /etc/nginx/nginx.conf
COPY cache.conf /etc/nginx/conf.d/cache.conf 
RUN mkdir /etc/nginx/mod
COPY loadcache.conf /etc/nginx/mod/loadcache.conf
RUN mkdir -p /var/log/nginx/












