# nginx
Forked to setup a local test env for Linux with a customized openssl version.

To build openssl separately:

```
mkdir github
cd github
git clone git://git.openssl.org/openssl.git
pushd openssl
# Prepare openssl source, we use 1.0.2n stable version here
git checkout -b 1.0.2n tags/OpenSSL_1_0_2n
./Configure linux-x86_64 -shared
make
popd
```

To build nginx with custom openssl from souorce code above:

* RHEL/CentOS
```
yum install -y gd-devel GeoIP-devel gperftools-devel libxslt-devel pcre-devel perl-ExtUtils-Embed redhat-rpm-config zlib-devel
```

```
mkdir github
pushd github
git clone -b OpenSSL_1_0_2-stable --depth 1 git://git.openssl.org/openssl.git
git clone -b stable-1.14-linux --depth 1 https://github.com/matr1xc0in/nginx.git
pushd nginx
./auto/configure --prefix=/usr/share/nginx \
--sbin-path=/usr/sbin/nginx \
--modules-path=/usr/lib64/nginx/modules \
--conf-path=/etc/nginx/nginx.conf \
--error-log-path=/var/log/nginx/error.log \
--http-log-path=/var/log/nginx/access.log \
--http-client-body-temp-path=/var/lib/nginx/tmp/client_body \
--http-proxy-temp-path=/var/lib/nginx/tmp/proxy \
--http-fastcgi-temp-path=/var/lib/nginx/tmp/fastcgi \
--http-uwsgi-temp-path=/var/lib/nginx/tmp/uwsgi \
--http-scgi-temp-path=/var/lib/nginx/tmp/scgi \
--pid-path=/run/nginx.pid \
--lock-path=/run/lock/subsys/nginx \
--user=nginx --group=nginx \
--with-file-aio \
--with-http_ssl_module \
--with-http_v2_module \
--with-http_realip_module \
--with-http_addition_module \
--with-http_xslt_module=dynamic \
--with-http_image_filter_module=dynamic \
--with-http_geoip_module=dynamic \
--with-http_sub_module \
--with-http_dav_module \
--with-http_flv_module \
--with-http_mp4_module \
--with-http_gunzip_module \
--with-http_gzip_static_module \
--with-http_random_index_module \
--with-http_secure_link_module \
--with-http_degradation_module \
--with-http_slice_module \
--with-http_stub_status_module \
--with-http_perl_module=dynamic \
--with-openssl=$(pwd)/../openssl \
--with-openssl-opt='-shared' \
--with-mail=dynamic \
--with-mail_ssl_module \
--with-pcre \
--with-pcre-jit \
--with-stream=dynamic \
--with-stream_ssl_module \
--with-google_perftools_module \
--with-debug \
--with-cc-opt='-O2 -g -pipe -Wall -Wp,-D_FORTIFY_SOURCE=2 -fexceptions -fstack-protector-strong --param=ssp-buffer-size=4 -grecord-gcc-switches -specs=/usr/lib/rpm/redhat/redhat-hardened-cc1 -m64 -mtune=generic' \
--with-ld-opt='-Wl,-z,relro -specs=/usr/lib/rpm/redhat/redhat-hardened-ld -Wl,-E'
```
