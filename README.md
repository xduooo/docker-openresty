# docker-openresty

## [openresty/docker-openresty](https://github.com/openresty/docker-openresty) based

Supported tags and respective Dockerfile links
====================

- [`Debian`, `from source`, (*buster/Dockerfile*)](buster/Dockerfile)
- [`Debian`, `DEB based`, (*buster/Dockerfile.deb*)](buster/Dockerfile.deb)
- [`CentOS`, `RPM based`, (*centos/Dockerfile.rpm*)](centos/Dockerfile.rpm)
- [`Alpine`, `from source`, (*alpine/Dockerfile*)](alpine/Dockerfile)

Running
====================

with Host network (Linux only)
```
docker run --detach \
  --name=openresty \
  --network=host \
  --restart always \
  xduooo/openresty:1.17.8.2
```

or with Bridge
```
docker run --detach \
  --name=openresty \
  --publish 80:80 --publish 443:443 \
  --restart always \
  xduooo/openresty:1.17.8.2
```

some settings that may be used
```
  --volume /opt/openresty/conf/nginx.conf:/usr/local/openresty/nginx/conf/nginx.conf \
  --volume /opt/openresty/conf/conf.d:/etc/nginx/conf.d \
  --volume /opt/openresty/logs:/usr/local/openresty/nginx/logs \
  --volume /opt/openresty/vhosts:/opt/openresty/vhosts \
```


Building
====================

Normal:

```
docker build -f buster/Dockerfile -t xduooo/openresty:1.17.8.2 .
```

With ARGs:

```
docker build -f buster/Dockerfile -t xduooo/openresty:1.17.8.2 --build-arg RESTY_DEB_FLAVOR="-debug" .
```


Building (Debian Buster, DEB based)
====================

| Key | Default | Description |
:----- | :-----: |:----------- |
|RESTY_IMAGE_BASE  | "xduooo/debian" | The Debian Docker image base to build `FROM`. |
|RESTY_IMAGE_TAG   | "buster-slim" | The Debian Docker image tag to build `FROM`. |
|RESTY_DEB_FLAVOR  | "" | The `openresty` package flavor to use.  Possibly `"-debug"` or `"-valgrind"`. |
|RESTY_DEB_VERSION | "=1.17.8.2-1~buster1" | The Debian package version to use, with `=` prepended. |


Building (CentOS, RPM based)
====================

| Key | Default | Description |
:----- | :-----: |:----------- |
|RESTY_IMAGE_BASE | "xduooo/centos" | The CentOS Docker image base to build `FROM`. |
|RESTY_IMAGE_TAG | "8" | The CentOS Docker image tag to build `FROM`. |
|RESTY_YUM_REPO | "https://openresty.org/package/centos/openresty.repo" | URL for the OpenResty YUM Repository. |
|RESTY_RPM_FLAVOR | "" | The `openresty` package flavor to use.  Possibly `"-debug"` or `"-valgrind"`. |
|RESTY_RPM_VERSION | "1.17.8.2-1" | The `openresty` package version to install. |
|RESTY_RPM_DIST | "el8" | The `openresty` package distribution to install. |
|RESTY_RPM_ARCH | "x86_64" | The `openresty` package architecture to install. |


Building (Debian/Alpine, from source)
======================

| Key | Default | Description |
:----- | :-----: |:----------- |
|RESTY_IMAGE_BASE | "xduooo/debian" / "xduooo/alpine" | The Debian or Alpine Docker image base to build `FROM`. |
|RESTY_IMAGE_TAG  | "buster-slim" / "3.12" | The Debian or Alpine Docker image tag to build `FROM`. |
|RESTY_VERSION | 1.17.8.2 | The version of OpenResty to use. |
|RESTY_OPENSSL_VERSION | 1.1.1g | The version of OpenSSL to use. |
|RESTY_OPENSSL_PATCH_VERSION | 1.1.1f | The version of OpenSSL to use when patching. |
|RESTY_OPENSSL_URL_BASE | https://www.openssl.org/source | The base of the URL to download OpenSSL from. |
|RESTY_PCRE_VERSION | 8.44 | The version of PCRE to use. |
|RESTY_J | 1 | Sets the parallelism level (-jN) for the builds. |
|RESTY_CONFIG_OPTIONS | "--with-compat --with-file-aio --with-http_addition_module --with-http_auth_request_module --with-http_dav_module --with-http_flv_module --with-http_geoip_module=dynamic --with-http_gunzip_module --with-http_gzip_static_module --with-http_image_filter_module=dynamic --with-http_mp4_module --with-http_perl_module=dynamic --with-http_random_index_module --with-http_realip_module --with-http_secure_link_module --with-http_slice_module --with-http_ssl_module --with-http_stub_status_module --with-http_sub_module --with-http_v2_module --with-http_xslt_module=dynamic --with-ipv6 --with-mail --with-mail_ssl_module --with-md5-asm --with-pcre-jit --with-sha1-asm --with-stream --with-stream_ssl_module --with-threads" | Options to pass to OpenResty's `./configure` script. |
|RESTY_LUAJIT_OPTIONS | "--with-luajit-xcflags='-DLUAJIT_NUMMODE=2 -DLUAJIT_ENABLE_LUA52COMPAT'" | Options to tweak LuaJIT. |
|RESTY_CONFIG_OPTIONS_MORE | "" | More options to pass to OpenResty's `./configure` script. |
|RESTY_ADD_PACKAGE_BUILDDEPS | "" | Additional packages to install with package manager required by build only (removed after installation) |
|RESTY_ADD_PACKAGE_RUNDEPS | "" | Additional packages to install with package manager required at runtime (not removed after installation) |
|RESTY_EVAL_PRE_CONFIGURE | "" | Command(s) to run prior to executing OpenResty's `./configure` script. (this can be used to clone a github repo of an extension you want to add to OpenResty, for example.  In that case, dont forget to add the appropriate argument to the RESTY_CONFIG_OPTIONS_MORE argument as described above). |
|RESTY_EVAL_POST_MAKE | "" | Command(s) to run after running make install.  |

These built-from-source flavors include the following modules by default, but one can easily increase or decrease that with the custom build options above:

 * file-aio
 * http_addition_module
 * http_auth_request_module
 * http_dav_module
 * http_flv_module
 * http_geoip_module=dynamic
 * http_gunzip_module
 * http_gzip_static_module
 * http_image_filter_module=dynamic
 * http_mp4_module
 * http_random_index_module
 * http_realip_module
 * http_secure_link_module
 * http_slice_module
 * http_ssl_module
 * http_stub_status_module
 * http_sub_module
 * http_v2_module
 * http_xslt_module=dynamic
 * ipv6
 * mail
 * mail_ssl_module
 * md5-asm
 * pcre-jit
 * sha1-asm
 * stream
 * stream_ssl_module
 * threads


Copyright & License
===================

`docker-openresty` is licensed under the 2-clause BSD license.

Copyright (c) 2017-2020, Evan Wies <evan@neomantra.net>.

This module is licensed under the terms of the BSD license.

Redistribution and use in source and binary forms, with or without modification, are permitted provided that the following conditions are met:

* Redistributions of source code must retain the above copyright notice, this list of conditions and the following disclaimer.
* Redistributions in binary form must reproduce the above copyright notice, this list of conditions and the following disclaimer in the documentation and/or other materials provided with the distribution.

THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT HOLDER OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
