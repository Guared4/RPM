# RPM

Размещаем свой RPM в своем репозитории

Домашнее задание

1) Создать свой RPM пакет (можно взять свое приложение, либо собрать, например,
Apache с определенными опциями).
2) Создать свой репозиторий и разместить там ранее собранный RPM.

Реализовать это все либо в Vagrant, либо развернуть у себя через Nginx и дать ссылку на репозиторий.

Задание выполнял в ОС  AlmaLinux 9.5

1. Для данного задания мне понадобятся следующие установленные пакеты:
[root@localhost ~]# yum install -y wget rpmdevtools rpm-build createrepo \
 yum-utils cmake gcc git nano

2.  Для примера возьму пакет Nginx и соберу его с дополнительным модулем ngx_broli

Загрузил SRPM пакет Nginx для дальнейшей работы над ним:

[root@localhost ~]# mkdir rpm && cd rpm
[root@localhost rpm]# yumdownloader --source nginx
enabling appstream-source repository
enabling baseos-source repository
enabling extras-source repository
AlmaLinux 9 - AppStream - Source                                                                                                                                    387 kB/s | 810 kB     00:02
AlmaLinux 9 - BaseOS - Source                                                                                                                                       199 kB/s | 269 kB     00:01
AlmaLinux 9 - Extras - Source                                                                                                                                       5.1 kB/s | 7.8 kB     00:01
nginx-1.20.1-20.el9.alma.1.src.rpm

При установке такого пакета в домашней директории создается дерево каталогов для сборки, далее поставил все зависимости для сборки пакета Nginx:

[root@localhost rpm]# sudo  rpm -Uvh nginx*.src.rpm
Updating / installing...
   1:nginx-2:1.20.1-20.el9.alma.1     warning: user mockbuild does not exist - using root

[root@localhost rpm]# yum-builddep nginx
enabling appstream-source repository
enabling baseos-source repository
enabling extras-source repository
Last metadata expiration check: 1:07:33 ago on Fri Dec  6 13:19:37 2024.
Package make-1:4.3-8.el9.x86_64 is already installed.
Package gcc-11.5.0-2.el9.alma.1.x86_64 is already installed.
Package systemd-252-46.el9_5.2.alma.1.x86_64 is already installed.
Package gnupg2-2.3.3-4.el9.x86_64 is already installed.
Dependencies resolved.
=======================

Installed:
  brotli-1.0.9-6.el9.x86_64                    brotli-devel-1.0.9-6.el9.x86_64                      bzip2-devel-1.0.8-8.el9.x86_64                 cairo-1.17.4-7.el9.x86_64
  dejavu-sans-fonts-2.37-18.el9.noarch         fontconfig-2.14.0-2.el9_1.x86_64                     fontconfig-devel-2.14.0-2.el9_1.x86_64         fonts-filesystem-1:2.0.5-7.el9.1.noarch
  freetype-2.10.4-9.el9.x86_64                 freetype-devel-2.10.4-9.el9.x86_64                   gd-2.3.2-3.el9.x86_64                          gd-devel-2.3.2-3.el9.x86_64
  glib2-devel-2.68.4-14.el9_4.1.x86_64         graphite2-1.3.14-9.el9.x86_64                        graphite2-devel-1.3.14-9.el9.x86_64            harfbuzz-2.7.4-10.el9.x86_64
  harfbuzz-devel-2.7.4-10.el9.x86_64           harfbuzz-icu-2.7.4-10.el9.x86_64                     jbigkit-libs-2.1-23.el9.x86_64                 langpacks-core-font-en-3.0-16.el9.noarch
  libICE-1.0.10-8.el9.x86_64                   libSM-1.2.3-10.el9.x86_64                            libX11-1.7.0-9.el9.x86_64                      libX11-common-1.7.0-9.el9.noarch
  libX11-devel-1.7.0-9.el9.x86_64              libX11-xcb-1.7.0-9.el9.x86_64                        libXau-1.0.9-8.el9.x86_64                      libXau-devel-1.0.9-8.el9.x86_64
  libXext-1.3.4-8.el9.x86_64                   libXpm-3.5.13-10.el9.x86_64                          libXpm-devel-3.5.13-10.el9.x86_64              libXrender-0.9.10-16.el9.x86_64
  libXt-1.2.0-6.el9.x86_64                     libblkid-devel-2.37.4-20.el9.x86_64                  libffi-devel-3.4.2-8.el9.x86_64                libgpg-error-devel-1.42-5.el9.x86_64
  libicu-67.1-9.el9.x86_64                     libicu-devel-67.1-9.el9.x86_64                       libjpeg-turbo-2.0.90-7.el9.x86_64              libjpeg-turbo-devel-2.0.90-7.el9.x86_64
  libmount-devel-2.37.4-20.el9.x86_64          libpng-2:1.6.37-12.el9.x86_64                        libpng-devel-2:1.6.37-12.el9.x86_64            libselinux-devel-3.6-1.el9.x86_64
  libsepol-devel-3.6-1.el9.x86_64              libtiff-4.4.0-13.el9.x86_64                          libtiff-devel-4.4.0-13.el9.x86_64              libwebp-1.2.0-8.el9_3.x86_64
  libwebp-devel-1.2.0-8.el9_3.x86_64           libxcb-1.13.1-9.el9.x86_64                           libxcb-devel-1.13.1-9.el9.x86_64               libxml2-devel-2.9.13-6.el9_4.x86_64
  libxslt-1.1.34-9.el9.x86_64                  libxslt-devel-1.1.34-9.el9.x86_64                    openssl-devel-1:3.2.2-6.el9_5.x86_64           pcre-cpp-8.44-4.el9.x86_64
  pcre-devel-8.44-4.el9.x86_64                 pcre-utf16-8.44-4.el9.x86_64                         pcre-utf32-8.44-4.el9.x86_64                   pcre2-devel-10.40-6.el9.x86_64
  pcre2-utf16-10.40-6.el9.x86_64               pcre2-utf32-10.40-6.el9.x86_64                       perl-AutoSplit-5.74-481.el9.noarch             perl-Benchmark-1.23-481.el9.noarch
  perl-CPAN-Meta-2.150010-460.el9.noarch       perl-CPAN-Meta-Requirements-2.140-461.el9.noarch     perl-CPAN-Meta-YAML-0.018-461.el9.noarch       perl-Devel-PPPort-3.62-4.el9.x86_64
  perl-Encode-Locale-1.05-21.el9.noarch        perl-ExtUtils-Command-2:7.60-3.el9.noarch            perl-ExtUtils-Constant-0.25-481.el9.noarch     perl-ExtUtils-Embed-1.35-481.el9.noarch
  perl-ExtUtils-Install-2.20-4.el9.noarch      perl-ExtUtils-MakeMaker-2:7.60-3.el9.noarch          perl-ExtUtils-Manifest-1:1.73-4.el9.noarch     perl-ExtUtils-ParseXS-1:3.40-460.el9.noarch
  perl-Fedora-VSP-0.001-23.el9.noarch          perl-File-Compare-1.100.600-481.el9.noarch           perl-File-Copy-2.34-481.el9.noarch             perl-I18N-Langinfo-0.19-481.el9.x86_64
  perl-JSON-PP-1:4.06-4.el9.noarch             perl-Math-BigInt-1:1.9998.18-460.el9.noarch          perl-Math-Complex-1.59-481.el9.noarch          perl-Test-Harness-1:3.42-461.el9.noarch
  perl-Time-HiRes-4:1.9764-462.el9.x86_64      perl-devel-4:5.32.1-481.el9.x86_64                   perl-doc-5.32.1-481.el9.noarch                 perl-generators-1.11-12.el9.noarch
  perl-locale-1.09-481.el9.noarch              perl-macros-4:5.32.1-481.el9.noarch                  perl-version-7:0.99.28-4.el9.x86_64            pixman-0.40.0-6.el9_3.x86_64
  python3-pyparsing-2.4.7-9.el9.noarch         sysprof-capture-devel-3.40.1-3.el9.x86_64            systemtap-sdt-devel-5.1-4.el9_5.x86_64         xml-common-0.6.3-58.el9.noarch
  xorg-x11-proto-devel-2024.1-1.el9.noarch     xz-devel-5.2.5-8.el9_0.x86_64                        zlib-devel-1.2.11-40.el9.x86_64

Complete!

Также нужно скачать исходный код модуля ngx_brotli — он
потребуется при сборке:

[root@localhost rpm]# cd /root
[root@localhost ~]# git clone --recurse-submodules -j8 \
https://github.com/google/ngx_brotli
Cloning into 'ngx_brotli'...
remote: Enumerating objects: 237, done.
remote: Counting objects: 100% (37/37), done.
remote: Compressing objects: 100% (16/16), done.
remote: Total 237 (delta 24), reused 21 (delta 21), pack-reused 200 (from 1)
Receiving objects: 100% (237/237), 79.51 KiB | 822.00 KiB/s, done.
Resolving deltas: 100% (114/114), done.
Submodule 'deps/brotli' (https://github.com/google/brotli.git) registered for path 'deps/brotli'
Cloning into '/root/ngx_brotli/deps/brotli'...
remote: Enumerating objects: 7690, done.
remote: Counting objects: 100% (1632/1632), done.
remote: Compressing objects: 100% (425/425), done.
remote: Total 7690 (delta 1329), reused 1254 (delta 1206), pack-reused 6058 (from 1)
Receiving objects: 100% (7690/7690), 36.51 MiB | 3.65 MiB/s, done.
Resolving deltas: 100% (5043/5043), done.
Submodule path 'deps/brotli': checked out 'ed738e842d2fbdf2d6459e39267a633c4a9b2f5d'

[root@localhost ~]# cd ngx_brotli/deps/brotli
[root@localhost brotli]# mkdir out && cd out

3. Собираю модуль ngx_brotli:

[root@localhost out]# cmake -DCMAKE_BUILD_TYPE=Release -DBUILD_SHARED_LIBS=OFF -DCMAKE_C_FLAGS="-Ofast -m64 -march=native -mtune=native -flto -funroll-loops -ffunction-sections -fdata-sections -Wl,--gc-sections" -DCMAKE_CXX_FLAGS="-Ofast -m64 -march=native -mtune=native -flto -funroll-loops -ffunction-sections -fdata-sections -Wl,--gc-sections" -DCMAKE_INSTALL_PREFIX=./installed ..
-- The C compiler identification is GNU 11.5.0
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working C compiler: /usr/bin/cc - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Build type is 'Release'
-- Performing Test BROTLI_EMSCRIPTEN
-- Performing Test BROTLI_EMSCRIPTEN - Failed
-- Compiler is not EMSCRIPTEN
-- Looking for log2
-- Looking for log2 - not found
-- Looking for log2
-- Looking for log2 - found
-- Configuring done (1.9s)
-- Generating done (0.1s)
CMake Warning:
  Manually-specified variables were not used by the project:

    CMAKE_CXX_FLAGS


-- Build files have been written to: /root/ngx_brotli/deps/brotli/out

[root@localhost out]# cmake --build . --config Release -j 2 --target brotlienc
[  3%] Building C object CMakeFiles/brotlicommon.dir/c/common/constants.c.o
[  6%] Building C object CMakeFiles/brotlicommon.dir/c/common/context.c.o
[ 10%] Building C object CMakeFiles/brotlicommon.dir/c/common/dictionary.c.o
[ 13%] Building C object CMakeFiles/brotlicommon.dir/c/common/platform.c.o
[ 17%] Building C object CMakeFiles/brotlicommon.dir/c/common/shared_dictionary.c.o
[ 20%] Building C object CMakeFiles/brotlicommon.dir/c/common/transform.c.o
[ 24%] Linking C static library libbrotlicommon.a
[ 24%] Built target brotlicommon
[ 27%] Building C object CMakeFiles/brotlienc.dir/c/enc/backward_references.c.o
[ 31%] Building C object CMakeFiles/brotlienc.dir/c/enc/backward_references_hq.c.o
[ 34%] Building C object CMakeFiles/brotlienc.dir/c/enc/bit_cost.c.o
[ 37%] Building C object CMakeFiles/brotlienc.dir/c/enc/block_splitter.c.o
[ 41%] Building C object CMakeFiles/brotlienc.dir/c/enc/brotli_bit_stream.c.o
[ 44%] Building C object CMakeFiles/brotlienc.dir/c/enc/cluster.c.o
[ 48%] Building C object CMakeFiles/brotlienc.dir/c/enc/command.c.o
[ 51%] Building C object CMakeFiles/brotlienc.dir/c/enc/compound_dictionary.c.o
[ 55%] Building C object CMakeFiles/brotlienc.dir/c/enc/compress_fragment.c.o
[ 58%] Building C object CMakeFiles/brotlienc.dir/c/enc/compress_fragment_two_pass.c.o
[ 62%] Building C object CMakeFiles/brotlienc.dir/c/enc/dictionary_hash.c.o
[ 65%] Building C object CMakeFiles/brotlienc.dir/c/enc/encode.c.o
[ 68%] Building C object CMakeFiles/brotlienc.dir/c/enc/encoder_dict.c.o
[ 72%] Building C object CMakeFiles/brotlienc.dir/c/enc/entropy_encode.c.o
[ 75%] Building C object CMakeFiles/brotlienc.dir/c/enc/fast_log.c.o
[ 79%] Building C object CMakeFiles/brotlienc.dir/c/enc/histogram.c.o
[ 82%] Building C object CMakeFiles/brotlienc.dir/c/enc/literal_cost.c.o
[ 86%] Building C object CMakeFiles/brotlienc.dir/c/enc/memory.c.o
[ 89%] Building C object CMakeFiles/brotlienc.dir/c/enc/metablock.c.o
[ 93%] Building C object CMakeFiles/brotlienc.dir/c/enc/static_dict.c.o
[ 96%] Building C object CMakeFiles/brotlienc.dir/c/enc/utf8_util.c.o
[100%] Linking C static library libbrotlienc.a
[100%] Built target brotlienc

[root@localhost out]# cd ../../../..

4. Нужно поправить сам spec файл, чтобы Nginx собирался с необходимыми опциями: нахожу секцию с параметрами configure (до условий if) и добавляю указание на модуль (не забываю указать завершающий обратный слэш), также добавил ещё модуль почты POP3/IMAP4/SMTP почтовый прокси-сервер:

перехожу в каталог rpmbuild/SPECS/ :
[root@localhost ~]# cd rpmbuild/SPECS/

открываю для редактирования файл nginx.spec:
[root@localhost SPECS]# nano nginx.spec

%build
# nginx does not utilize a standard configure script.  It has its own
# and the standard configure options cause the nginx configure script
# to error out.  This is is also the reason for the DESTDIR environment
# variable.
export DESTDIR=%{buildroot}
# So the perl module finds its symbols:
nginx_ldopts="$RPM_LD_FLAGS -Wl,-E"
if ! ./configure \
    --prefix=%{_datadir}/nginx \
    --sbin-path=%{_sbindir}/nginx \
    --modules-path=%{nginx_moduledir} \
    --conf-path=%{_sysconfdir}/nginx/nginx.conf \
    --error-log-path=%{_localstatedir}/log/nginx/error.log \
    --http-log-path=%{_localstatedir}/log/nginx/access.log \
    --http-client-body-temp-path=%{_localstatedir}/lib/nginx/tmp/client_body \
    --http-proxy-temp-path=%{_localstatedir}/lib/nginx/tmp/proxy \
    --http-fastcgi-temp-path=%{_localstatedir}/lib/nginx/tmp/fastcgi \
    --http-uwsgi-temp-path=%{_localstatedir}/lib/nginx/tmp/uwsgi \
    --http-scgi-temp-path=%{_localstatedir}/lib/nginx/tmp/scgi \
    --pid-path=/run/nginx.pid \
    --lock-path=/run/lock/subsys/nginx \
    --user=%{nginx_user} \
    --group=%{nginx_user} \
    --with-compat \
    --with-debug \
    --with-mail \                       <-------- 
    --with-mail=dynamic \               <--------
    --add-module=/root/ngx_brotli \     <--------
%if 0%{?with_aio}

5. Далее приступаю к сборке RPM пакета:

[root@localhost ~]# cd ~/rpmbuild/SPECS/

[root@localhost SPECS]# rpmbuild -ba nginx.spec -D 'debug_package %{nil}'
----------------------------
configuring additional modules
adding module in /root/ngx_brotli
 + ngx_brotli was configured
checking for PCRE library ... found
checking for PCRE JIT support ... found
checking for OpenSSL library ... found
checking for zlib library ... found
checking for libxslt ... found
checking for libexslt ... found
checking for GD library ... found
checking for GD WebP support ... found
checking for perl
 + perl version: This is perl 5, version 32, subversion 1 (v5.32.1) built for x86_64-linux-thread-multi
 + perl interpreter multiplicity found
creating objs/Makefile
------------------------------
Executing(%doc): /bin/sh -e /var/tmp/rpm-tmp.NlFpAd
+ umask 022
+ cd /root/rpmbuild/BUILD
+ cd nginx-1.20.1
+ DOCDIR=/root/rpmbuild/BUILDROOT/nginx-1.20.1-20.el9.alma.1.x86_64/usr/share/doc/nginx-core
+ export LC_ALL=C
+ LC_ALL=C
+ export DOCDIR
+ /usr/bin/mkdir -p /root/rpmbuild/BUILDROOT/nginx-1.20.1-20.el9.alma.1.x86_64/usr/share/doc/nginx-core
+ cp -pr CHANGES /root/rpmbuild/BUILDROOT/nginx-1.20.1-20.el9.alma.1.x86_64/usr/share/doc/nginx-core
+ cp -pr README /root/rpmbuild/BUILDROOT/nginx-1.20.1-20.el9.alma.1.x86_64/usr/share/doc/nginx-core
+ cp -pr README.dynamic /root/rpmbuild/BUILDROOT/nginx-1.20.1-20.el9.alma.1.x86_64/usr/share/doc/nginx-core
+ RPM_EC=0
++ jobs -p
+ exit 0
Executing(%license): /bin/sh -e /var/tmp/rpm-tmp.ANppoa
+ umask 022
+ cd /root/rpmbuild/BUILD
+ cd nginx-1.20.1
+ LICENSEDIR=/root/rpmbuild/BUILDROOT/nginx-1.20.1-20.el9.alma.1.x86_64/usr/share/licenses/nginx-core
+ export LC_ALL=C
+ LC_ALL=C
+ export LICENSEDIR
+ /usr/bin/mkdir -p /root/rpmbuild/BUILDROOT/nginx-1.20.1-20.el9.alma.1.x86_64/usr/share/licenses/nginx-core
+ cp -pr LICENSE /root/rpmbuild/BUILDROOT/nginx-1.20.1-20.el9.alma.1.x86_64/usr/share/licenses/nginx-core
+ RPM_EC=0
++ jobs -p
+ exit 0
---------------------------------
Executing(%clean): /bin/sh -e /var/tmp/rpm-tmp.0xKdtj
+ umask 022
+ cd /root/rpmbuild/BUILD
+ cd nginx-1.20.1
+ /usr/bin/rm -rf /root/rpmbuild/BUILDROOT/nginx-1.20.1-20.el9.alma.1.x86_64
+ RPM_EC=0
++ jobs -p
+ exit 0

6. Убедился, что пакеты создались:
[root@localhost ~]#  ll rpmbuild/RPMS/x86_64/
total 2000
-rw-r--r--. 1 root root   36247 Dec 10 16:18 nginx-1.20.1-20.el9.alma.1.x86_64.rpm
-rw-r--r--. 1 root root 1033100 Dec 10 16:18 nginx-core-1.20.1-20.el9.alma.1.x86_64.rpm
-rw-r--r--. 1 root root  759719 Dec 10 16:18 nginx-mod-devel-1.20.1-20.el9.alma.1.x86_64.rpm
-rw-r--r--. 1 root root   19370 Dec 10 16:18 nginx-mod-http-image-filter-1.20.1-20.el9.alma.1.x86_64.rpm
-rw-r--r--. 1 root root   31014 Dec 10 16:18 nginx-mod-http-perl-1.20.1-20.el9.alma.1.x86_64.rpm
-rw-r--r--. 1 root root   18169 Dec 10 16:18 nginx-mod-http-xslt-filter-1.20.1-20.el9.alma.1.x86_64.rpm
-rw-r--r--. 1 root root   53790 Dec 10 16:18 nginx-mod-mail-1.20.1-20.el9.alma.1.x86_64.rpm
-rw-r--r--. 1 root root   80450 Dec 10 16:18 nginx-mod-stream-1.20.1-20.el9.alma.1.x86_64.rpm

7. Копирую пакеты в общий каталог:
[root@localhost ~]# cp ~/rpmbuild/RPMS/noarch/* ~/rpmbuild/RPMS/x86_64/
[root@localhost ~]# cd ~/rpmbuild/RPMS/x86_64
[root@localhost x86_64]#

8. Теперь можно установить пакет и убедиться, что nginx работает:
[root@localhost x86_64]# yum localinstall *.rpm
----------------
Installed:
  almalinux-logos-httpd-90.5.1-1.1.el9.noarch                         nginx-2:1.20.1-20.el9.alma.1.x86_64                         nginx-all-modules-2:1.20.1-20.el9.alma.1.noarch
  nginx-core-2:1.20.1-20.el9.alma.1.x86_64                            nginx-filesystem-2:1.20.1-20.el9.alma.1.noarch              nginx-mod-devel-2:1.20.1-20.el9.alma.1.x86_64
  nginx-mod-http-image-filter-2:1.20.1-20.el9.alma.1.x86_64           nginx-mod-http-perl-2:1.20.1-20.el9.alma.1.x86_64           nginx-mod-http-xslt-filter-2:1.20.1-20.el9.alma.1.x86_64
  nginx-mod-mail-2:1.20.1-20.el9.alma.1.x86_64                        nginx-mod-stream-2:1.20.1-20.el9.alma.1.x86_64

Complete!

[root@localhost x86_64]# systemctl start nginx
[root@localhost x86_64]# systemctl status nginx
● nginx.service - The nginx HTTP and reverse proxy server
     Loaded: loaded (/usr/lib/systemd/system/nginx.service; disabled; preset: disabled)
     Active: active (running) since Tue 2024-12-10 16:40:14 MSK; 21s ago
    Process: 33608 ExecStartPre=/usr/bin/rm -f /run/nginx.pid (code=exited, status=0/SUCCESS)
    Process: 33609 ExecStartPre=/usr/sbin/nginx -t (code=exited, status=0/SUCCESS)
    Process: 33610 ExecStart=/usr/sbin/nginx (code=exited, status=0/SUCCESS)
   Main PID: 33611 (nginx)
      Tasks: 2 (limit: 11098)
     Memory: 6.3M
        CPU: 116ms
     CGroup: /system.slice/nginx.service
             ├─33611 "nginx: master process /usr/sbin/nginx"
             └─33612 "nginx: worker process"

Dec 10 16:40:13 localhost.localdomain systemd[1]: Starting The nginx HTTP and reverse proxy server...
Dec 10 16:40:14 localhost.localdomain nginx[33609]: nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
Dec 10 16:40:14 localhost.localdomain nginx[33609]: nginx: configuration file /etc/nginx/nginx.conf test is successful
Dec 10 16:40:14 localhost.localdomain systemd[1]: Started The nginx HTTP and reverse proxy server.

Далее буду использовать его для доступа к своему репозиторию.

9. Создаю свой репозиторий и размещаю там ранее собранный RPM
Теперь приступаю к созданию своего репозитория. Директория для статики у Nginx по умолчанию /usr/share/nginx/html. Создадю там каталог repo:

[root@localhost ~]# mkdir /usr/share/nginx/html/repo

Копирую туда свои собранные RPM-пакеты:
[root@localhost ~]# cp ~/rpmbuild/RPMS/x86_64/*.rpm /usr/share/nginx/html/repo/

Инициализирую репозиторий командой:
[root@localhost ~]# createrepo /usr/share/nginx/html/repo/
Directory walk started
Directory walk done - 10 packages
Temporary output repo path: /usr/share/nginx/html/repo/.repodata/
Preparing sqlite DBs
Pool started (with 5 workers)
Pool finished

Для прозрачности настроил в NGINX доступ к листингу каталога. В файле /etc/nginx/nginx.conf в блоке server добавил следующие директивы:

[root@localhost ~]# nano /etc/nginx/nginx.conf

-------------
  server {
        listen       80;
        listen       [::]:80;
        server_name  _;
        root         /usr/share/nginx/html;
        index index.html index.htm;
        autoindex on;

--------------

Проверил синтаксис и перезапустил NGINX:

[root@localhost ~]# nginx -t
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful

[root@localhost ~]# nginx -s reload

Теперь ради интереса посмотрел с помощью curl:
[root@localhost ~]# curl -a http://localhost/repo/
<html>
<head><title>Index of /repo/</title></head>
<body>
<h1>Index of /repo/</h1><hr><pre><a href="../">../</a>
<a href="repodata/">repodata/</a>                                          10-Dec-2024 13:44                   -
<a href="nginx-1.20.1-20.el9.alma.1.x86_64.rpm">nginx-1.20.1-20.el9.alma.1.x86_64.rpm</a>              10-Dec-2024 13:43               36247
<a href="nginx-all-modules-1.20.1-20.el9.alma.1.noarch.rpm">nginx-all-modules-1.20.1-20.el9.alma.1.noarch.rpm</a>  10-Dec-2024 13:43                7357
<a href="nginx-core-1.20.1-20.el9.alma.1.x86_64.rpm">nginx-core-1.20.1-20.el9.alma.1.x86_64.rpm</a>         10-Dec-2024 13:43             1033100
<a href="nginx-filesystem-1.20.1-20.el9.alma.1.noarch.rpm">nginx-filesystem-1.20.1-20.el9.alma.1.noarch.rpm</a>   10-Dec-2024 13:43                8440
<a href="nginx-mod-devel-1.20.1-20.el9.alma.1.x86_64.rpm">nginx-mod-devel-1.20.1-20.el9.alma.1.x86_64.rpm</a>    10-Dec-2024 13:43              759719
<a href="nginx-mod-http-image-filter-1.20.1-20.el9.alma.1.x86_64.rpm">nginx-mod-http-image-filter-1.20.1-20.el9.alma...&gt;</a> 10-Dec-2024 13:43               19370
<a href="nginx-mod-http-perl-1.20.1-20.el9.alma.1.x86_64.rpm">nginx-mod-http-perl-1.20.1-20.el9.alma.1.x86_64..&gt;</a> 10-Dec-2024 13:43               31014
<a href="nginx-mod-http-xslt-filter-1.20.1-20.el9.alma.1.x86_64.rpm">nginx-mod-http-xslt-filter-1.20.1-20.el9.alma.1..&gt;</a> 10-Dec-2024 13:43               18169
<a href="nginx-mod-mail-1.20.1-20.el9.alma.1.x86_64.rpm">nginx-mod-mail-1.20.1-20.el9.alma.1.x86_64.rpm</a>     10-Dec-2024 13:43               53790
<a href="nginx-mod-stream-1.20.1-20.el9.alma.1.x86_64.rpm">nginx-mod-stream-1.20.1-20.el9.alma.1.x86_64.rpm</a>   10-Dec-2024 13:43               80450
</pre><hr></body>
</html>

10.  Все готово для того, чтобы протестировать репозиторий.
Добавил его в /etc/yum.repos.d:

[root@localhost ~]#  cat >> /etc/yum.repos.d/otus.repo << EOF
[otus]
name=otus-linux
baseurl=http://localhost/repo
gpgcheck=0
enabled=1
EOF

Убедился, что репозиторий подключился и посмотрел, что в нем есть:

[root@localhost ~]# yum repolist enabled | grep otus
otus                             otus-linux

Добавил пакет в свой репозиторий:

[root@localhost ~]# cd /usr/share/nginx/html/repo/

[root@localhost repo]# wget https://repo.percona.com/yum/percona-release-latest.noarch.rpm

--2024-12-10 16:53:29--  https://repo.percona.com/yum/percona-release-latest.noarch.rpm
Resolving repo.percona.com (repo.percona.com)... 49.12.125.205, 2a01:4f8:242:5792::2
Connecting to repo.percona.com (repo.percona.com)|49.12.125.205|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 27900 (27K) [application/x-redhat-package-manager]
Saving to: ‘percona-release-latest.noarch.rpm’

percona-release-latest.noarch.rpm                100%[==========================================================================================================>]  27.25K  --.-KB/s    in 0.05s

2024-12-10 16:53:30 (592 KB/s) - ‘percona-release-latest.noarch.rpm’ saved [27900/27900]

11. Обновил список пакетов в репозитории:

[root@localhost repo]# createrepo /usr/share/nginx/html/repo/
Directory walk started
Directory walk done - 11 packages
Temporary output repo path: /usr/share/nginx/html/repo/.repodata/
Preparing sqlite DBs
Pool started (with 5 workers)
Pool finished

[root@localhost repo]# yum makecache
AlmaLinux 9 - AppStream                                                                                                                                             5.9 kB/s | 4.2 kB     00:00
AlmaLinux 9 - BaseOS                                                                                                                                                7.5 kB/s | 3.8 kB     00:00
AlmaLinux 9 - Extras                                                                                                                                                6.1 kB/s | 3.8 kB     00:00
otus-linux                                                                                                                                                          826 kB/s | 7.2 kB     00:00
Metadata cache created.

[root@localhost repo]# yum list | grep otus
percona-release.noarch                               1.0-29                              otus

12. Так как Nginx уже стоит, установил репозиторий percona-release:

[root@localhost repo]# yum install -y percona-release.noarch
Last metadata expiration check: 0:01:27 ago on Tue Dec 10 16:55:01 2024.
Dependencies resolved.
====================================================================================================================================================================================================
 Package                                               Architecture                                 Version                                        Repository                                  Size
====================================================================================================================================================================================================
Installing:
 percona-release                                       noarch                                       1.0-29                                         otus                                        27 k

Transaction Summary
====================================================================================================================================================================================================
Install  1 Package

Total download size: 27 k
Installed size: 48 k
Downloading Packages:
percona-release-latest.noarch.rpm                                                                                                                                   4.5 MB/s |  27 kB     00:00
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Total                                                                                                                                                               2.7 MB/s |  27 kB     00:00
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                                                                                                                                            1/1
  Installing       : percona-release-1.0-29.noarch                                                                                                                                              1/1
  Running scriptlet: percona-release-1.0-29.noarch                                                                                                                                              1/1
* Enabling the Percona Release repository
<*> All done!
* Enabling the Percona Telemetry repository
<*> All done!
* Enabling the PMM2 Client repository
<*> All done!
The percona-release package now contains a percona-release script that can enable additional repositories for our newer products.

Note: currently there are no repositories that contain Percona products or distributions enabled. We recommend you to enable Percona Distribution repositories instead of individual product repositories, because with the Distribution you will get not only the database itself but also a set of other componets that will help you work with your database.

For example, to enable the Percona Distribution for MySQL 8.0 repository use:

  percona-release setup pdps8.0

Note: To avoid conflicts with older product versions, the percona-release setup command may disable our original repository for some products.

For more information, please visit:
  https://docs.percona.com/percona-software-repositories/percona-release.html


  Verifying        : percona-release-1.0-29.noarch                                                                                                                                              1/1

Installed:
  percona-release-1.0-29.noarch

Complete!

___________
end 
