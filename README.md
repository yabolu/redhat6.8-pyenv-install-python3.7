### redhat6.8 pyenv 安装python3.7

##### 1. Requirements

```
sudo yum install zlib-devel bzip2 bzip2-devel readline-devel sqlite sqlite-devel \
openssl-devel xz xz-devel libffi-devel
```

```
如果遇到内网无法连接外网的情况, 那就只能使用本地yum源了:
[root@db media]# mkdir /iso
[root@db media]# mount /dev/sr0 /iso
mount: block device /dev/sr0 is write-protected, mounting read-only
[root@db ~]# vi /etc/yum.repos.d/ol5.repo
[root@db ~]# cat /etc/yum.repos.d/ol5.repo 
[OL5]
name=OL5
enabled=1
gpgcheck=0
baseurl=file:///iso/Server/
[root@db media]# yum -y install gcc
```

##### 2. 安装openssl

如果出现一下错误, 需要重新安装最新的openssl, 因为yum 安装的版本不是最新的, 所以建议源码安装

`ERROR: The Python ssl extension was not compled. Missing the OpenSSL lib?`

```
tar zxvf openssl-1.1.1.tar.gz
cd openssl-1.1.1
./config --prefix=/usr/local/openssl --openssldir=/usr/local/openssl shared zlib
make
make test
make install

mv /usr/bin/openssl /root/
ln -s /usr/local/openssl/bin/openssl /usr/bin/openssl

ln -s /usr/local/openssl/lib/libcrypto.so.1.1 /usr/lib64/libcrypto.so.1.1
ln -s /usr/local/openssl/lib/libssl.so.1.1 /usr/lib64/libssl.so.1.1
```

##### 3. 安装pyenv

```
# 或者直接去github 下载, 解压到~/.pyenv
git clone https://github.com/pyenv/pyenv.git ~/.pyenv
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bash_profile
echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bash_profile
echo -e 'if command -v pyenv 1>/dev/null 2>&1; then\n  eval "$(pyenv init -)"\nfi' >> ~/.bash_profile
source ~/.bash_profile
```

##### 4. 安装pyenv-virtualenv

```
###或者去github下载直接解压到$(pyenv root)/plugins/
git clone https://github.com/pyenv/pyenv-virtualenv.git $(pyenv root)/plugins/pyenv-virtualenv
echo 'eval "$(pyenv virtualenv-init -)"' >> ~/.bash_profile
source ~/.bash_profile
```

##### 5. 安装python3.7.0

```
cd ~/.pyenv
mkdir cache
cd cache 
将下载的python3.7.0直接拷贝到当前目录
//在当前目录执行
LD_RUN_PATH="/usr/local/openssl/lib" LDFLAGS="-L/usr/local/openssl/lib" CPPFLAGS="-I/usr/local/openssl/include" CFLAGS="-I/usr/local/openssl/include" CONFIGURE_OPTS="--with-openssl=/usr/local/openssl" pyenv install 3.7.0 -v
```

##### 6. 创建python3.7.0虚拟环境

```
//创建虚拟环境
pyenv virtualenv 3.7.0 venv370
//激活
pyenv activate venv370

```

##### 7. 安装python3.7安装包

```
//按照文档给出的顺序安装
tar zxvf 包名
cd 包名
python setup.py install

//具体包的安装顺序参考文档python3包安装顺序
```

##### 8. 安装rpm安装包(已下载)

```
rpm -i xorg-x11-fonts-75dpi-7.5-20.fc29.noarch.rpm
rpm -i wkhtmltox-0.12.5-1.centos6.x86_64.rpm
rpm -i libpng12-0-1.2.44-7.1.x86_64.rpm
```




