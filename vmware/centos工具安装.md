
## 工具安装：
```base
yum -y install curl-devel expat-devel gettext-devel openssl-devel zlib-devel asciidoc wget
yum -y install gcc automake autoconf libtool make
```

## git安装
```base
cd /usr/local/src
wget http://mirrors.edge.kernel.org/pub/software/scm/git/git-2.32.0.tar.xz
tar -xf git-2.32.0.tar.xz
cd git-2.32.0/
make prefix=/usr/local/git all
make prefix=/usr/local/git install
echo "export PATH=$PATH:/usr/local/git/bin" >> /etc/profile
source /etc/profile
```
